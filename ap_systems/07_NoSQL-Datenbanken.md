### 2.2.2 NoSQL-Datenbanken
[LTS+17] legen hier und im Folgenden dar, dass es sich bei NoSQL-Datenbanken um
schemafreie Datenbanken handelt. Der Begriff schemafreie Datenbanken wurde 2009
gepr�gt, um eine Bezeichnung f�r die steigende Anzahl nicht relationaler Datenbanken
zu haben. Flexible Datenbanken werden durch das dynamisch steigende Datenvolumen
von Web 2.0 Anwendungen erforderlich, welche die gro�en Datenmengen auf mehrere 
Hardwareeinheiten verteilt speichern k�nnen, wof�r die relationalen Datenbanken sich
nicht gut eignen. Neben der Tatsache, dass sie nicht relational und schemafrei sind,
zeichnen sich NoSQL Datenbanken dadurch aus, dass diese Systeme von Beginn an
auf eine horizontale Skalierbarkeit, also die Erweiterung eines Systems mit zus�tzlichen
Recheneinheiten, ausgelegt sind. 

Zus�tzlich unterst�tzen NoSQL-Systeme eine einfache Datenreplikation f�r die verteilte
Architektur. NoSQL-Datenbanken verf�gen dar�ber hinaus �ber eine einfache API und
liegen dem BASE-Prinzip zugrunde, nicht jedoch dem ACID-Prinzip. NoSQL-Systeme
verfolgen damit das Konzept des Eventually Consistent.

Es gibt verschiedene Arten von NoSQL-Datenbanken:
�	Schl�ssel-Wert Datenbanken: Die Werte der Daten werden �ber jeweils einen
                  eindeutigen Schl�ssel referenziert. Die Daten k�nnen sowohl auf der Festplatte 
                  als auch im Arbeitsspeicher vorgehalten werden.
�	Dokumentenorientierte Datenbanken: Die Daten werden als Dokumente
                  abgespeichert, deren Formate durch die Datenbank erkannt werden kann. Die
                  Inhalte der Dokumente k�nnen ebenfalls verarbeitet werden.
�	Spaltenorientierte Datenbank: Die Daten werden in Tabellen gespeichert, deren
                  Spalten dynamisch wachsen k�nnen. F�r jede Spalte existiert dabei eine eigene
                  Datei.
�	Graphdatenbank: Die Daten werden als Graph gespeichert, der aus Knoten f�r
                  die Datenobjekte und Kanten f�r die Beziehungen zwischen den Objekten besteht. 
