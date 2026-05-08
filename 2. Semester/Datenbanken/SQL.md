# MYSQL
[StyleGuide](https://www.sqlstyle.guide/de/)

---
Starten: ```
```bash
sudo systemctl status mariadb
sudo systemctl start mariadb
sudo mysql
```
---
## Anlegen einer Datenbank

*Spezifikation:*
```sql
CREATE {DATABASE | SCHEMA} [IF NOT EXISTS] db_name;

create_specification
[DEFAULT] CHARACTER SET [=] charset_name
```

*Beispiel:*
```sql
CREATE DATABASE IF NOT EXISTS test_datenbank123 DEFAULT COLLATE latin1_german1_ci;
```

-> wirft Fehler wenn Datenbank bereits existiert + Programm wird beendet (optional)
-> COLLATE: Sortierreihenfolge festlegen (optional)

```sql
SHOW WARNINGS;
```

-> Fehler anzeigen lassen

---
## Auswahl einer Datenbank

*Spezifikation*
```sql
USE db_name;
```

*Beispiel:*
```sql
USE test_datenbank123;
```

---
## Löschen einer Datenbank

*Spezifikation*
```sql
DROP db_name;
```

*Beispiel:*
```sql
DROP test_datenbank123;
```

---
## Tabellen anlegen

*Spezifikation*
```sql
CREATE TABLE [IF NOT EXISTS] tbl_name
(
col_name2 DATATYPE [constraints], col_name2 DATATYPE [constraints], ...
);
```

*Beispiel:*
```sql
CREATE TABLE test_tabelle (
vorname VARCHAR(35), 
nachname VARCHAR(35),
PRIMARY KEY(vorname, nachname)
);
```

---
**Constraints**

- ==NULL== -> zeigt Fehlen von Informationen an, zu Variable die NULL zurückgibt soll keine Aussage getroffen werden (kann in allen Datentypen vorkommen)
- ==NOT NULL== -> Angabe eines Variablenwertes kann erzwungen werden
- ==PRIMARY KEY== -> identifiziert die Spalte in einer Tabelle eindeutig, muss einzigartig sein
- ==UNIQUE== -> jeder Wert muss einzigartig sein
- ==AUTO_INCREMENT== -> bei ganzzahligem Feld automatische Zuweisung eines Variablenwerts, kann nur mit einem PRIMARY KEY verwendet werden (kann mit 0, NULL oder DEFAULT verwendet werden)
- ==COMMENT 'string'== -> Kommentar hinzufügen
- ==FOREIGN KEY== -> Spalte einer Tabelle als Fremdschlüssel definieren, benötigt ==REFERENCE== auf die der Schlüssel zeigt
- ==CHECK== -> kann Prüfungen eines Dateneintrages vornehmen (in MySQL ohne Funktion)

---
**Anlage eines Index**

- Spalten, in denen häufig gesucht wird mit einem Index versehen
- Beispiel: ==BTREE== -> Baumstruktur siehe AuD

---
**Ableiten einer Tabelle**

- mit *select_statement* kann einer Tabelle auf Basis einer ==SELECT== Anweisung erzeugt werden
- weitere Tabellendefinitionen dann nicht notwendig
- zur Kopie von Tabellen

*Spezifikation*
```sql
CREATE [TEMPORARY] TABLE [IF NOT EXISTS] tbl_name [select_statement];
```

*Beispiel:*
```sql
SELECT vorname, nachname FROM personen WHERE nachname='Skywalker';
```

---
**Anlegen temporärer Tabellen**

- mit ==TEMPORARY== beim erstellen
- nach beenden der Session weg

*Beispiel:*
```sql
CREATE TEMPORARY TABLE tempoTabelle;
```

---
## Ändern einer Tabelle

*Allgemeine Spezifikation*
```sql
ALTER TABLE tbl_name;
```

-> immer in Kombination mit anderen Befehlen

---
*Umbenennen*
```sql
ALTER TABLE tbl_name RENAME TO new_tbl_name;
```

- FOREIGN KEY wird mit geändert

*Spalte hinzufügen*
```sql
ALTER TABLE tbl_name ADD COLUMN new_clmn examplename DATATYPE [NOT NULL] [DEFAULT wert];
```

- bei NOT NULL ggf. mit DEFAULT Fehler abfangen

*Spalte ändern*
```sql
ALTER TABLE tbl_name MODIFY [COLUMN] clmn_name specification;
```

```sql
ALTER TABLE personen MODIFY name CHAR(35);
```

- Vorsicht vor Datenverlust!

---
## Tabellen löschen

*Spezifikation*
```sql
DROP TABLE tbl_name [,...] [RESTRICT | CASCADE];
```

- ==CASCADE== löscht auch zusammenhängende Tabellen
- ==RESTRICT== macht Gegenteil von CASCADE und löscht nur die angegene Tabelle (muss nicht mit angegeben werden, wird normalerweise immer gemacht)

---
## Anzeigen von Tabellen

*Spezifikation*
```sql
SHOW [FULL] TABLES FROM db_name;
EXPLAIN tbl_name [\G];
```

---
# Datentypen

- 37 unterschiedliche Datentypen
---
## Ganzzahlige Datentypen

**Beispiel: TINYINT**
- 1 Byte Speicherkapazität
- M gibt Anzahl der Zeichen an, die zur Zahlendarstellung in der Tabelle verwendet werden
- UNSIGNED gibt an, dass kein Bit für die Speicherung des Vorzeichens verwendet werden soll

```sql
TINYINT [(M)] [UNSIGNED]
```

![[Pasted image 20260508104249.png]]

---
## Reelle Zahlen

**Beispiel DECIMAL**
- speichert exakte Zahlenwerte mit Nachkommastellen als Zeichenkette
- M definiert Gesamtzahl der verfügbaren Dezimalziffern (max 65, default 10)
- D definiert Anzahl der Nachkommastellen

```sql
DECIMAL [(M,D)] [UNSIGNED]
```

*Beispiel: DECIMAL (5,2)*
![[Pasted image 20260508104357.png]]

![[Pasted image 20260508104431.png]]

---
## Text 

**Beispiel: CHAR**
- speichert Zeichenketten
- M gibt Anzahl der Bytes an, die gespeichert werden sollen (0-255)
- leere Byte werden ausgefüllt
- BINARY legt fest, dass Zeichen auf Basis ihrer Binärwerte verglichen und sortiert werden
- CHARACER SET definiert verfügbaren Zeichensatz
- COLLATE legt die Sortierung des Zeichensatzes fest

```sql
CHAR [(M)] [BINARY] [CHARCTER SET charset_name] [COLLATE collation_name]
```


**Beispiel VARCHAR**
- wie CHAR nur mit variabler Länge
- leere Byte werden nicht aufgefüllt

**Beispiel ENUM**
- ermöglicht Angabe einer Liste vordefinierter Strings

![[Pasted image 20260508104507.png]]
![[Pasted image 20260508104534.png]]

---
## Zeitangaben

**DATE, TIME, DATETIME**
- Schreibweise DATE: 'yyyy-mm-dd'
- Plausibilität wird geprüft
- wenn dd = 00 -> ganzer Monat wird gespeichert
- wenn dd, m = 00 -> ganzes Jahr wird gespeichert

![[Pasted image 20260508104612.png]]

---
# Daten verwalten
## INSERT

- Daten manuell in Tabelle erfassen
- auf Anzahl Spalten achten!
- IGNORE führt dazu, dass Fehler bei INSERT Operationen ignoriert werden -> Warnung wird rausgegeben und Operation wird übersprungen, Skript läuft weiter

```sql
INSERT [IGNORE]
INTO tbl_name[(col_name,...)] 
VALUES (newValues|DEFAULT);
```


