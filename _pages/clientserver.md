---
title: Einführung in Client/Server Systeme
author: Peter Braun
weight: 0
---

Herzlich willkommen zur Einheit über Client-Serversysteme. Wir wollen uns in dieser Einheit zunächst grundsätzlich mit der Architektur von Client-Server Systemen beschäftigen und die besonderen Herausforderungen betrachten, die wir bei der Programmierung von solchen Systemen beachten müssen. Anschließend werden wir die Programmierung von solchen Systemen in der Programmiersprache Java üben. Dabei betrachten wir zunächst den einfachen Fall einer synchronen Kommunikation. Sie werden an einigen Aufgaben auch selber Lösungen erarbeiten können. Am Ende betrachten wir dann auch noch kurz die asynchrone Kommunikation und bearbeiten auch wieder gemeinsam eine Aufgabe. 

——————————————————

Schauen wir uns zunächst an wie ein Client-Server System grundsätzlich aufgebaut ist. Starten wir dafür auf der untersten Ebene. Zunächst einmal geht es um ein verteiltes System d. h. wir haben mindestens zwei physikalische oder virtuelle Computer. In der Grafik benennen wir diese Computer erst mal ganz einfach mit dem Wort „Box“. Diese beiden Boxen sind über ein Netzwerk miteinander verbunden.

Um aus diesen beiden Boxen, die bislang lediglich aus etwas Hardware und miteinander über eine Netzwerk Schnittstelle verbunden sind, ein System zu machen, brauchen wir noch Software. Ohne die Software haben wir kein Client-Server System oder anders formuliert erst die Software macht aus zwei Computern ein funktionsfähiges Client-Server System.

Mit den beiden Begriffen Client und Server beschreiben wir die Rollen, die die beiden Computer in diesem verteilten System einnehmen. Fangen wir mit dem Server an. Der Server, bzw. genauer gesagt die Software die auf der Box läuft, die jetzt die Rolle des Servers einnimmt, stellt einen Dienst zur Verfügung. Unter einem Dienst kann man sich jetzt zunächst einmal alles vorstellen. Das kann ein Drucker sein, der wiederum physikalisch an diesem Computer angeschlossen ist und von der Software angesteuert werden kann. Dies kann aber auch ein Filesystem sein und die Software auf dem Server ist in der Lage, Dateien zu speichern, zu löschen und natürlich wiederzufinden. Oder aber es ist ein Dienst im Sinne eines Algorithmus der also bestimmte Parameter entgegennimmt und aus diesen Eingaben eine bestimmte Ausgabe produziert.

Der Computer, der die Rolle des Clients ein nimmt, ist nun der Kunde dieses Service. Auf diesem Computer läuft eine Software, die für einen ordnungsgemäßen Ablauf genau die Dienste benötigt, die ein Server bereitstellt.

Aus dieser Rollenverteilung wird nun auch klar, dass eine Kommunikation zwischen Client und Server immer vom Client ausgeht. Es ist der Client, der eine Anfrage an den Server sendet und dann auf die Antwort des Servers wartet mit seinen eigenen Operationen anschließend fortführen zu können.

Diese Rollenverteilung zwischen Client und Server hört sich zunächst sehr einfach an und genau darin liegt auch sein großer Vorteil. Die Rollenverteilung ist klar und sie ändert sich über die Lebenszeit der Rechner auch nicht. Vor allem die Festlegung, dass eine Kommunikation immer vom Client ausgeht, nie vom Server, macht die Programmierung solcher Systeme einfach.

Trotzdem ist die Programmierung nicht trivial. In der Praxis haben wir viele Herausforderungen bei der Programmierung von solchen System, die unbedingt berücksichtigt werden müssen.

———————————————————

Schauen wir uns einige der Herausforderung mal etwas genauer an.

Zunächst betrachten wir die Herausforderung der Heterogenität in den realen System. Stellen Sie sich zum Beispiel vor, dass der Client auf einem Rechner mit dem Betriebssystem Windows läuft, der Server aber auf einem Linux Betriebssystem. 
Sie sind bestimmt alle schon mal über die unterschiedlichen Konventionen bei der Groß- und Kleinschreibung von Dateinamen oder die Zeichen zum Trennen von Verzeichnissen in einem Dateipfad gestolpert. 

