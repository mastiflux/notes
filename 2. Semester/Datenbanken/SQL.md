
[StyleGuide](https://www.sqlstyle.guide/de/)

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

