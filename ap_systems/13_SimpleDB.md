## 3.5 SimpleDB
SimpleDB ist nach [ABGA15] eine verteilte Datenbank, die von Amazon.com entwickelt
wurde und deren Cloud-Computing-Service Amazon Web Services zugeh�rig ist.
Zusammen mit den anderen Diensten von Amazon Web Services, n�mlich Amazon
Simple Storage Service (Amazon S3) und Amazon Elastic Compute Cloud (Amazon EC2)
k�nnen Daten in der Cloud gespeichert, bearbeitet und abgefragt werden. Daten k�nnen
mit SimpleDB auf sehr einfache Art organisiert werden. Der gesamte Datenraum ist
untergliedert in Dom�nen, denen Items zugeordnet sind, die wiederum �ber Attribute verf�gen,
�ber die sie beschrieben werden. Die Dom�nen entsprechen den Tabellen bei relationalen
Datenbanken. Nach [Ama14] bestehen die Attribute aus Name-Wert Paaren.
 [ABGA15] beschreiben, dass es bei SimpleDB keine Unterscheidung zwischen Daten und
Metadaten gibt. Die Struktur des Informationsraums kann jederzeit beliebig ge�ndert werden.

![Anschaulicher Vergleich des SimpleDB-Aufbaus mit einer Tabellenkalkulation](https://github.com/TobiasZiolkowski/ostfalia_db_2016_hausarbeiten/blob/master/ap_systems/Bilder/SimpleDB%20Aufbau.jpg)

**Abbildung 6: Anschaulicher Vergleich des SimpleDB-Aufbaus mit einer Tabellenkalkulation [AMA14]**

Abbildung 7 veranschaulicht diesen Aufbau durch den Vergleich mit der Datei einer
Tabellenkalkulation. Die Dom�nen entsprechen dabei den Tabs im unteren Bereich des Bildes,
wie [Ama14] hier und im Folgenden aufzeigt. Die Dom�nen entsprechen damit den Tabellen. 
Weil es keine Abh�ngigkeiten zwischen den Dom�nen gibt, k�nnen Abfragen immer nur gegen
eine Dom�ne get�tigt werden, aber nicht gegen mehrere Dom�nen. Die Zeilen sind analog zu
den Items von SimpleDB. Sie enthalten Attribute, welche Datenkategorien entsprechen und hier
als Spalten der Tabelle dargestellt sind. Eine Item-Attribut Kombination enth�lt die Instanz einer
Datenkategorie. Dabei kann es sich um einen Wert oder aber um mehrere Werte handeln, was
einen Unterschied zu Tabellenkalkulationsprogrammen darstellt, bei denen eine Zelle immer nur
einen Wert enthalten darf. 

![Items mit unterschiedlichen Attributen in einer Dom�ne](https://github.com/TobiasZiolkowski/ostfalia_db_2016_hausarbeiten/blob/master/ap_systems/Bilder/Items-Attribute%20SimpleDB.jpg)

**Abbildung 6: Items mit unterschiedlichen Attributen in einer Dom�ne [AMA14]**

Die Dom�nen von SimpleDB ben�tigen keine spezifischen Attribute. Es k�nnen also Items mit
unterschiedlichen Attributen in einer Tabelle vorliegen. In der auf Abbildung 8 dargestellten
Dom�ne befinden sich zum Beispiel Objekte unterschiedlicher Produktkategorien. Die Indexierung
der Daten findet bei SimpleDB automatisch statt.

Es werden bei SimpleDB zwei Konsistenz-Modi unterst�tzt: Eventually Consistent Read und
Consistent Read. Ist der Modus Eventually Consistent Read aktiv, so folgt SimpleDB dem Konzept
des Eventually Consistent. Die Daten werden sofort zur Verf�gung gestellt, aber repr�sentieren
mitunter nicht den Stand, der durch einen k�rzlich abgeschlossenen Schreibvorgang geschaffen
worden ist. Allerdings dauert es �blicherweise nur etwa eine Sekunde bis die Konsistenz der
gesamten Datenbank hergestellt ist. Ist der Modus Consistent Read aktiv, so werden nur konsistente
Daten zur�ckgegeben.

Obwohl sich Anfragen bei SimpleDB nur auf eine einzige Dom�ne beziehen k�nnen, k�nnen
Anwendungen eine parallele Partitionierung der Daten zur Verbesserung der Performance
unterst�tzen. Daf�r m�ssen die Daten in mehreren Dom�nen vorliegen. Die Anwendung schickt dann
Anfragen an diese Dom�nen und fasst die Ergebnisse zusammen. Bei Daten wie Logs, die sich
nicht leicht partitionieren lassen, k�nnen die Daten �ber einen Hash-Algorithmus auf eine von
mehreren Dom�nen abgebildet werden, wodurch eine einheitliche Verteilung auch auf sehr viel
Dom�nen erreicht wird. 

[Wei09] stellt als Nachteil die fehlende Kontrolle fest, weil die Daten bei einem Drittanbieter
vorliegen. Ferner ist zu bem�ngeln, dass wegen des Fehlens von Datentypen eine gr��ere
Anf�lligkeit f�r Programmierfehler gegeben ist. Au�erdem werden keine Joints unterst�tzt und die
Konsistenz kann wie bei allen AP-Systemen nicht garantiert werden. Zus�tzlich zu der bereits erkl�rten
Schemafreiheit mit variablen Attributen f�r Items einer Dom�ne sind als Vorteile zu nennen:
Automatische Backups, Beschr�nkung der Kosten auf den aktuellen Bedarf, geringe Latenz, einfache
Schnittstellen, spontane Skalierung mit der Last und dadurch hoher Durchsatz.