Oder stellen Sie sich vor, dass Client und Server in unterschiedlichen Programmiersprachen geschrieben sind. Ein Datentyp Integer in der Programmiersprache C muss nicht unbedingt das gleiche bedeuten wie ein Datentyp Integer in der Programmiersprache Java, da die Wertebereiche unterschiedlich sein können. Oder betrachten Sie unterschiedliche Kodierungen für Zeichenketten. In den eher hardwarenahen Sprachen werden Zeichenketten mit einem null Zeichen terminiert, wohingegen in anderen Sprachen die Länge der Zeichenkette mitgespeichert wird. Auch können die Zeichensätze auf den beiden Computer unterschiedlich sein. Bei der Übertragung von Daten zwischen Client und Server müssen diese unterschiedlichen Datenformate oder Datenkodierungen beachtet werden. 

Eine zweite Herausforderung, mit der wir uns bei Client-Server Systemen beschäftigen müssen, stellt der große Bereich der Performance dar. Die Übertragung von Daten zwischen Client und Server braucht nun mal Zeit und ist, auch wenn wir uns im Bereich der Gigabit Netzwerke bewegen, immer noch langsamer als wenn wir nur Daten zwischen zwei Prozessen auf dem gleichen Computer verschicken würden. Diese Herausforderung wird sogar noch größer, wenn unser System bestehend aus dem Client und dem Server auf unterschiedlichen Kontinenten verteilt ist. Hierbei kann das Verschicken einer Nachricht auch schon mal einige Sekunden dauern. Bei der Programmierung vor allem des Clients muss man daher auf solche Situationen vorbereitet sein, dass Nachrichten eventuell erst mit einem großen Verzug eintreffen.

Immer dann wenn wir Nachrichten über ein Netzwerk verschicken, ist natürlich auch die Sicherheit eine große Herausforderung. Im einfachsten Fall haben wir Client und Server in einem lokalen Netzwerk miteinander vernetzt, dass wir auch unter unserer vollen Kontrolle haben. Das ist aber nur ein sehr einfacher Fall. Wenn Client und Server in unterschiedlichen Netzwerken organisiert sind und vielleicht sogar weltweit verteilt sind, haben wir als Entwickler keine Möglichkeit der Kontrolle was mit unseren Daten auf dem Weg zwischen Client und Server alles passieren könnte. Daher müssen wir selbst Vorkehrungen treffen, damit die Nachrichten entsprechend gegen Manipulation, Verfälschung aber auch dem einfachen Mithören geschützt ist.

Schließlich haben wir auch noch die Herausforderung, dass in einem Client-Server System viele Dinge auch einmal nicht funktionieren können. Zum Beispiel kann der Client abstürzen. Das ist das vergleichsweise einfachste Problem, bei dem der Server eben keine Nachrichten mehr von diesem Client empfängt, aber ansonsten von diesem Problem nicht weiter berührt ist. Schwerwiegender ist der Fall dass die Netzwerkkommunikation zwischen Client und Server gestört oder sogar über längere Zeit unterbrochen ist. Damit hört das Client-Server System eigentlich auf zu funktionieren, denn beide Teile sind ja aufeinander angewiesen. Der Client kann ohne den Server nicht arbeiten und der Server ist ohne Anfragen vom Client relativ sinnlos. Schließlich kann auch der Server abstürzen und das ist sicherlich ein schwerwiegendes Problem, denn der Client kann ohne den Server nicht mehr vernünftig arbeiten. 

Bei allen diesen drei möglichen Fehlerfällen müssen wir als Entwickler zum einen feststellen, dass aktuell überhaupt ein Fehler vorliegt und wir müssen entscheiden, wie wir mit dem Fehler umgehen. Das bedeutet, wir müssen für unsere Applikation definieren, wie viel Zeit im Normalfall ein Kommunikationsschritt zwischen Client und Server benötigen darf und ab welcher Zeitdauer wir unserem Nutzer auf dem Client dann eine Fehler-Mitteilung senden und ihn darüber informieren, dass entweder das Netzwerk oder der Server gerade nicht verfügbar sind. Unter Umständen ist es auch sinnvoll in bestimmten Fehlersituationen den Nutzer nicht vorschnell darüber zu informieren dass ein schwerwiegendes Problem vorliegt. Wir könnten also überlegen, durch welche Maßnahmen wir bestimmte Fehler Fälle auch ausblenden oder überspielen können. Zum Beispiel könnte man eine über kurze Zeit unterbrochene Netzwerkverbindung dadurch kompensieren, dass der Endbenutzer erst mal nur lokal auf dem Client arbeitet und später, nach der Wiederherstellung der Netzwerkverbindung die Daten für den Endbenutzer vollständig unsichtbar, von der Applikation selbst mit dem Server wieder synchronisiert werden. Dies sind allerdings sehr aufwändige Algorithmen, die für solche Schritte notwendig sind.

