## 3.6 Dynamo
Wie [DHD07]  hier und im Folgenden darlegen, ist Dynamo eine Schl�ssel-Wert Datenbank
mit hoher Verf�gbarkeit, die von Amazon entwickelt wird. Im Gegensatz zu SimpleDB wird
Dynamo, das nicht mit DynamoDB verwechselt werden darf, nur intern von Amazon
verwendet. Amazon hat eine weltweit operierende Online-Vertriebsplattform, die eine hohe
Performanz, Verl�sslichkeit und Effizienz erfordert. Die Plattform basiert auf tausenden
Servern, die auf der ganzen Welt verteilt und miteinander verkn�pft sind, wodurch Ausf�lle
immer wieder vorkommen. Schon kleine Ausf�lle haben jedoch drastische finanzielle
Auswirkungen und beeintr�chtigen das Kundenvertrauen negativ. Das Dynamo System stellt
sicher, dass trotzdem eine unterbrechungsfreie Nutzung der Vertriebsplattform m�glich ist,
auch wenn die Festplatte eines Servers besch�digt wird oder eine Naturkatastrophe ein
ganzes Datenzentrum lahmlegt. Um besondere Belastungen abzufedern, wie sie zum
Beispiel bei einigen Feiertagen auftreten k�nnen, muss das System die Leistung au�erdem
gut skalieren k�nnen, was durch einen dezentralen Aufbau des Netzes von Dynamo erreicht
wird. Jeder Knoten hat volle M�chtigkeit und eine zentrale Steuerungseinheit gibt es nicht.
Wie durch das CAP-Theorem bekannt ist, ist eine hohe Konsistenz und Verf�gbarkeit bei
gegebener Partitionstoleranz nicht gleichzeitig zu erreichen. Weil die hohe Verf�gbarkeit
des Webangebots von Amazon hohe Priorit�t hat, findet das Konzept Eventually Consistent
Anwendung.

Um die genannten Anforderungen zu erreichen, implementiert Dynamo die folgenden f�nf
Kerntechniken, welche auch die wesentlichen Vorteile von Dynamo darstellen: Konsistente
Hashfunktion, Vector Clocks, Sloppy Quorum und Hinted Handoff, Anti-Entropie,
Gossip-basiertes Protokoll.

Die konsistente Hashfunktion setzt das schon im Kapitel Voldemort erkl�rte Prinzip eines
logischen Rings aus Knoten um. Aus den Schl�sseln wird jeweils ein Hashwert berechnet,
der als Zuordnung zu einem Knoten fungiert. Deckt ein Knoten zum Beispiel den Bereich
zehn bis 20 ab, so wird der Wert eines Schl�ssels mit der Zahl 12 auf diesen Knoten
gespeichert. Es werden auch eine frei konfigurierbare Anzahl Kopien in nachfolgenden
Knoten gespeichert, um die Verf�gbarkeit zu gew�hrleisten, falls ein Knoten ausf�llt.

Sloppy Quorum ist ein Prinzip, das bestimmt, bei wie vielen Knoten Lese- und
Schreiboperationen erfolgreich sein m�ssen, damit der gesamte Vorgang erfolgreich ist.
Konfigurierbare Parameter sind hierbei die Anzahl der f�r die Replizierung benutzten
Knoten und die Mindestanzahl der Knoten, die den Erfolg einer Lese- oder Schreiboperation
melden m�ssen. Je nachdem wie die Werte der Parameter justiert sind, k�nnen
unterschiedliche Bedarfsszenarien, wie Lesepuffer oder schnelle Schreibzugriffe abgedeckt
werden.

Das Hinted Handoff Prinzip greift, sobald der Knoten, in dem der Wert eines Schl�ssel-Wert
Paares gespeichert werden soll, nicht verf�gbar ist. Der Inhalt wird dann auf einen weiteren
Knoten �bertragen, wobei gelegentlich �berpr�ft wird, ob der urspr�ngliche Knoten wieder
ansprechbar ist, um die Daten dorthin zu transferieren.

Es kann zu Versionskonflikten kommen, falls ein Schreibzugriff einen Knoten nicht erreicht hat,
der bei einem sp�teren Lesezugriff wieder verf�gbar ist. Die der Versionierung dienenden
Vektoruhren l�sen das Problem, indem jede gespeicherte Datei einen Vektor mit der ID des
Knotens und der Versionsnummer erh�lt. Bei jedem Update wird die Versionsnummer um
eine Zahl erh�ht und es wird bei einer Leseanfrage immer nur die h�chste gefundene Version
zur�ckgegeben.

F�llt der Knoten aus, der durch das Hinted-Handoff die Datei eines anderen Knoten erh�lt, so
erf�hrt der urspr�ngliche Knoten nichts davon, dass ihm eigentlich eine Datei zugewiesen
werden soll. Das Problem wird durch den Mechanismus der Anti-Entropie behandelt, indem
der urspr�ngliche Knoten, sobald er wieder gestartet ist, seinen Inhalt mit den nachfolgenden
Knoten vergleicht, die die Kopie des neuen Inhalts haben.

Das Gossip-basierte Protokoll dient der Behandlung des Hinzuf�gens und Entfernens von
Knoten. So k�nnen die logischen Knoten auf die physikalisch existierenden Datenzentren
dynamisch verteilt werden, um eine m�glichst g�nstige Auslastung zu erzielen. Die genaue
Funktionsweise des Protokolls wird im Rahmen dieser Ausarbeitung nicht beschrieben.

Als Schl�ssel-Wert Datenbank teilt Dynamo mit Voldemort den Nachteil, dass die Daten nicht
miteinander in Beziehung gesetzt werden k�nnen und der Schl�ssel f�r jedes Datum bekannt
sein muss. Wie alle AP-Datenbanken ist Dynamo nicht immer konsistent.
