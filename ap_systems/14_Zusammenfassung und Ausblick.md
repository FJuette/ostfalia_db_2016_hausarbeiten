# 4 Zusammenfassung und Ausblick
Diese Ausarbeitung hat das Ziel, die Eigenschaften und Vor- und Nachteile von AP-Datenbanken
aufzuzeigen. Das Ziel wurde erreicht, indem nach der Behandlung wichtiger theoretischer
Grundlagen, wie das CAP-Theorem und die Prinzipien ACID und BASE, die Analyse von konkreten
Datenbanksystemen und des Konzepts Cloud Computing erfolgt ist. 

So stellt das Cloud-Computing eine neue Technologie dar, mit der alle Daten �ber das Netzwerk
gespeichert und abgerufen werden k�nnen. Zusammen mit der Virtualisierung von Rechnersystemen
k�nnen logische Serversysteme und Dienste auf der physikalischen Ebene getrennt behandelt
werden, sodass die Hardware effektiver genutzt werden kann. Ein Anbieter der sogenannten Cloud
ist Googles AppEngine Datastore. Dieses nutzt das schemalose, nicht-relationale Datenbankmodell
Goggle BigTable. Dieses Modell zeichnet sich dadurch aus, dass sehr hohe Lese- und
Abfragegeschwindigkeiten, bei gleichzeitig niedrigen Latenzzeiten, erzielt werden. Die
Synchronisierung der Server erfolgt asynchron, wodurch sich Schw�chen in der Konsistenz zeigen.
Schlie�lich zeichnet sich dieses Modell durch eine sehr gute Ausfalltoleranz aus. 

Die Datenbank Cassandra, die beim sozialen Netzwerk Facebook zum Einsatz kommt, basiert auf
mehreren MySQL Knoten. Cassandra wird den immer gr��er werdenden Datenmengen gerecht,
indem durch Ma�nahmen wie asynchrone Schreibvorg�nge eine hohe Performanz erreicht wird.
Durch Replikation und eine automatische Fehlerbehandlung bei ausgefallenen Rechnern und weil
es keinen Single Point of Failure gibt, ist Cassandra auch ausfallsicher. Ein Nachteil von Cassandra
ist, dass Nachrichten bei Facebook mehrfach oder gar nicht angezeigt werden k�nnen, weil die
Konsistenz der Datenbank nicht immer erf�llt ist. Dies ist auf Nutzerseite aber tolerierbar und durch
Skalierung der Konsistenzlevel kann das Problem auch eingegrenzt werden.

Als dokumentenorientierte, schemafreie Datenbank ist der Aufbau der gespeicherten Daten bei
CouchDB nicht festgelegt. CouchDB bietet zudem eine hohe Flexibilit�t wegen seiner Schemafreiheit.
Wie f�r AP-Systeme typisch hat Cassandra geringe Anforderungen der Konsistenz. Ein
Replikationsmechanismus mit bidirektionaler Konfliktbehandlung l�st Konflikte bei den auf mehrere
Server verteilten Daten auf. CouchDB unterst�tzt vertikale Skalierung und ist damit sowohl auf gro�en
Server-Clustern, als auch auf einzelnen Ger�ten lauff�hig. Cassandra kommt bei Web-Anwendungen
wie Foren oder Blogs zum Einsatz, wo Dokumente nicht fest definierter Menge gespeichert werden
m�ssen.

Voldemort ist eine Schl�ssel-Wert Datenbank mit horizontaler Skalierung. Eine Voldemort Datenbank
setzt sich aus logischen, Rechnerknoten zusammen, die Stores verwalten, welche das performante
�quivalent zu Tabellen bei relationalen Datenbanken sind. Durch eine Ring-Replikation der Knoten
wird die Verf�gbarkeit der Daten gew�hrleistet. Der modulare Aufbau der Voldemort-Architektur,
diverse Speicherstrategien und zwei unterschiedliche Routing-Modi erm�glichen zus�tzlich die flexible
Anpassung des Systems an die vorliegenden Anforderungen. Als Schl�ssel-Wert Datenbank kann
allerdings keine Beziehung zwischen den Daten hergestellt werden und der Schl�ssel muss zuvor
bekannt sein.

