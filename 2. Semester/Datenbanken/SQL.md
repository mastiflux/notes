
[StyleGuide](https://www.sqlstyle.guide/de/)

## Anlegen einer Datenbank

*Spezifikation:*
```sql
CREATE {DATABASE | SCHEMA} [IF NOT EXISTS] db_name

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
USE db_name
```

*Beispiel:*
```sql
USE test_datenbank123;
```

---
## Löschen einer Datenbank

*Spezifikation*
```sql
DROP db_name
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
)
```

*Beispiel:*
```sql
CREATE TABLE test_tabelle (vorname VARCHAR(35), nachname VARCHAR(35));
```

**Constraints**

- ==NULL== -> zeigt Fehlen von Informationen an, zu Variable die NULL zurückgibt soll keine Aussage getroffen werden (kann in allen Datentypen vorkommen)
- ==NOT NULL== -> Angabe eines Variablenwertes kann erzwungen werden
- 