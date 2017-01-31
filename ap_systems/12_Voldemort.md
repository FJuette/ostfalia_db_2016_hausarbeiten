## 3.4 Voldemort
Wie [SKL+12] hier und im Folgenden zu entnehmen ist, ist Voldemort ein horizontal skalierendes,
verteiltes Datenbanksystem, das die Datens�tze als Schl�ssel-Wert Paare speichert. Ein Cluster
dieses Systems kann dabei mehrere Knoten enthalten, die jeweils �ber eine eigene ID verf�gen.
Die Knoten enthalten mehrere Stores, die Tabellen in relationalen Datenbanken entsprechen.
Jeder Store enth�lt die folgenden konfigurierbaren Parameter:

�	Replication factor (N): Anzahl der Knoten, in denen jeder Schl�ssel-Wert Tupel repliziert
	ist.
�	Required reads (R): Anzahl der Knoten, die w�hrend eines Lesezugriffs parallel gelesen 
	werden, bevor ein Erfolg der Operation festgestellt wird.
�	Required writes (W): Anzahl der Knoten, die Voldemort blockiert, bis der Schreibzugriff 
	erfolgreich ist.
�	Key/value serialization and compression: Voldemort unterst�tzt unterschiedliche 
	Serialisations-Schemas f�r Schl�ssel und Werte. So sind mit Voldemort pro-Tupel
	Kompression m�glich, als auch selbsterstellte bin�re JSON-Formate. Serialisation und
	Kompression werden komplett clientseitig behandelt, so dass sich der Server nur noch
	um rohe bin�re Datenarrays k�mmern muss.
�	Storage engine type: Voldemort unterst�tzt die Lese/Schreib Storage Engine Formate
	Berkeley DB, Java Edition und MySQL. Au�erdem sind selbsterstellte Formate m�glich, f�r die
	aber nur Lesezugriff m�glich ist.

Die Cluster Topologie und die Speicherdefinitionen sind in jedem Knoten als Metadaten hinterlegt. [Pro17]
ist dazu erg�nzend zu entnehmen, dass hierdurch ein separater Cache �berfl�ssig wird und das
Speichersystem dadurch sehr performant ist.

![Voldemort-Architektur mit modularen Komponenten](https://github.com/TobiasZiolkowski/ostfalia_db_2016_hausarbeiten/blob/master/ap_systems/Bilder/Voldemort-Architektur.jpg)

**Abbildung 6: Voldemort-Architektur mit modularen Komponenten [SKL+12]**

[SKL+12] erkl�ren hier und im Folgenden, dass Voldemort �ber eine modulare Architektur verf�gt, bei
der die einzelnen Komponenten mit anderen Komponenten ausgetauscht werden k�nnen. Die auf
Abbildung 5 gezeigten Boxen entsprechen dabei einem dieser Module, die jeweils �ber das gleiche
Interface ansprechbar sind. Dadurch, dass jedes Modul nur eine Funktionalit�t kapselt, wird die
Austauschbarkeit erleichtert. So kann das Routing Modul sowohl auf der Seite des Clients, als auch
auf der Seite des Servers eingesetzt werden. Die Trennung der Funktionalit�t zwischen den Modulen
erm�glicht es au�erdem, auf einfache Weise Mock-Up-Tests durchzuf�hren, bei denen die Module
getestet werden k�nnen, ohne dass eine reale Datenbank vorhanden ist. Die oberste Box steht f�r
ein einfaches API-Modul, das simple get-Befehle f�r das Lesen von Daten und put-Befehle f�r das
Schreiben verarbeitet.

Die Tupel werden f�r die Verf�gbarkeit repliziert und mit einem Zeitstempel f�r die Versionierung 
versehen. Hierdurch wird die Integrit�t der Daten f�r den Fall des Eintritts eines Fehlers erh�ht, ohne
dass die Verf�gbarkeit der Daten eingeschr�nkt wird. Die Module Conflict Resolution und Repair
Mechanism kommen bei Lese/Schreib Storage Engines zum Einsatz und behandeln dort
inkonsistente Replikate. Das Routing Modul ist f�r die Schaffung von Replikaten und Partitionen
zust�ndig.

![Cluster-Topographie als Ring](https://github.com/TobiasZiolkowski/ostfalia_db_2016_hausarbeiten/blob/master/ap_systems/Bilder/Voldemort%20Cluster-Topographie.jpg)

**Abbildung 6: Cluster-Topographie als Ring[SKL+12]**

Auf Abbildung 6 ist eine einfache Ring Cluster Topographie dargestellt. Der Ring wird von Voldemort
in Partitionen mit gleicher Gr��e aufgeteilt. Die Partitionen erhalten eine eindeutige ID und werden
auf einen Knoten abgebildet. Der Ring wird mit allen Stores geteilt, wodurch �nderungen der
Abbildungen von Partitionen auf Knoten in allen Stores ge�ndert werden m�ssen. Die auf der rechten
Seite von Abbildung 6 gezeigte Pr�ferenzliste listet f�r alle Partitions-IDs auf, in welchen Knoten die
Replikate gespeichert werden. Zun�chst wird ein Hash-Wert eines Schl�ssels erzeugt, der auf einen
Bereich einer Partition zeigt. Dann wird der Ring im Uhrzeigersinn durchlaufen, bis N-1 Partitionen
gefunden wurden, denen ein anderer Knoten zugeteilt ist, als der urspr�nglich betrachteten Partition.
Wird zum Beispiel ein Replikationsfaktor von N=2 verwendet, so besteht die Pr�ferenzliste f�r einen
Schl�ssel, der auf Partition 11 gehasht wird, aus den Eintr�gen Partition 11, Partition 0. Dies
geschieht, weil Partition 0 die erste Partition ist, die auf Partition 11 im Uhrzeigersinn folgt und anstatt
des Knoten 2 den Knoten 0 hat. Die Daten k�nnen so in den Cluster eingebracht werden, ohne dass
alle Daten neu verteilt werden m�ssen. Weil die Knoten unabh�ngig voneinander vorliegen, gibt es
keine zentrale Stelle, �ber die sie verwaltet werden m�ssen und die im Fehlerfall s�mtliche Knoten
betreffen w�rde. Der Single Point of Failure wird also vermieden.

Voldemort unterst�tzt zwei Routing Modi: Clientseitiges Routing und serverseitiges Routing.
�blicherweise wird das clientseitige Routing benutzt, das in einem anf�nglichen Schritt die Metadaten
f�r Speicherdefinition und Topologie des Clusters einholt. Wenn die Metadaten vom Client eingeholt
wurden, ist im Vergleich zum serverseitigen Routing ein Schritt weniger erforderlich, weil die Orte der
Replikate on-the-fly berechnet werden k�nnen. Dies hat jedoch den Nachteil, dass ein kompliziertes
Ausbalancieren der Daten notwendig wird, weil ein Mechanismus erforderlich ist, die verschiedenen
Clients mit den Metadaten zu versorgen.

Als reines Schl�ssel-Wert System hat Voldemort den Nachteil, das die Daten nicht untereinander in
Beziehung gesetzt werden k�nnen. Um einen Wert zu erhalten, muss der Schl�ssel bereits bekannt
sein.

[Pro17] erkl�rt, dass die Knoten von Voldemort mit 10.000 bis 20.000 Operationen pro Sekunde, in
Abh�ngigkeit zur Hardware, sehr performant sind. Dar�ber hinaus werden austauschbare
Speicher-Strategien unterst�tzt, die zum Beispiel die Verteilung der Daten �ber geographisch weit
voneinander entfernten Servern m�glich machen.