————————————————————

Wir wollen uns in dieser Einheit auch mit den beiden häufigsten Arten von Client-Server Kommunikation beschäftigen. Fangen wir auch hier mit dem einfachsten Fall an: der synchronen Kommunikation. 

Sie sehen in dieser Grafik die einzelnen Verarbeitungsschritte, die Client und Server durchlaufen müssen. Der Client startet die Applikation und versucht nun als erstes eine Anfrage an den Server zu senden. Der Pfeil nach rechts soll andeuten das hier nun eine Kommunikation stattfindet, d. h. ein Nachrichtenpaket wird über das Netzwerk verschickt. Der Pfeil verläuft nicht horizontal sondern leicht nach unten, um wie in einem UML Sequenzdiagramm den Zeitverzug zu symbolisieren. 

Die Nachricht wird dann vom Server empfangen, die in der Nachricht codierte Anfrage wird ausgeführt und der Server schickt eine Antwort zurück an den Client. 

Während der Server mit der Verarbeitung der Anfrage beschäftigt ist, wartet der Client. Dies können Sie sich sehr einfach zum Beispiel wie ein aktives Warten vorstellen in dem in einer Schleife ständig geprüft wird, ob die Antwort des Servers endlich vorliegt. So wird das natürlich in der Praxis nicht gelöst. Auf der Betriebssystemebene wird der Prozess in einen Zustand „blockiert“ versetzt, was aber auf jeden Fall bedeutet, dass er nicht weiter arbeitet, denn er wüsste ja auch nicht woran. Er wartee auf ein Signal, dass eine Antwort vom Server eingetroffen ist. Der Prozess schläft also in der Zeit. 

Wenn Sie sich nun vorstellen, dass der Client eine grafische Oberfläche hat, bei dem die Endbenutzer beliebige Events, wie zum Beispiel das Drücken auf einen Button, ausführen können, wird klar, das dies keine sinnvolle Lösung sein kann. Die eingehenden Events würden ja in der Zeit des Wartens nicht verarbeitet werden, die grafische Oberfläche wurde nicht aktualisiert, der Nutzer würde kein Feedback erhalten, dass das Drücken auf einen Button irgendein Effekt hat. In solchen Clients, die also eine grafische Oberfläche beinhalten, sollte die synchrone Kommunikation daher nicht verwendet werden. 

Aber zurück zum Ablauf der Client-Server Kommunikation. Wenn nun die Antwort des Servers am Client eintrifft wird diese entsprechend verarbeitet und damit ist der Kommunikationsschritt zu Ende.

————————————————————

Wir wollen uns im folgenden zunächst auf die synchrone Kommunikation konzentrieren und schauen uns dazu an, wie man eine solche ganz einfache Kommunikation über TCP/IP in Java implementieren könnte.

Zunächst müssen wir noch einige Grundlagen aus dem Bereich der Netzwerkkommunikationtion wiederholen.

Wir gehen davon aus, dass jeder Rechner eine mehr oder weniger eindeutige IP-Adresse hat. Wir vernachlässigen an der Stelle zunächst solche Fälle, in denen sich die Computer periodisch neue IP-Adressen geben lassen. Außerdem gehen wir davon aus, dass der Rechner einen Namen haben kann, der über ein Verzeichnisdienst wie zum Beispiel DNS zuverlässig auf die IP-Adresse des Computers abgebildet werden kann.

