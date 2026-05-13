# Lernzettel: Programmieren 2 Testat 2
## Exceptions, Generics, Modularisierung

---

## 1. Ausnahmebehandlung (Exceptions)

Fehler zur Laufzeit lassen sich oft nicht vermeiden (z.B. falsche Benutzereingaben, fehlende Dateien). Statt das Programm abstürzen zu lassen oder umständliche Fehlercodes (wie `-1`) zurückzugeben, nutzt Java Exceptions, um Fehlerbehandlung und Programmlogik sauber zu trennen.

### Geprüfte vs. Ungeprüfte Ausnahmen

| Eigenschaft | Geprüfte Ausnahmen (Checked) | Ungeprüfte Ausnahmen (Unchecked) |
|---|---|---|
| Basisklasse | Unterklassen von `Exception` (außer `RuntimeException`) | Unterklassen von `RuntimeException` |
| Compiler-Prüfung | Ja. Müssen mit `try-catch` behandelt oder mit `throws` weitergeleitet werden. | Nein. Der Compiler zwingt nicht zur Behandlung. |
| Ursache | Unwahrscheinliche, aber mögliche externe Ereignisse (z.B. `FileNotFoundException`) | Programmierfehler im Code (z.B. `NullPointerException`, Division durch 0) |

### Konzept & Kontrollfluss

- **`throw`** – Eine Ausnahme wird mit `throw new Exception();` geworfen. Die Methode wird sofort unterbrochen.
- **`try`** – Code, der Ausnahmen werfen kann, kommt in den `try`-Block.
- **`catch`** – Passt der Typ der geworfenen Exception zum Parameter des `catch`-Blocks, wird dieser ausgeführt.
- **`finally`** – Wird immer ausgeführt, egal ob eine Exception auftrat oder nicht. Wichtig zum Schließen von Ressourcen.
- **`try-with-resources`** – Moderne Alternative zu `finally` für Klassen, die `AutoCloseable` implementieren. Die Ressource wird automatisch geschlossen.

> **Tipp:** `try-with-resources` ist in der Praxis gegenüber manuellem `finally`-Closing vorzuziehen, da es auch bei Exceptions im Konstruktor korrekt aufräumt und weniger fehleranfällig ist.

### Klausur-Fallen

> **Reihenfolge der `catch`-Blöcke:** Spezifische Exceptions müssen vor allgemeinen stehen (z.B. `FileNotFoundException` vor `IOException`). Andernfalls gibt es einen Compilerfehler, da der spezifische Block nie erreichbar wäre.

> **Exceptions bei Vererbung (Kovarianz):** Überschreibt eine Unterklasse eine Methode der Oberklasse, darf die `throws`-Klausel keine neuen oder allgemeineren checked Exceptions hinzufügen. Weniger oder spezifischere Exceptions sind erlaubt.

> **Tipp:** Man kann mehrere Exception-Typen in einem einzigen `catch`-Block behandeln: `catch (IOException | SQLException e)`. Das vermeidet doppelten Code, wenn die Behandlung identisch ist.

---

## 2. Generics (Generische Typen)

Generics erlauben es, Klassen, Interfaces und Methoden als "Schablonen" zu schreiben, die erst bei der Verwendung mit einem konkreten Referenztyp versehen werden.

### Kernkonzepte

- **Zweck:** Der Compiler kann strenge Typprüfungen durchführen, und man spart sich fehleranfällige manuelle Typkonvertierungen (Casts).
- **Einschränkung:** Typparameter (`<T>`) funktionieren nur mit Referenztypen. Primitive Typen (wie `int`) sind verboten; man muss Wrapper-Klassen (`Integer`) nutzen.
- **Type Erasure (Typlöschung):** Generics existieren nur für den Compiler. Zur Laufzeit ersetzt die JVM den Typparameter `<T>` durch `Object` (oder den angegebenen Obertyp).

### Wildcards und Bounds

| Syntax | Bedeutung | Typischer Einsatz |
|---|---|---|
| `<T extends Number>` | Upper Bound: T muss Unterklasse von `Number` sein | Lesen und Rechnen mit Number-Methoden |
| `<T extends MyClass & Comparable<T>>` | Multiple Bounds | T muss mehrere Typen erfüllen |
| `<?>` | Unbekannter Typ, kompatibel mit jeder generischen Ausprägung | Nur lesen, nicht schreiben |
| `<? extends Typ>` | Unterklassen erlaubt (Lese-Zugriff) | Producer: Daten entnehmen |
| `<? super Typ>` | Oberklassen erlaubt (Schreib-Zugriff) | Consumer: Daten hinzufügen |

