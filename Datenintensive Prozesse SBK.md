#  Datenintensive Protesse bei der Stadtbibliothek Köln

Autor: Philipp Bültmann

## 1	Einleitung

Die Stadtbibliothek Köln verfügt über einen physischen Bestand von ca. 800.000 Medien sowie zahlreiche digitale Angebote. Sie besteht aus einer Zentralbibliothek und  elf Zweigstellen und einem Bibliotheksbus. Als Library Management System wird das Programm „Concerto“ der Firma Bibliomondo genutzt. Die verschiedenen Bibliotheken sind dabei über ein Corporate  Network miteinander verbunden, um allen den Zugriff zum Library Management System sowie zu anderen Dienste zu ermöglichen. 

## 2	Datenintensive Prozesse

In den folgenden Abschnitten sollen die datenintensiven Prozesse der Stadtbibliothek Köln vorgestellt werden. Dabei wird zwischen rein bibliothekarischen Prozessen sowie allgemeinen IT-Prozessen unterschieden. Diese Unterscheidung spiegelt sich auch in den Aufgabengebieten der verschiedenen Mitarbeitenden der Stadtbibliothek wider.

### 2.1	Bibliothekarische Prozesse

Fremddatenübernahme 
Im Rahmen der Fremddatenübernahme importiert die Stadtbibliothek verschiedene Daten in die Datenbanken des Library Management Systems (LMS). Dies geschieht, um unter anderem die Katalogisierung zu vereinfachen. Die Daten der DNB Reihen A, N; T und M werden z.B. in den Katalog des LMS  geladen. Die für diesen Import benötigte Daten werden von der Stadtbibliothek über den FTP-Server des HBZ heruntergeladen. Danach werden diese zunächst durch ein Konventierungstool umgewandelt, um das Datenformat in ein für Concerto passendes Format zu ändern. Danach können die Daten in eine dafür speziell vorgesehene Datenbank des LMS kopiert werden.

Daten-Korrektur
Im Prozess der Neuanmeldung eines Benutzers bzw. einer Benutzerin, werden verschiedene personenbezogene Daten erfasst. Diese Daten werden vom Personalausweis des zukünftigen Nutzenden abgelesen und händisch in ein entsprechendes Anmeldemenü des LMS eingetragen. Bei diesem Prozess kann es zu fehlerhaften Eingaben durch die Mitarbeitenden kommen. Dies äußert sich besonders negativ in Bezug auf die Adressdaten, da diese insbesondere für die Bearbeitung von Geldmahnungen absolut zutreffend übernommen sein müssen. Nach entsprechender Fehlermeldung der Software, korrigieren die Mitarbeitenden der Rechnungsstelle der Bibliothek diese Daten manuell im LMS. Dies geschieht für gewöhnlich einmal im Monat und nimmt mehrere Stunden Arbeitszeit eines Mitarbeitenden in Anspruch.
 
 ### 2.2	.IT-Prozesse

Datensicherung 
Die Stadtbibliothek Köln pflegt ein eigenständiges Netz mit entsprechender Server-Client-Umgebung. In diesem kommt neben dem oben erwähnten LMS Concerto eine Microsoft Active-Directory Umgebung zum Einsatz. Für das Aufgabengebiet der Datensicherung stehen verschiedene Techniken zur Verfügung. Die Datenbank des LMS sowie ein Domänen-Controller, welche das Kernstück einer Microsoft Domänen bilden, werden über die Software BackupExec von Veritas auf Linear Tape Open(LTO)  Magnetbänder gesichert. Die Sicherung wird nächtlich durchgeführt und benötigen mehrere Stunden. Diese Magnetbänder werden anschließend in einem feuerfesten Tresor für vier Wochen aufbewahrt. Zusätzlich zu dieser Form der Sicherung werden Kopien der übrigen Server auf einen separaten Netzwerkspeicher exportiert. Dies geschieht je nach Server täglich bzw. einmal pro Woche und wird durch ein spezielles Powershell-Skript ermöglicht. Diese Sicherungsskripte arbeiten in zwei Schritten: Zunächst wird der Server im Live-Betrieb in wenige, dafür relativ große Dateien exportiert. In einem zweiten Schritt wird dieser Export auf den Netzwerkspeicher kopiert und verbleibt dort bis zur nächsten Sicherung.
Routine Tätigkeit „Operating“
Der EDV-Abteilung der Stadtbibliothek fallen bestimmte, dauerhaft wiederkehrende Aufgaben zu, die für gewöhnlich morgens erledigt werden: 
•	Der Kassenbericht wird auf einem Netzwerk-Share veröffentlicht
•	Die LTO-Bänder werden kontrolliert und ausgetauscht
•	Die postalischen Benachrichtigungen für die Nutzenden werden gedruckt 
Gerade dieser Prozess des morgendlichen Operatings bietet Verbesserungspotenziale und soll im nächsten Abschnitt genauer betrachtet werden.