Mit einer solchen IP-Adresse kann jetzt ein Client zumindest schon einmal einen Server ansprechen, allerdings würde dies bedeuten, dass auf jedem Rechner nur ein Service angeboten werden kann. Damit nun unterschiedliche Services auf dem gleichen Rechner unterschieden werden können, benötigen wir noch ein weiteres Konzept zur Unterscheidung und dies nennen wir Ports. Damit kann es also jetzt auf dem Server mehrere Softwaresysteme geben, die vollkommen unabhängig voneinander auf verschiedenen Ports auf eingehende Anfragen warten und auf der anderen Seite kann ein Client über die Angabe der Portnummer den Service auswählen mit dem er kommunizieren möchte.

Das Konzept der Ports wird nicht nur auf dem Server sondern auch auf dem Client verwendet. D. h. bei jeder ausgehenden Verbindung vom Client wird ebenfalls ein Port auf dem Client reserviert. Damit haben wir eine logische Verbindung zwischen Client und Server und dem Port auf dem Client mit dem jeweiligen Port auf dem Server. Damit ist klar dass es nicht zwei gleichzeitige Verbindungen geben kann die zwischen den gleichen Portnummern auf dem Client zum gleichen Port auf dem Server führen. Aber es ist natürlich möglich das von einem Client mehrere Verbindungen auf unterschiedliche Server geöffnet werden können. Und es auch möglich, dass ein Server mehrere Verbindungen von unterschiedlichen Clients auf dem gleichen Port bearbeiten kann. 

————————————————————

Um eine einfache TCP/IP Verbindung in Java zu programmieren, brauchen wir jetzt als nächstes das Konzept der Sockets. 

Ein Socket abstrahiert von der konkreten physikalischen Verbindung und stellt einen logischen Endpunkt dar, bestehend aus der IP-Adresse und dem Port. Für eine Verbindung zwischen Client und Server braucht es also einen Socket auf dem Client und einen Socket auf dem Server. Java stellt hierfür entsprechend Klassen bereit, auf dem Client heißt die Klasse einfach Socket und auf dem Server heißt sie Server-Socket. 

In Java ist übrigens eine IP-Adresse nicht einfach ein String, auch dafür gibt es eine Klasse mit dem Namen INetAddress. Schauen wir uns jetzt ein Beispiel an, wie man in Java eine einfache TCP/IP Verbindung über Sockels zwischen einem Client und einem Server realisieren kann.

————————————————————

Fangen wir mit dem Server an. 

Der Server legt in Zeile 2 einen Socket an, der auf Port 6789 gebunden sein soll. Der Server soll nun unendlich lang laufen, deswegen starten wir in Zeile 4 eine Schleife um auf eingehende Verbindungsanfragen auf diesem Port zu lauschen. Das machen wir in Zeile 5 in dem wir die Methode „accept“ aufrufen. An der Stelle wartet der Server nun auf eine eingehende Verbindungsanfrage, d. h. die Methode blockiert und wir bleiben erst mal in Zeile 5 stehen. 

Sobald nun ein Client eine Verbindung auf diesen Port geöffnet hat, arbeitet der Server in Zeile 6 weiter. Wir verpacken den sogenannten Inputstream, über den die Daten des Clients bei uns am Server ankommen in weitere Objekte, die uns das Lesen von Textnachrichten vereinfachen. Schließlich lesen wir in Zeile 8 alle Zeichen bist zum ersten Zeilentrenner und speichern die eingehende Nachricht in der Variablen Input.

Diese geben wir in Zeile 10 zur Demonstration auf dem Bildschirm aus und konvertieren in Zeile 12 alle Zeichen der eingehenden Nachricht in Großbuchstaben. Dies ist praktisch unser Service den wir anbieten. Anschließend müssen wir das Ergebnis über den gleichen Kanal wieder zurückschicken. D. h. wir holen uns von dem Socket den Outputstream, verpacken ihn wieder so, dass wir einfach Text in den Stream reinschreiben können, das passiert in Zeile 15 und kennzeichnen das Ende unserer Übertragung mit der <em>Zeilenschaltung</em>. Mit der Methode „Flush“ in Zeile 16 informieren wir den Socket darüber, dass nun alle Daten an den Client geschickt werden können und schließlich schließen wir die Socket Verbindung. Aufgrund der unendlichen Schleife landen wir dann wieder in Zeile fünf und warten auf eine neue eingehende Verbindung.

—————————————————————