> **Merkhilfe PECS:** *Producer Extends, Consumer Super.* Wenn eine Struktur Daten liefert (produziert), nutze `extends`. Wenn sie Daten aufnimmt (konsumiert), nutze `super`.

### Klausur-Fallen

> **Invarianz:** `GenericList<String>` ist **keine** Unterklasse von `GenericList<Object>`! Folgende Zuweisung führt zu einem Compilerfehler:
> ```java
> GenericList<Object> list = new GenericList<String>(); // FEHLER
> ```

> **Generische Arrays:** Die Instanziierung `new T[10]` ist verboten, da es die Typsicherheit gefährdet. Der Workaround ist ein Cast:
> ```java
> (T[]) new Object[10];
> ```

> **`instanceof` mit Generics:** Da Typen zur Laufzeit gelöscht werden, ist `if (obj instanceof GenericList<String>)` verboten. Nur der Rohtyp funktioniert:
> ```java
> if (obj instanceof GenericList) // korrekt
> ```

---

## 3. Modularisierung (Pakete & Innere Klassen)

Modularisierung hilft dabei, Ordnung in Programme zu bringen, Zugriffsrechte zu kontrollieren und Namenskonflikte zu vermeiden.

### Pakete (Packages)

- Pakete fungieren als Sichtbarkeitsgrenzen. Was ohne `public` in einem Paket deklariert ist, ist in anderen Paketen unsichtbar (Paketsichtbarkeit).
- Klassennamen müssen nur innerhalb des eigenen Pakets eindeutig sein (Kollisionsfreiheit).
- Namen können mit `public` für andere Pakete exportiert (sichtbar gemacht) werden.
- Um eine Klasse aus einem anderen Paket zu nutzen, muss sie importiert werden oder mit dem vollen Paketnamen qualifiziert aufgerufen werden.

### Information Hiding (Geheimnisprinzip)

- **Konzept:** Die Implementierung komplexer Datenstrukturen wird vor dem Benutzer versteckt. Der Zugriff erfolgt ausschließlich über Methoden.
- **Warum?** Es verringert die Komplexität und erlaubt es, die interne Implementierung später zu ändern, ohne dass der Benutzer seinen Code anpassen muss.

### Sichtbarkeits-Hierarchie

| Modifier          | Eigene Klasse | Eigenes Paket | Unterklassen | Überall |
| ----------------- | :-----------: | :-----------: | :----------: | :-----: |
| `private`         |      Ja       |     Nein      |     Nein     |  Nein   |
| *(kein Modifier)* |      Ja       |      Ja       |     Nein     |  Nein   |
| `protected`       |      Ja       |      Ja       |      Ja      |  Nein   |
| `public`          |      Ja       |      Ja       |      Ja      |   Ja    |

### Innere Klassen

| Art | Besonderheit | Objekterzeugung |
|---|---|---|
| Statische innere Klasse | `static`; Zugriff auf Klassenvariablen der äußeren Klasse | `Outer.Inner obj = new Outer.Inner();` |
| Innere Instanzklasse | Kein `static`; Zugriff auf alle (auch Instanz-)Variablen der äußeren Klasse | `Outer.Inner obj = outerObj.new Inner();` |
| Lokale innere Klasse | Innerhalb einer Methode definiert; Zugriff auf effektiv finale Parameter | Normal innerhalb der Methode |
| Anonyme innere Klasse | Ohne Namen, direkt bei `new` definiert; typisch für Interfaces (Listener, Iteratoren) | `new InterfaceName() { ... };` |

> **Tipp:** Anonyme innere Klassen wurden in modernem Java weitgehend durch **Lambda-Ausdrücke** ersetzt, die kürzer und lesbarer sind. Sie funktionieren aber nur für funktionale Interfaces (genau eine abstrakte Methode).

### Klausur-Fallen

> **Instanziierung von Instanzklassen:** Eine Instanzklasse kann nicht ohne ein Objekt der äußeren Klasse instanziiert werden. Das korrekte Format lautet:
> ```java
> InstanzDerÄußerenKlasse.new InnereKlasse()
> ```

> **Tipp:** `protected` wird oft falsch eingeschätzt – es gewährt Zugriff für Unterklassen auch paketübergreifend, also nicht nur im eigenen Paket.
