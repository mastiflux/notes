## Ein- und Ausgabeströme

- Standardeingabe (STDIN, 0) – hieraus werden die verarbeitenden Informationen gelesen (Java: System.in.read())
- Standardausgabe (STDOUT, 1) – hierhin werden die verarbeiteten Informationen geschrieben (Java: System.out.print())
- Standardfehlerausgaben (STDERR, 2) – hierhin werden Fehlermeldungen geschrieben (Java: System.err.print())

### Umlenkung 

- Umleitung der Standardeingabe mit < ```
```shell
befehl < datei 
```
- Umleitung der Standardausgabe mit >
```shell
befehl > datei // bisherigen Inhalt überschreiben
befehl >> datei // neuen Inhalt anhängen
```
- Umleitung der Standardfehlerausgabe mit 2>
```shell
befehl 2> datei
```
**Beispiel** 
```shell
wc < /etc/passwd
wc < 2>&1 // Standardfehlerausgabe umleiten
```

### Verkettung von Kommandos - Pipes

- mittels einer **Pipe** können zwei Kommandos unidirektional verbunden werden
- die Standardausgabe des ersten Kommandos wird auf die Standardeingabe des zweiten Kommandos gelenkt
- die shell ermöglicht die Verkettung mit dem Pipe Operator (**|**)

**Beispiel** 
```shell
ps -ax | sort --key=5 | less
cut -d: -f 7 /etc/passwd | sort | uniq -c | sort -rn
```

### Programme zur Textbearbeitung
![[Pasted image 20260413144945.png]]

## Benutzer

- Unix verwendet das Konzept der User, um separate Umgebungen zu schaffen und Rechte zu managen
- jeder Nutzer besitzt individuell ein *Heimatverzeichnis* und *Umgebungsvariablen*
- Benutzer werden mit einer *User-ID* identifiziert

```
root - Superuser mit umfassenden Rechten (Prompt meist # statt $)
Standardbenutzer - für die interaktive Arbeit
Systembenutzer - für Systemdienste wie Email, kein Login, keine Shell
```

### Konzept Gruppe

- jeder Benutzer ist einer Hauptgruppe (*primary group*) zugeordnet, zudem kann man Mitglied weiterer Gruppen (*supplementary group*) sein
- die Gruppenzugehörigkeit ist wichtig für den Zugriff auf Dateien, Geräte und Dienste
- Nutzer zu einer Gruppe hinzufügen:
```shell
usermod -aG gruppe benutzer
```

**Typische Systemgruppen:** 
- *sudo* für Benutzer, die sich mit dem Kommando sudo Superuser-Rechte verschaffen dürfen
- *audio, video* für Benutzer, die auf entsprechende Geräte zugreifen dürfen
- *docker* für Benutzer, die Docker (ohne sudo) nutzen dürfen

#### Superuser auf Zeit: sudo

sudo = substitute user do -> Befehle mit Rechten eines anderen Benutzers ausführen (meist *root*), ohne sich als *root* einzuloggen

- Minimalprinzip: dauerhaftes Arbeiten als Superuser vermeiden

**Syntax:**
```shell
sudo befehl // Befehl als root ausführen
sudo -u benutzername befehl // Befehl als anderer Benutzer ausführen
sudo -i // interaktive root-Shell
```

## Dateien und Befehle 

**Dateien:**
- /etc/passwd – diese Datei enthält die Infos zu allen Benutzern
- /etc/shadow – enthält (gesalzene) Hashes der Passwörter 
- /etc/group – enthält die Infos zu allen Gruppen 

**Befehle:** 
- id liefert User ID (UID) und Group ID (GID) des Benutzers 
- users aktive Benutzer ausgeben  
- groups liefert die Gruppenzugehörigkeit 
- whoami eigener Username 
- who eingeloggte Benutzer

### Dateizugriffsrechte

	Jeder Datei ist genau ein Benutzer als Owner und genau eine Group zugeordnet.

- Zuordnung einer Datei zeigt *ls -l*
- Zugriffsrechte werden für drei Benutzerklassen unterschieden: User (Rechte des Besitzers), Group (Rechte der "besitzenden Gruppe"), Others (alle anderen Benutzer)

- mögliche Rechte sind:
- Read r - lesen
- Write w - schreiben
- Execute x - Ausführen

- Zugriffsrechte beziehen sich auf den Inhalt einer Datei

**Angabe von Zugriffsrechten als dreistellige Oktalzahl:**

![[Pasted image 20260413153331.png]]

**Befehle:**
```shell
chmod // Zugriffsrechte der Datei ändern 
chown // Besitzer der Datei ändern
chgrp // Gruppe der Datei ändern
umask // Maske mit Zugriffsrechten setzen
```

## Hardlinks und Softlinks

### Hardlink

	Hardlinks sind weitere Verzeichniseinträge, die auf den selben I-Node zeigen.

- jede Datei hat I-Node mit Metadaten
- Verzeichniseinträge verweisen nur auf I-Nodes
- Erstellen:
```shell
ln quelldatei linkname
ls -li // I-Node und Linkzähler anzeigen
```
- Link-Zähler der I-Node wird erhöht -> Datei existiert solange Zähler größer gleich 0
- Löschen einer Datei verringert nur den Linkzähler (**rm**)
- nur innerhalb des gleichen Dateisystems möglich (Stichwort *mount*)

### Softlink

	Softlinks (Symlinks) sind Dateien, die einen Pfad als Inhalt enthalten.

![[Pasted image 20260413172419.png]]

- Erstellen:
```shell
ln -s Ziel Linkname
ls -l // Anzeigen
```

- Berechtigungen des Links selbst sind irrelevant (es gelten die des Ziels)
- zeigt nur auf abs. oder rel. Pfad, NICHT auf I-Nodes
- kann auf Verzeichnisse zeigen (auch auf Netzlaufwerke)
- *Dangling Link:* Ziel wurde gelöscht/verschoben -> zeigt ins Leere
  