Der Java Code für den Client sieht dann folgendermaßen aus: zunächst verpacken wir den Inputstream für die Tastatureingaben in Zeile 2 so, dass das Lesen der Eingabe etwas einfacher wird. In Zeile 3 legen wir einen Socket an, bei dem der erste Parameter der Hostname des Servers ist. Bitte nicht verwirren lassen: wir verwenden an dieser Stelle localhost, weil der Server ja auf dem gleichen Rechner laufen wird, auf dem wir gerade entwickeln und auf dem der Client laufen wird. Wenn wir in einem echten verteilten System mit mehreren Rechnern arbeiten würden, würde an dieser Stelle die IP-Adresse oder der Name des entfernten Servers stehen. Wir wollen eine Verbindung aufmachen zum Port 6789 auf dem Server.

In Zeile 5 geben wir die ausgehende Verbindung zum Server an, über die wir dann später unsere Anfrage an den Server schicken wollen. In den Zeilen 6 und 7 legen wir jetzt schon die eingehende Verbindung für die Antwort an.

In den Zeilen 9 und 10 fragen wir vom Nutzer über die Tastatur eine Eingabe ab und schicken sie in den Zeilen 12 und 13 an den Server. Schließlich warten wir in Zeile 15 auf die Antwort vom Server, lesen diese ein und speichern die Antwort in der Variablen „Output“. Und zur Demonstration zeigen wir das Ergebnis am Ende in Zeile 17 auf dem Bildschirm an.

——————————————————————

So, jetzt sind Sie an der Reihe, das erste Client-Server System ans Laufen zu bringen. Klonen Sie bitte das Git Repository, das Sie mit eine Klick auf den Button erreichen können. Importieren Sie das Verzeichnis als Maven Projekt in Ihre Entwicklungsumgebung und schauen Sie sich die beiden Klassen an. Es ist genau der Quellcode, den wir auf den letzten beiden Folien besprochen haben. 

Starten Sie als erstes die Main Methode aus der Klasse TCPServer und danach die Main Methode aus der Klasse TCP Client. Trippen Sie dann auf der Console einige Buchstaben udn schauen Sie sich das Ergebnis an. 

——————————————————————

Als erstes wollen wir nun den Code etwas verbessern, so dass wir nicht nur eine Nachricht verschicken können, sondern mehrere. Wir verschieben also den Code zum Senden einer Nachricht in eine eigene Methode, die wir dann aus der Main Methode des Clients mehrfach aufrufen können. 

Versuchen Sie es. Wenn Sie fertig sind, klicken Sie bitte auf Weiter, dann besprechen wir gemeinsam die Lösung.

——————————————————————

In der zweiten Aufgabe wollen wir nun probieren, nicht nur einen einzelnen Parameter vom Typ String zu verschicken, sondern auch noch einen zweiten Parameter vom Typ Integer. Überlegen Sie bitte, was an dem Quellcode geändert werden muss, damit wir eine Nachricht bestehend aus zwei Parametern verschicken können. 

Klicken Sie auf Weiter, wenn Sie sich die Lösung ansehen wollen. 

——————————————————————

Lassen Sie uns an dieser Stelle noch kurz besprechen, was wir da gerade gemacht haben. 

Im ersten Beispiel haben wir gesehen wie man eine einzelne Textzeile vom Client an den Server schicken kann und auch die Antwort war wiederum eine einfache Textzeile. In der Übung haben Sie sich jetzt Gedanken darüber gemacht, wie man auch mehrere einzelne Daten zwischen Client und Server versenden kann. Sie haben dabei bemerkt das es irgend eine Möglichkeit geben muss, die einzelnen Parameter oder Werte so zu trennen, dass der Server sie auch wieder unterscheiden kann. Das ist so ähnlich wie das Versenden von vielen kleinen Gegenständen in einem großen Paket. Der Client muss also die einzelnen Werte in ein Paket einpacken und der Server muss das Paket auspacken, um die einzelnen Werte wieder zu erhalten.

Dies ist ein ganz grundsätzliches Problem, für das es in der Praxis natürlich sehr viele verschiedene Möglichkeiten gibt. Allgemein bezeichnet man diese beiden Aktionen tatsächlich als einpacken und auspacken oder auf Englisch Marshaling und Unmarshalling.

In der Programmiersprache Java haben sich auch noch die Begriffe „serialisation“ und „de-serialisation“ eingebürgert.

