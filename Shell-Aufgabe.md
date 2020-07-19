## Lösung der Shell-Aufgabe

Die zu bereinigende Datei „2020-05-23-Article_list_dirty.tsv“ wurde in einen Ordner mit der Bezeichnung „Shell-Aufgabe“ abgelegt, welcher sich auf meinem Desktop befindet. Diesen rufe ich zunächst auf:

__BEN@BEN MINGW64 ~
$ cd Desktop/Shell-Aufgabe__

Anschließend lasse ich mir den Inhalt anzeigen. Dabei sollte mir nur um die Datei 2020-05-23-Article_list_dirty.tsv“  handeln.

__BEN@BEN MINGW64 ~/Desktop/Shell-Aufgabe
$ ls
2020-05-23-Article_list_dirty.tsv__

__Im nächsten Schritt lasse ich mir den Inhalt der Datei anzeigen:
BEN@BEN MINGW64 ~/Desktop/Shell-Aufgabe
$ cat 2020-05-23-Article_list_dirty.tsv                                                                                 Creator Issue   Volume  Journal ISSN    ID      Citation        Title   Place Labe      Language        PublisherDate
Andrews, E. M.  2       41      AUSTRALIAN JOURNAL OF POLITICS AND HISTORY      ISSN: 0004-9522 (Uk)RN000249208 AUSTRALIAN JOURNAL OF POLITICS AND HISTORY 41(2), 239-252. (1995)       """For Australia's Wartime Interests"": W. M. Hughes and the Push Against Asquith, Britain, March-July 1916"    at      eng     THE UNIVERSITY OF QUEENSLAND PRESS      1995__

In dieser Form ist die Tabelle schwer lesbar und macht das Arbeiten umständlicher. Ich entferne also zunächst überfüssige Zeilen:

__BEN@BEN MINGW64 ~/Desktop/Shell-Aufgabe
$ less -S 2020-05-23-Article_list_dirty.tsv__

Von den angezeigten Spalten sind nur zwei für die Aufgabe relevant, die Spalte „ISSN“ (5) sowie  die Spalte „Date“ (12). Auf diese reduziere ich die Tabelle und speichere mein Ergebnis in einer neuen Datei („2020-05-23-Article_list_dirtyCUT.dsv“):

__BEN@BEN MINGW64 ~/Desktop/Shell-Aufgabe
$ cut -f 5,12 2020-05-23-Article_list_dirty.tsv > 2020-05-23-Article_list_dirtyCUT.tsv__

Meine Datei enthält nun nur noch die Spalten „ISSN“ und „Date“.
In den weiteren Schritten geht es nun darum, diese Spalten zu bereinigen. Es fallen diverse   Unsauberkeiten auf: einige Zeilen enthalten nur den Eintrag „eng“, oftmals sind der ISSN verschiedenste Einträge vorangestellt („ISSN:“, „issn:“, teilweise mit Leerzeichen) etc.
Wir entfernen zunächst „eng“ und erstellen eine neue Datei:

__BEN@BEN MINGW64 ~/Desktop/Shell-Aufgabe
$ grep -v eng 2020-05-23-Article_list_dirtyCUT.tsv > 2020-05-23-Article_list_dirtyGREP.tsv__

Anschließend konzentrieren wir uns auf Leerzeichen. Um diese möglichst großflächig „abzuarbeiten“, bediene ich mich der regulären Ausdrücke [siehe z.B. hier)[ https://wiki.ubuntuusers.de/grep/]. Ich verwende alledings hier den Befehl „sed“:

__BEN@BEN MINGW64 ~/Desktop/Shell-Aufgabe
$ sed '/^\s*$/d' 2020-05-23-Article_list_dirtyGREP.tsv > 2020-05-23-Article_list_dirtySED.tsv__                           

Nun bleibt noch der „ISSN“-Eintrag in seinen verschiedenen Varianten. Wieder verwende ich den „sed“-Befehl. Ich nutze den Befehl weiterhin, um den Eintrag „Date“ zu entfernen:

__BEN@BEN MINGW64 ~/Desktop/Shell-Aufgabe
$ sed 's/issn//ig;s/:/ /g;s/date//ig' 2020-05-23-Article_list_dirtySED.tsv > 2020-05-23-Article_list_dirtyNEWSED.tsv__

Nach der Ausführung bleiben noch einige Leerzeichen zurück, die im zu den „ISSN“-Zeilen gehören und in meinem vorherigen Befehl nicht berücksichtigt wurden. Ich nutze nochmals einen „sed“-Befehl, um diese zu bereinigen:

__BEN@BEN MINGW64 ~/Desktop/Shell-Aufgabe
$ sed 's/ //g' 2020-05-23-Article_list_dirtyNEWSED.tsv > 2020-05-23-Article_list_dirtyLASTSED.tsv__

Ich wende nun den „sort“-Befehl an. In Verbindung mit „–u“ werden doppelte und somit redundante Zeilen entfernt:

__BEN@BEN MINGW64 ~/Desktop/Shell-Aufgabe
$ sort -u 2020-05-23-Article_list_dirtyLASTSED.tsv > 2020-05-23-Article_list_dirtySORT.tsv__

Danach möchte ich die Liste nach den ISSNs sortieren:

__BEN@BEN MINGW64 ~/Desktop/Shell-Aufgabe
$ sort -n 2020-05-23-Article_list_dirtySORT.tsv > 2020-05-23-Article_list_dirtyNEWSORT.tsv__

Die Datei „2020-05-23-Article_list_dirtyNEWSORT.tsv“ entspricht dem Ergebnis und wird in “2020-05-23-Dates_and_ISSN.tsv“ umbenannt. 


