### 2.1.3 Das BASE-Prinzip
Wie [FHK17] hier und im Folgenden feststellt, steht das Akronym BASE
f�r Basically Available, Soft State, Eventually Consistent. Verbreitete
Anwendung findet das BASE Prinzip bei NoSQL Datenbanken. Im Web
2.0 Zeitalter haben sich die Anforderungen an Web-Anwendungen
teilweise ge�ndert. So ben�tigen soziale Netzwerke eine hohe
Verf�gbarkeit und hohe Performance w�hrend der Gesamtlaufzeit,
wobei die Konsistenz der Nachrichten vernachl�ssigt werden darf.
Datenbanken, die das BASE-Prinzip verfolgen, kommen vollst�ndig
ohne Sperren aus. Sperren von Datens�tzen durch eine Transaktion
bewirken, dass sie so lange durch weitere Transaktionen nicht 
zugreifbar sind, wie die Sperre anh�lt. Dies kann mit Wartezeiten
einhergehen, deren L�nge nicht kalkulierbar ist. 

Bei verteilten Datenbanksystemen, wie sie bei Web 2.0 Anwendungen
n�tig sind, ist bereits die �bereinkunft von Sperren fehleranf�llig und
im Aufwand erheblich. Die Dauer der Sperre verl�ngert sich hier
zus�tzlich um die Zeit, die es braucht, um die Datenbank�nderung auf
allen beteiligten Datenbankservern durchzuf�hren. Sperren sind
deswegen aufwendig, weil die �brigen Server �ber die Absicht der
Sperrung in Kenntnis gesetzt werden m�ssen, wobei die einzelnen
Server der Anfrage entsprechen oder diese auch ablehnen k�nnen.
Im Falle der Zustimmung muss der anfragende Server benachrichtigt
werden. Dieser wartet bis genug best�tigende Antworten eingegangen
sind, aktiviert anschlie�end die Sperre und informiert dann die �brigen
Server dar�ber, dass die Sperre gesetzt ist. Diese Kommunikation
findet in Form von Nachrichten �ber das Netz statt und ist ebenso
aufwendig, wenn die Sperre wieder aufgehoben werden soll. Die Idee
des Eventually Consistent ist nun, dass der Zustand der Konsistenz der
Daten irgendwann, aber nicht unbedingt sofort, erreicht wird. Dass bis
zum Eintreten des Zustands der Konsistenz die Daten nicht konsistent
vorliegen, wird hierbei hingenommen. 

Zu beachten ist hier, dass beim BASE-Prinzip der Begriff Konsistenz
eine andere Bedeutung hat, als beim ACID-Prinzip. Beim ACID-Prinzip
bezieht sich die Konsistenz auf die Integrit�t der Daten, w�hrend beim
BASE-Prinzip die inhaltliche Korrektheit des abzuspeichernden
Datensatzes als konsistent betrachtet wird. Das ACID-Prinzip und das
BASE-Prinzip sind trotz ihrer Differenzen als zwei Enden eines
Konsistenzspektrums zu sehen, wobei die �berg�nge flie�end sind.
So gibt es Systeme, die sowohl das ACID-Prinzip als auch das
BASE-Prinzip verfolgen.

![Akzeptable Wartezeiten bei BASE-Datenbanken](https://github.com/TobiasZiolkowski/ostfalia_db_2016_hausarbeiten/blob/master/ap_systems/Bilder/Wartezeit%20BASE.jpg)
**Abbildung 3:Akzeptable Wartezeite bei BASE-Datenbanken [FHK17]**

Abbildung 3 verdeutlicht die Konsistenz ohne Sperren bei Anwendungen
des BASE-Prinzips. Bei Transaktion TransB wird bei einer �nderung der
Daten ein neuer Datensatz erzeugt. Die lesenden Transaktionen TransC
und TransD verweisen so lange auf den alten Datenzustand, bis die neue
Version auf allen Datenknoten repliziert ist.