Das von Amazon entwickelte SimpleDB ist Teil dessen Web-Services, mit dem Daten abgelegt,
verarbeitet und abgerufen werden k�nnen. Der Datenraum ist auf einfache Weise in Dom�nen
aufgeteilt, die wiederum aus Items bestehen, die Attribute haben, welche gespeicherte Werte enthalten
k�nnen. Die Struktur des Datenraums ist dabei dynamisch �nderbar und Items in einer Dom�ne k�nnen
auch unterschiedliche Attribute enthalten. Die Indexierung der Daten �bernimmt das System dabei
automatisch.  Anfragen k�nnen bei SimpleDB immer nur gegen eine Dom�ne get�tigt werden, aber
durch parallele Partitionierung der Daten auf mehrere Dom�nen kann die Performanz dennoch
gesteigert werden. Nachteile sind zum Beispiel mangelnde Kontrolle, Anf�lligkeit f�r Programmierfehler
und fehlende Joints. Automatische Backups, die dynamische Struktur des Datenraums, gute Latenz und
guter Durchsatz, sowie eine einfache Schnittstelle sind als Vorteile festzustellen.

Dynamo ist ebenfalls eine Schl�ssel-Wert Datenbank, die von Amazon entwickelt wird, jedoch nur f�r
den internen Gebrauch der Firma verwendet wird. Das weltweit operierende Amazon-Netzwerk stellt
hohe Anforderungen an Verf�gbarkeit und Performanz, dem die f�nf Kerntechniken von Voldemort
gerecht werden: Konsistente Hashfunktion, Vector Clocks, Sloppy Quorum und Hinted Handoff,
Anti-Entropie, Gossip-basiertes Protokoll. Als Nachteile sind zu sehen, dass die Daten nicht miteinander
in Beziehung gesetzt werden k�nnen und der Schl�ssel f�r jeden Wert bekannt sein muss.

Insgesamt l�sst sich feststellen, dass die AP-Datenbanken neben ihrer Gemeinsamkeit, eine hohe
Verf�gbarkeit und Partitionstoleranz bei nachgelagerter Konsistenz zu bieten, auch einige Unterschiede
haben; zum Beispiel bei der Skalierbarkeit, Partitionierung, Architektur, Anpassbarkeit,
Fehlerbehandlung und Formatierung der Daten. Alle Datenbanken streben, wie es bei AP-Datenbanken
zu erwarten ist, eine gute Performanz und Verf�gbarkeit f�r ihren Gebrauch im Internet an, eignen sich
durch die Unterschiede aber auch f�r unterschiedliche konkrete Anwendungsgebiete von Blogs �ber
weltweit operierende Verkaufsplattformen bis hin zu sozialen Netzwerken.

Die Ergebnisse dieser Ausarbeitung k�nnen f�r k�nftige wissenschaftliche Arbeiten benutzt werden. So
bieten sich Vergleiche der hier dargestellten AP-Datenbanken mit anderen Datenbanken vom Typ CP
oder CA an. Weil zunehmend neue AP-Datenbanken erscheinen und wieder verschwinden, k�nnen
sp�tere Arbeiten die hier zusammengetragenen Erkenntnisse erg�nzen. Auch Software-Projekte, die die
Entwicklung von Datenbanken zum Ziel haben, die mehrere genannte Eigenschaften und Vorteile der hier
vorgestellten Datenbanken vereinen, sind m�glich. Es k�nnen ferner weitere Anwendungsgebiete ermittelt
werden, f�r die sich AP-Datenbanken eignen, wie zum Beispiel aufwendige Online-Spiele und darauf
aufbauend neue AP-Datenbanken entwickelt werden.
