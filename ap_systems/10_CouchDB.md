## 3.3 CouchDB
CouchDB ist die Abk�rzung f�r �Cluster of unreliable Commodity Hardware Database�
und geh�rt zu den dokumentorientierten, schemalosen Datenbanken. Das bedeutet,
dass der Aufbau von Dokumenten vor der Speicherung nicht bekannt sein muss und
die Datenbank den Vorteil bietet, dass die Dokumente sowohl dem nat�rlichen Objekt
als auch dessen Repr�sentationen in einer objektorientierten Sprache entsprechen
k�nnen. Somit ist die Datenbank sehr flexibel da die Dokumente keinem Schema
entsprechen m�ssen.
CouchDB zeichnet sich laut [EW13] unter Anderem dadurch aus, dass es zu keinen
Schreib-Lese-Blockaden kommt. F�r die Verwaltung von konkurrierenden Zugriffen
wird das Multi-Version Concurrency Control verwendet. CouchDB speichert die
Dokumente im JSON-Format. F�r die Verwaltung wird dabei jedem JSON-Objekt
eine Versionsnummer und ein eindeutiger Schl�ssel zugewiesen. Um nun die
konkurrierenden Zugriffe zu verwalten wird die Versionsnummer bei jeder �nderung
des Dokuments um einen erh�ht. CouchDB selbst erkennt beim Schreiben die
Konflikte und meldet diese an den Client. Dadurch entstehen keine
Schreib-Lese-Blockaden durch Locking, wie bei anderen Systemen.
Da CouchDB zu den AP-Datenbanken geh�rt weist sie Schw�chen bei den
Konsistenzanforderungen auf. Sie verf�gt �ber einen inkrementellen
Replikationsmechanismus mit bidirektionaler Konfliktbehandlung. Das bedeutet,
dass Konflikte auf verschiedenen Servern, die durch das �ndern eines Objekts
verursacht wurden, automatisiert erkannt und anschlie�end behandelt werden.
Es bleiben alle Versionen vorhanden.
Ein weiterer Vorteil von CouchDB ist laut [ASF17] die weitreichende Skalierung.
CouchDB kann sowohl nach oben in einem Cluster, als auch nach unten skalieret
werden. Eine Skalierung nach unten ist notwendig f�r den Gebrauch von CouchDB
auf einem Smartphone. Es l�sst sich also eine Instanz von CouchDB auf einem
Android-Ger�t oder einem iPhone starten.
CouchDB wird aufgrund der oben genannten Eigenschaften haupts�chlich f�r
Webanwendungen verwendet, da hier oft viel Text mit einer nicht definierten L�nge
gespeichert werden muss. Klassische Anwendungsbeispiele sind also Foren,
Blogs oder Content-Management-Systeme.
Um die Vor- und Nachteile von CouchDB besser zu beschreiben wird im Folgenden
der Einsatz f�r einen Blog herangezogen.
CouchDB kann in einem Blog zum Beispiel f�r die Speicherung der Blogeintr�ge
und Kommentare dienen. Jeder Blogeintrag steht in einem Dokument. Der Text wird
entweder als Array von Strings in einem Wert, oder �ber mehrere Strings verteilt in
mehreren Werten gespeichert. Dazugeh�rige Kommentare werden im selben
Dokument gespeichert. Der Text wird in einem String gespeichert und der
Zeitstempel kann als Schl�ssel benutzt werden.
An diesem Beispiel wird der Vorteil von CouchDB deutlich, dass alle Inhalte, die zu
dem Block geh�ren, in einem Dokument gespeichert werden. Dahingegen sind bei
Blogeintrage bei Blogs, die relationale Datenbankmodelle nutzen, oft auf mehrere
Tabellen verteilt. Beispielhaft g�be es eine Tabelle f�r die Blogeintr�ge und eine
Tabelle f�r die Kommentare.
Die Nutzung von CouchDB bringt aber auch Nachteile mit sich. So k�nnen keine
detaillierten Schreib- und Leserechte gesetzt werden. Es gibt nur einen Nutzer
�Administrator�. Deshalb eignet sie sich eher f�r Anwendungen, bei denen nur ein
kleiner Nutzerkreis Daten schreiben darf.
