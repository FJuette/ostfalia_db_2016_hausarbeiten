### Atomic Commit Protokolle
In verteilten Datenbankensystemen werden Transaktionen verwendet, um eine Folge von Aktionen oder Schritten, als komplette Einheit entweder ganz oder gar nicht durchzuf�hren. 
Die Transaktion soll die sogenannten ACID-Eigenschaften erf�llen:

Atomicity: Die Ausgef�hrte Operation muss atomar sein, d.h. die Operation muss ganz oder gar nicht ausgef�hrt werden. Besteht beispielsweise eine Transaktion aus mehreren �Updates� einer Tabelle, so muss bei einem Fehler einer der Updates alle bisherigen �nderungen wieder r�ckg�ngig gemacht werden. Dies wird meistens durch eine Historie und ein Rollback realisiert.

Consistency: Das System muss sich in einem konsistenten Zustand befinden, nachdem eine Transaktion durchgef�hrt wurde, wenn es zuvor bereits in einem konsistenten Zustand war. 

Isolation: Werden mehrere Transaktionen gleichzeitig ausgef�hrt, so d�rfen diese sich nicht gegenseitig beeinflussen. 

Durability: Nach dem Abschluss einer Transaktion ist garantiert, dass die �nderungen persistent gespeichert sind. Tritt nach der abgeschlossenen Transaktion ein Fehler auf, so darf dies niemals dazu f�hren, dass das Ergebnis der abgeschlossenen Transaktion verloren geht. 

Atomic Commit Protokolle werden bei verteilten Transaktionen verwendet um einen Teil der ACID-Eigenschaften zu gew�hrleisten. Durch ein Atomic Commit Protokoll kann die Atomarit�t und die Konsistenz erreicht werden. An eine verteilte Transaktion werden verschiedene Anforderungen gestellt. Alle Knoten m�ssen global zu einer gleichen Entscheidung f�r (�Commit�) oder gegen (�Abort�) die Transaktion kommen. Der Commit wird nur ausgef�hrt, wenn alle Knoten mit �Ja� gestimmt haben und kein Fehler aufgetreten ist. Somit wird die Atomarit�t der Transaktion sichergestellt, da die Transaktion entweder ganz oder gar nicht ausgef�hrt wird. Da dies auf allen Knoten mit der gleichen Entscheidung durchgef�hrt wird, ist die Konsistenz �ber alle Knoten gew�hrleistet [Ray09].

F�r die Transaktionen werden Undo- und Redo Logs verwendet um die Transaktion erfolgreich abzuschlie�en oder abzubrechen. Bei einem �Commit� wird der Redo Log verwendet, und die Transaktion persistent durchgef�hrt. Bei einem �Abort� wird der Undo Log verwendet, um die Transaktion r�ckg�ngig zu machen. Jegliche Log-Eintr�ge in das Logbuchen m�ssen erst vollst�ndig persistent geschrieben werden, bevor weitere Aktionen ausgef�hrt werden d�rfen. Fehler auf der anderen Seite werden nur durch Timeouts erkannt. Dabei wird die Annahme getroffen, dass der jeweilige Knoten, der nicht mehr erreichbar ist, durch einen �Soft-Crash� nicht mehr reagiert. Ein abgest�rzter Knoten kann durch einen Neustart oder anderweitige Aktionen wieder in einen Zustand gebracht werden, sodass dieser eine Transaktion fortsetzen kann.

Der Knoten, der ein Atomic Commit Protokoll startet und leitet ist der Koordinator, w�hrend alle weiteren Knoten die Teilnehmer sind.
