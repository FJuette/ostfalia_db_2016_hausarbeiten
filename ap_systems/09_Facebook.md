## 3.2 Facebook
Der zentrale Anlaufpunkt der Facebook-Architektur ist laut [JI09] dessen Cache-System.
Traditionelle Ans�tze der relationalen Datenbanken sind f�r die vernetzten Daten eines
Social Networks nicht geeignet. Ein wesentlicher Unterschied zu handels�blichen
Anwendungen ist hier n�mlich, dass die Nutzer nicht nach ihren eigenen Daten, sondern,
z.B. bei Facebook, nach den Daten anderer Nutzer suchen.

Die Daten werden von Facebook auf den Datenbankservern in normalisierter Form
gehalten. Die Nutzer werden zuf�llig auf die Datenbankserver verteilt. Es findet also
kein Clustering nach Gruppen statt. Deswegen kommt dem Caching-System eine
zentrale Bedeutung zu. Facebook verwendet daf�r ein weiterentwickeltes Memcached,
welches von Brad Fitzpatrick entwickelt wurde.

F�r die Datenverwaltung verwendet Facebook tausende  auf mehrere Rechenzentren
verteilte MySQL-Server. Wesentliche Funktionen einer relationalen Datenbank werden
allerdings nicht genutzt, wie zum Beispiel JOINs. JOINs werden nur f�r ausgew�hlte
Systeme wie die Suche eingesetzt. Der Grund daf�r ist, dass es beinahe unm�glich ist,
�ber die verteilten Datenbanken JOINs einzurichten.

Anfangs reichten Facebook 20 MySQL Server auf einzelnen physischen Maschinen aus.
Mittlerweile wurden diese aber auf mehr Server verteilt, was durch die oben beschriebene
Struktur aber unproblematisch war. Genau in diesem Punkt profitiert Facebook von der
AP-Struktur ihrer Datenbanken, die eine hohe Skalierbarkeit gew�hrleistet.

Das bei Facebook eingesetzte Datenbanksystem hei�t Cassandra. Cassandra wurde
urspr�nglich von Facebook f�r Facebook und deren spezifische Probleme entwickelt.
Die Motivation zur Entwicklung von Cassandra war laut [JS11] das sogenannte
�Inbox Search Problem�. Die Facebook-Nutzer sollten die M�glichkeit haben ihre erhaltenen
und gesendeten Nachrichten zu durchsuchen. Mit MySQL gibt es dazu keine ausreichend
performante L�sung mehr, da die Anzahl der Nutzer und die konstant anwachsende
Datenmenge zu gro� geworden sind.

Mit Cassandra konnte dieses Problem gel�st werden. Das neu entwickelte
Datenbanksystem ist auf hohe Schreib- und Lesedurchs�tze optimiert, wobei die 
Schreibvorg�nge standardm��ig asynchron erfolgen. Die Daten werden also zun�chst im
RAM gehalten und geb�ndelt auf die Festplatte geschrieben. Weiter bietet Cassandra die
M�glichkeit der Partitionierung auch �ber mehrere Rechenzentren hinweg.

AP-Datenbanken zeichnet zudem aus, dass sie den besonderen Anforderungen der
Hochverf�gbarkeit und Ausfalltoleranz gerecht werden und gleichzeitig laut CAP-Theorem
nicht zu jedem Zeitpunkt konsistent sind. Cassandra zeichnet sich durch diese Eigenschaften
aus. So bietet die Datenbank die M�glichkeit der Replikation und eine automatische
Erkennung von ausgefallenen Rechnern im Datenbankverbund. Die ausgefallenen Rechner
werden durch die Nutzer nicht wahrgenommen. Es gibt dadurch also keine sichtbaren
negativen Effekte f�r den Nutzer.

Die Ausfallsicherheit von Cassandra ist auch deswegen so gut, da es keinen Single Point
of Failure gibt. Die Lese- und Schreibzugriffe sind nicht auf bestimmte Nodes beschr�nkt
Allgemein bietet Cassandra eine gute Konfigurierbarkeit bez�glich des CAP-Theorems.
Nachdem die Anforderungen des Systems feststehen kann der Anwender bestimmen,
welche Eigenschaften des CAP-Dreiecks es eher erf�llen soll.

Der Nachteil von AP-Datenbanken und damit auch von Cassandra ist, dass die Daten
nicht immer konsistent vorliegen. Es kann also sein, dass ein Anwender einen Eintrag
tempor�r doppelt oder gar nicht sieht. F�r das Einsatzgebiet der sozialen Netzwerke ist
die Konsistenz aber kein kritisches Merkmal, da der Nutzer keinen Nachteil dadurch hat,
wenn in seinem Newsfeed ein gepostetes Video doppelt oder nicht angezeigt wird.
Deswegen werden mit Cassandra Schw�chen in der Konsistenz akzeptiert, um eine hohe
Verf�gbarkeit, Geschwindigkeit und Ausfallsicherheit zu gew�hrleisten.

Cassandra erlaubt es dem Nutzer aber auch verschiedene Konsistenzlevel zu definieren,
sodass hier auf Kosten der Verf�gbarkeit eine h�here Konsistenz erreicht werden kann. In
den nachfolgenden Tabellen sind die einzelnen Konsistenzlevel nach Schreiboperationen
und Leseoperationen aufgezeigt:

![Schreibkonsistenz bei Cassandra](https://github.com/TobiasZiolkowski/ostfalia_db_2016_hausarbeiten/blob/master/ap_systems/Bilder/Schreibkonsistenz%20Cassandra.jpg)

**Abbildung 4:Schreibkonsistenz von Cassandra [FHK17]**

![Lesekonsistenz bei Cassandra](https://github.com/TobiasZiolkowski/ostfalia_db_2016_hausarbeiten/blob/master/ap_systems/Bilder/Lesekonsistenz%20Cassandra.jpg)

**Abbildung 5: Lesekonsistenz bei Cassandra [FHK17]**

Die Skalierbarkeit von Cassandra ist deswegen so hoch, da f�r das Hinzuf�gen von Nodes nur
eine neue Instanz gestartet werden muss. Es werden keine weiteren Aktionen ben�tigt. Das
Verkleinern des Verbundes funktioniert genauso. 
Als Nachteil des Einsatzes von Cassandra ist die geringe Menge an Abfragem�glichkeiten zu
nennen. In dieser Hinsicht bieten andere Datenbankmanagementsysteme bessere und
zahlreichere L�sungen an. Gerade in den Bereichen der Map- und Reduce Verfahren k�nnen
Berechnungen mit alternativen L�sungen besser durchgef�hrt werden.
Allerdings bietet Cassandra die M�glichkeit solche Berechnungen nachzur�sten, z.B. �ber die
Cassandra-Hadoop-Inspiration.


