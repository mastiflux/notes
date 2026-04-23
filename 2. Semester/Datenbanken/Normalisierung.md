**Motivation:**

- bisherige Modellierungsregeln vermeiden Redundanzen nicht -> Probleme entstehen

_Beispiel CDs:_

| ID   | Interpret       | Gründungsjahr | CD-Titel      | Lieder                                                        | Label      |
| ---- | --------------- | ------------- | ------------- | ------------------------------------------------------------- | ---------- |
| 1001 | Lady Gaga       | 2005          | Chromatica    | 1. Chromatica, 2. Alice, 3. Stupid Love, 4. Rain on me        | Interscope |
| 2002 | Lady Gaga       | 2005          | Born This Way | 1. Marry the night, 2. Born this way, 3. Hair                 | Interscope |
| 3003 | Micheal Jackson | 1964          | Dangerous     | 1. Jam, 2. WYWTOM, 3. In the closet, 4. She drives me wild    | Epic       |
| 4004 | Foo Fighters    | 1994          | Greatest Hits | 1. All my life, 2. Best of you, 3. Everlong, 4. The pretender | Sony       |
-> es kann nicht nach Liedern gesucht werden -> mehrere daten in einer Spalte
-> aufwendig, Daten zu verändern

## Die erste Normalform

- für jedes Mehrfachattribut wird eine neue Zeile erzeugt
- zusammengesetzter Primärschlüssel
-> für jedes Lied eine einzelne Zeile

**Eine Relation ist in der ersten Normalform, wenn sie nur aus atomaren Attributen besteht.** 

Nachteile: 
- Erzeugung von vielen Redundanzen
- Speicheraufwand sehr groß

## Die zweite Normalform

**Eine Relation ist in der zweiten Normalform, wenn die sich in der ersten Normalform befindet und falls jedes nicht-SChlüsselattribut der Relation voll funktional abhängig von jedem Schlüsselkandidaten der Relation ist.

- Zerlegung der Tabellen, bis Definition zutrifft

## Die dritte Normalform

**Eine Relation ist in der dritten Normalform, wenn sie in der zweiten Normalform ist und kein nicht-Schlüsselelement funktional abhängig von einem anderen nicht-Schlüsselelement ist.**

- Zerlegung der Tabellen in bspw. CDs und Interpreten
