# Analyse konkreter Beispiele
Dieser Abschnitt er�rtert die Funktionsweise und Eigenschaften einiger AP-Datenbanken,
sowie das h�ufig mit diesen verkn�pfte Konzept des Cloud-Computings. Behandelt werden
Facebook mit Cassandra, CouchDB, Voldemort, SimpleDB und Dynamo.

## 3.1 Cloud Computing
Das Cloud Computing ist eine neue Technologie die Unternehmen und Privatpersonen eine
Datenspeicherung �ber das Netzwerk, bzw. das Internet, erlaubt. Es stellt somit eine alternative
L�sung der Datenspeicherung dar, sodass keine teuren Rechenzentren mehr angeschafft
werden m�ssen. Der Datenspeicher kann einfach bei einem Cloud-Anbieter angemietet werden.

Der Begriff Cloud-Computing umfasst laut [TG13] zudem de Bereitstellung von IT-Diensten und
Ressourcen jeglicher Art, wie zum Beispiel Anwendungen, Rechenleistung oder einer Kombination
daraus. In dieser Ausarbeitung wird aber nur spezieller auf das Bereitstellen von Speicherplatz und
der zu Grunde liegenden Technik und Architektur eingegangen.

Es gibt eine Vielzahl an Diensten, die in der Cloud angeboten werden k�nnen. Das Bereitstellen
von Hardware nennt sich Infrastructure-as-a-Service. Hier wird dem Nutzer nicht nur das eigentliche
Programm, sondern auch die gesamte Datenverarbeitungsstruktur zur Verf�gung gestellt.

Die Kunden verwalten ihren angemieteten Speicherbedarf eigenst�ndig. Lediglich der
infrastrukturelle Bereich, der die Bereitstellung der Hardware, die Ausfallsicherheit und die
Datensicherung/- wiederherstellung umfasst, wird vom Anbieter �bernommen.

Eine Technik, die die effiziente Umsetzung des Cloud Computings erst erm�glicht hat, ist die
Virtualisierung. Bei der Virtualisierung werden logische Serversysteme und Dienste auf der
physikalischen Ebene getrennt behandelt. Die Hardwareressourcen stehen dabei mehreren
virtuellen Servern oder Benutzern gleichzeitig zur Verf�gung. Damit es keine Probleme bei der
parallelen Nutzung der Dienste gibt, erfolgt die Ausf�hrung der Anwendungen v�llig isoliert
voneinander.

Es gibt laut [TG13] zwei Varianten um Cloud-Dienste zu nutzen. Dies ist abh�ngig von den 
Anforderungen, die durch die Kunden bestimmt werden. Die am h�ufigsten verwendete Variante
des Cloud-Computings ist die Public Cloud. Mit Public Cloud wird ein Modell beschrieben, bei dem
mehrere Kunden gleichzeitig auf dieselben physikalischen Ressourcen zugreifen. Die Ressourcen
werden dabei entsprechend nach dem Anteil zugewiesen, der vom Anbieter angemietet wurde.

Einige gro�e Unternehmen, die eine Public Cloud verwenden, sind die Services von Google,
Amazon und Microsoft.

Demgegen�ber steht die Private Cloud. Diese Variante des Cloud Computings zeichnet sich durch
eine bessere Sicherheit und Privatsph�re aus. Dies wird dadurch gew�hrleistet, dass die Nutzer in
der Regel bekannt sind und das Unternehmen die Infrastruktur selber stellt. Der gro�e Nachteil ist
aber, dass das Unternehmen f�r die Anschaffung und Wartung der Hardware selber aufkommen
muss.
Nachdem die Funktionsweise des Cloud Computings klar ist, werden im Folgenden die konkreten
Vor- und Nachteile der Verwendung von AP-Datenbanken als Grundlage f�r das Cloud-Computing
am Beispiel der Google AppEngine DataStore analysiert.