## 3	Beispielprozess "Operating"

Wie oben beschrieben teilen sich die Routinetätigkeiten in benannte drei Bereiche auf. Dabei ist die Tätigkeit der Kontrolle und des Austausches der LTO-Bänder nur im entfernteren Sinne ein datenintensiver Prozess und soll daher hier für die weitere Behandlung nicht betrachtet werden. Die anderen beiden Teilschritte sollen zunächst detaillierter betrachtet werden.
Das LMS der Stadtbibliothek Köln erstellt nach seiner nächtlichen Konsolidierung mit dem Tool CyberQuery verschieden Statusberichte, beispielsweise Transaktionszaählungen, Vormerkungen, Geld- sowie Medienmahnungen. Diese Berichte werden in einem speziellen proprietären Dateiformat erstellt und können zunächst nur mit dem entsprechenden Tool geöffnet werden. Einige Berichte bilden die Grundlagen von E-Mail-Benachrichtigungen welche automatisch an die Nutzenden verschickt werden, die keine postalischen Benachrichtigungen wünschen. Für die restlichen Nutzenden müssen diese Benachrichtigungen ausgedruckt, sortiert und an die Poststelle weitergeleitet werden.
Ähnlich wie die Benachrichtigung der Nutzenden liegt auch der Kassenbericht zunächst nur in einem proprietären Dateiformat vor. Er wird daher von einem Mitarbeitenden in das PDF-Format konvertiert. Zur besseren Übersicht und besseren Unterscheidung der einzelnen Kassenberichte wird der Dateiname mit dem aktuellen Datum versehen Als letzten Schritt wird der als PDF exportierte Kassenbericht auf einem internen Webserver abgelegt und ist für andere Mitarbeitende über einen Browser abrufbereit. 
Dieser beschriebene Prozess ist durch die starke Interaktion mit dem Mitarbeitenden gekennzeichnet und eignet sich deshalb gut für eine mögliche Automatisierung. Vorstellbar wäre der Einsatz eines Skriptes, welches den Prozess ohne Interaktion mit einem oder einer Benutzende übernehmen könnte. 
Zunächst müsste das Skript das proprietäre Dateiformat des Berichtes in ein PDF-Format konvertieren, damit es von jedem Arbeitsplatz geöffnet werden kann. Vorstellbar wäre, dass das Skript dafür auf einen PDF-Drucker zurückgreift. Als Nächstes müsste das Skript das aktuelle Datum ermitteln und den Dateinamen des gerade erstellten Berichtes dahinlegend anpassen. Dabei sollte das aktuell verwendete Format „TK_TAG-MONAT-JAHR.pdf“ erhalten bleiben. TK steht hier als Abkürzung für die Tageskasse. Als letzten Schritt müsste das Skript den Tageskassenbericht kopieren und auf dem freigegebenen Netzwerk-Share ablegen. Durch ein solches Skript müssten Mitarbeitende nicht mehr in den Prozess eingreifen und die Tätigkeit wäre weitestgehend automatisiert. 
Um zusätzlich zu vermeiden, dass Mitarbeitende das Skript selbst starten müssen, könnte auch dies in einem nächsten Schritt automatisiert werden. Über die Windows Aufgabenplanung oder einen Linux Cron-Job lassen sich Prozesse nach bestimmten Kriterien automatisiert starten. Hierdurch könnte definiert werden das, dass oben vorgestellte Skript kurz nach der Erstellung der Berichte durch das LMS gestartet wird. Dies hätten den weiteren Vorteil, dass Mitarbeitende nicht auf den Kassenbericht warten müssten, sondern auf diesen direkt nach dem Starten ihres Systems zugreifen können.