Wenn wir Java als Programmiersprache verwenden bietet uns die Programmiersprache für diese beiden Aktionen entsprechende Klassen mit dem Namen DataInputStream und DataOutputStream an. Diese beiden Klassen bieten entsprechend Methoden zum Schreiben von allen primitiven Datentypen und natürlich das korrespondierende Lesen von solchen Werten. Aber es geht sogar noch weiter: man kann sogar einfach ein Java Objekt oder sogar einen ganzen Objektgrafen in einen solchen Stream schreiben und Java kümmert sich dann selber darum, wie die einzelnen Werte verpackt werden. Das ist eine sehr einfache Methode, aber das funktioniert eben nur, wenn sowohl Client als auch Server tatsächlich als Programmiersprache Java verwenden.

Schauen wir uns das im Folgendem an einem Beispiel an.

——————————————————————

Wenn wir Programmiersprachen unabhängig sein möchte gibt es andere Datenformate, die wir nutzen könnten. Wir werden darauf später noch mal genauer eingehen, daher hier nur ein erster Hinweis. Die aller einfachste Methode ist es die einzelnen Werte einfach in eine Textzeile zu schreiben und durch ein Semikolon zu trennen. Dabei schreiben wir aber nur die Werte in die Textzeile und müssten zwischen Client und Server vorab vereinbaren, an welcher Position welches Datum stehen wird. 

Etwas eleganter sind daher die Datenformate XML oder JSON, bei denen nicht nur die Werte sondern auch die Namen der Variablen bzw. Attribute codiert werden und es sogar möglich ist hierarchische Datenstrukturen zu verpacken.

Das schauen wir uns jetzt direkt an einem Beispiel an.


————————————————————

Um die Probleme mit der synchronen Kommunikation gerade in solchen Fällen wenn der Client eine grafische Oberfläche hat oder auf andere Arten direkt mit dem Nutzer interagiert zu umgehen, betrachten wir nun noch abschließend die asynchrone Kommunikation. 

Der Kommunikationsschritt beginnt in ähnlicher Weise, der Client startet und schickt eine Anfrage an den Server. Jetzt kommt aber schon der Unterschied. Der Client wird nun nicht in den blockierenden Zustand versetzt, sondern kann mit anderen Aktivitäten sofort weiterarbeiten. Das Warten auf die Antwort des Servers findet praktisch parallel in einem eigenen Thread zur weiteren Abarbeitung der Applikation auf dem Client statt. Wieder im Fall einer grafischen Oberfläche, der Client kann nun auf Klicks reagieren und der Endbenutzer hat den Eindruck die Applikation lebt noch. Parallel dazu findet die Verarbeitung der Anfrage auf dem Server statt. 

Irgendwann schickt dann der Server die Antwort zum Client und nun geht es noch um die Frage, wie der Client die Antwort verarbeitet. Die Lösung bezeichnet man als Callback. Als der Client die Anfrage zum Server geschickt hat, hat er bereits definiert, welche Funktion ausgeführt werden soll, wenn dann die Antwort vom Server eintrifft. Diese Funktion wird nun parallel zur eigentlichen Applikation gestartet, die Antwort wird verarbeitet, eventuell müssen Daten auf der grafischen Oberfläche aktualisiert werden. Wenn all dies entsprechend erfolgt ist, endet die Verarbeitung dieser Funktion und auch der Thread ist damit zu Ende.

——————————————————————

Auch für die Programmierung einer asynchronen Kommunikation schauen wir jetzt noch ein Beispiel an. 

——————————————————————

Das war's schon. Vielen Dank für Ihr Interesse an der Einführung in die Programierung eines einfachen Client-Server Systems in Java. Sie haben heute zwei unterschiedliche Arten der Kommunikation, nämlich die synchrone und die asynchrone Kommunikation kennengelernt. Sie haben erste Beispiele implementiert, wie man in Java Nachrichten über ein Socket von einem Client zu einem Server und wieder zurück schicken kann. 

Auf diese Weise wurden tatsächlich in der Vergangenheit solche verteilten Systeme implementiert. Aber nicht lange, denn die Programmierung ist dann doch in komplexeren Situation etwas unhandlich. In der nächsten Einheit schauen wir uns deshalb Methoden an, wie wir die Client-Server Programmierung für uns Entwickler einfacher machen können.