Googles App Engine verwendet laut [TG13] das ebenfalls von Google entwickelte BigTable System.
Bei diesem System ist es im Gegensatz zu relationalen Modellen m�glich, dass unterschiedliche
Zeilen andere Spalten beinhalten. Es handelt sich also um ein schemaloses und kein relationales
Modell.

BigTable kann als gro�es, sortiertes, mehrdimensionales Array beschrieben werden, bei dem jede
Zeile eine gewisse Anzahl an Spalten hat. Unterschiedliche Zeilen k�nnen aber unterschiedlich viele
Spalten haben.

Ein weiteres wichtiges Merkmal des AppEngine Datastore ist, dass keine Tabellen selbst erstellt
werden k�nnen. Stattdessen werden die Daten automatisch in die vorhandene Struktur eingebunden.
Um Datens�tze zu gruppieren k�nnen aber Entit�tsgruppen angelegt werden.

Die grunds�tzliche Motivation zur Entwicklung des Google Datastores war eine sehr hohe Lese- und
Abfragegeschwindigkeit zu erzielen. Gleichzeitig sind die Latenzzeiten sehr niedrig, sodass sich
BigTable sowohl f�r Anwendung eignet, die eine sehr hohe Skalierbarkeit bei geringer Latenz erfordert,
als auch f�r Anwendungen die sehr viele Transaktionen ben�tigen.

Hinsichtlich der Verf�gbarkeit und Konsistenz bietet diese Datenbank zwei verschiedene Modi an. Da
laut CAP-Theorem nur zwei der drei Eigenschaften Partition Tolerance, Availability und Consistency
erf�llt werden k�nnen, muss sich der Nutzer f�r einen Modus entscheiden. Da es in dieser Ausarbeitung
um die Vor- und Nachteile von AP-Datenbanken geht, wird hier nur der Modus beschrieben, bei dem
die Verf�gbarkeit sehr hoch ist.

Neben dem Master/Slave Datenspeicher l�sst sich die Datenbank der Google AppEngine Datastore
laut [TG13] mit einem High Replication-Datenspeicher konfigurieren. Bei diesem Datenspeicher werden
alle Daten asynchron mit Hilfe des auf einem Paxos-Algorithmus bestehenden Systems �ber mehrere
Datenzentren hinweg repliziert. 

Durch die Replikation kann eine sehr hohe Verf�gbarkeit f�r Lese- und Schreibvorg�nge erreicht werden.
Bei einer gleichzeitigen, sehr niedrigen Latenzzeit zeigt sich, dass diese Architektur sehr gut f�r operative,
aber auch analytische Zwecke geeignet ist. 

Wie bereits in den Grundlagen beschrieben, zeichnen NoSQL-Datenbanken, bzw. AP-Datenbanken, sich
dadurch aus, dass sie das Modell der Eventual Consistency verfolgen. Dies ist auch bei dem von Google
entwickelten Datastore der Fall. Es kann also nicht jederzeit gew�hrleistet werden, dass der abgefragte
Inhalt immer auf dem neuesten Stand ist, da die Synchronisierung der Daten innerhalb der Server
asynchron erfolgt.

Googles BigTable zeichnet sich au�erdem durch eine sehr gute Partition Tolerance aus. Dies zeigt sich
zum Beispiel darin, dass Updates w�hrend dem laufenden Betrieb eingespielt werden k�nnen und keinen
Neustart des gesamten Systems erfordern. Dadurch kann eine optimale Nutzung der Ressourcen und
Applikationen auf dieser Datenbank sichergestellt werden. Alle Prozesse und Workflows bleiben also online.
Gleiches gilt f�r das �nderungen am zu Grunde liegenden Cluster. Nodes k�nnen dynamisch hinzugef�gt,
oder entfernt werden. Das System ist also nicht abh�ngig von einzelnen Knoten. Der Nutzer bekommt von
einer �nderung am Cluster nichts mit.

Als weitere Nachteile des Google AppEngine Data Stores sind h�here Kosten f�r die CPU und den Speicherplatz zu nennen. 
