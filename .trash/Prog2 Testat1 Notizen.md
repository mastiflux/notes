# Lernübersicht: Programmieren 2

## 1. Nützliches aus der Java-API

### Wrapper-Klassen
* [cite_start]**Konzept:** Primitiven Datentypen (wie `int`, `double`) werden in Objekten gekapselt[cite: 885, 1054].
* [cite_start]**Klassen:** `Byte`, `Short`, `Integer`, `Long`, `Double`, `Float`, `Boolean`, `Character`[cite: 889, 1055].
* [cite_start]**Zweck:** Bereitstellung von Hilfsmethoden (z.B. Formatierung/Parsen) und Nutzung in generischen Datenstrukturen (wie Listen), da diese nur Referenzen akzeptieren[cite: 893, 894, 1057].
* [cite_start]**(Un-)Boxing:** Java wandelt primitive Typen und Wrapper-Objekte automatisch ineinander um[cite: 920, 1084].
* [cite_start]**Unveränderbarkeit (Immutable):** Einmal erzeugt, kann der Wert eines Wrapper-Objekts nicht mehr geändert werden[cite: 941, 958].
* [cite_start]**Erzeugung:** Entweder durch automatiseches Boxing, per Konsturktor (veraltet) oder am besten über statische `valueOf()`-Methoden[cite: 910, 911, 912]. [cite_start](Achtung: Kleine Integer-Werte werden per `valueOf` in einem Pool vorgehalten [cite: 943]).

### Zeichenketten (Strings)
* [cite_start]**Eigenschaften:** Strings sind Objekte und ebenfalls unveränderbar (immutable)[cite: 964].
* [cite_start]**String-Pool:** String-Literale im Quellcode werden vom Compiler in einer Tabelle verwaltet, weshalb identische Strings oft auf dasselbe Objekt zeigen[cite: 965].
* [cite_start]**Vergleich:** Referenztypen (Objekte) immer mit `.equals()` auf inhaltliche Gleichheit prüfen, primitive Typen mit `==`[cite: 967].
* [cite_start]**Wichtige Methoden:** `charAt()`, `substring()`, `trim()`, `startsWith()`, `equalsIgnoreCase()`, `toLowerCase()`, `replace()`, `split()`[cite: 970, 971, 972, 974, 976, 978].
* [cite_start]**StringBuilder:** Bei vielen String-Manipulationen sollte aus Effizienzgründen ein `StringBuilder` (z.B. mit `.append()`) verwendet werden[cite: 985, 986].

### JAR-Archive
* [cite_start]**Definition:** Ein Java ARchive (JAR) ist ein ZIP-Archiv für Class-Dateien und Ressourcen, das Meta-Informationen (Manifest) enthält[cite: 989, 991].
* [cite_start]**Konsolen-Befehle:** * Ausführen: `java -jar <name>.jar <args>`[cite: 996, 1061].
  * [cite_start]Inhalt ansehen: `jar -tvf <name>.jar`[cite: 996, 1061].
  * [cite_start]Entpacken: `jar -xvf <name>.jar`[cite: 996, 1061].

### I/O Grundlagen (Streams)
* [cite_start]**Definition:** Ein Stream befördert Daten sequentiell (FIFO-Prinzip) von einer Quelle zu einem Ziel[cite: 998, 1064].
* [cite_start]**Richtung:** `InputStream` liest aus einer Quelle ins Programm, `OutputStream` schreibt aus dem Programm in eine Senke[cite: 1001, 1002].
* **Datentypen:**
  * **Byte-Streams:** Für Binärdaten (Bilder, .class). [cite_start]Klassen: `InputStream`, `OutputStream`[cite: 1008, 1009].
  * **Character-Streams:** Für Textdaten. [cite_start]Klassen: `Reader`, `Writer`[cite: 1010].
* [cite_start]**Pufferung:** `BufferedReader` (mit `readLine()`) und `BufferedWriter` (mit `newLine()`, `flush()`) reduzieren I/O-Aufrufe und erhöhen die Effizienz[cite: 1021, 1023, 1101, 1106].
* [cite_start]**Ressourcen:** Streams müssen immer mit `.close()` geschlossen werden, optimalerweise in einem *Try-with-Resources*-Block[cite: 1015, 1125].

---

## 2. Enumerations (Enums)

* [cite_start]**Definition:** Ein Enumerationstyp beschränkt eine Variable auf eine vorab explizit definierte Menge von Werten (z.B. Himmelsrichtungen oder Farben)[cite: 750, 754, 776].
* **Verwendung:** Werte werden mit dem Typnamen qualifiziert (z.B. `Color.RED`). [cite_start]Der Compiler sichert die Typsicherheit ab[cite: 797, 799].
* [cite_start]**switch-Anweisung:** Enums können als case-Marken verwendet werden (dort ohne qualifizierenden Typnamen)[cite: 837, 838].
* [cite_start]**Klasse Enum:** Alle Enums sind intern Unterklassen der speziellen Klasse `Enum`[cite: 819, 841]. 
* [cite_start]**Wichtige Methoden:** * `.ordinal()` (Positionsnummer beginnend bei 0)[cite: 821, 846].
  * [cite_start]`.toString()` (Name als String)[cite: 846].
  * [cite_start]`.valueOf(String)` (String zu Enum-Wert)[cite: 846].
  * [cite_start]`.values()` (Array aller Werte)[cite: 847].
  * [cite_start]`.compareTo()` (Vergleich anhand der Definitionsposition)[cite: 823].
* **Erweiterbarkeit:** Enums sind vollwertige Klassen. [cite_start]Sie können eigene Konstruktoren, Attribute und Methoden besitzen[cite: 851, 853].

---

## 3. UML Modellierung


### Klassendiagramme
* [cite_start]**Aufbau:** Ein Rechteck unterteilt in drei Abschnitte: Klassenname, Attribute, Methoden[cite: 611].
* [cite_start]**Sichtbarkeits-Modifikatoren:** `+` (public), `-` (private), `#` (protected), `~` (package-private)[cite: 638, 662].
* **Syntax:** `Sichtbarkeit Name(Parameterliste): Rückgabetyp` (bei Methoden) bzw. [cite_start]`Sichtbarkeit Name: Typ` (bei Attributen)[cite: 627, 664, 668].

### Beziehungen zwischen Klassen
* **Assoziation:** Allgemeinste Beziehung. Durchgezogene Linie mit Pfeil für Leserichtung. Multiplizität (z.B. `1`, `0..*`) gibt an, wie viele Objekte in Beziehung stehen. [cite_start]In Java als Array oder Liste realisiert[cite: 685, 686, 698, 699].
* **Aggregation:** "Teil-Ganzes"-Relation. Teile können unabhängig vom Ganzen existieren. [cite_start]Symbol: leere Raute[cite: 688, 689].
* **Komposition:** Strenge "Teil-Ganzes"-Relation. Lebensdauer der Teile ist zwingend an das Ganze gekoppelt. Wird in Java im Konstruktor der äußeren Klasse erzeugt. [cite_start]Symbol: ausgefüllte Raute[cite: 691, 692, 704].
* **Vererbung:** "Ist-ein"-Beziehung (Generalisierung/Spezialisierung). Subklasse erbt von Superklasse. [cite_start]Symbol: Linie mit leerer, geschlossener Pfeilspitze zur Superklasse[cite: 711, 713].
* **Realisierung (Interfaces):** Eine Klasse implementiert eine Schnittstelle. [cite_start]Symbol: gestrichelte Linie mit leerer, geschlossener Pfeilspitze[cite: 715, 716].

### Use Case Diagramme
* [cite_start]**Zweck:** Zeigt das externe Systemverhalten aus Akteurssicht auf einer groben Ebene[cite: 729].
* [cite_start]**Bestandteile:** Akteur (Strichmännchen), Use Case (Oval), Systemgrenze (Kasten)[cite: 730, 731].
* [cite_start]**Beziehungen:** * `<<include>>`: Zwingender Aufruf eines anderen Use Cases[cite: 731].
  * [cite_start]`<<extend>>`: Optionale Erweiterung eines Use Cases[cite: 731].

---

## 4. Vererbung & Interfaces

### Konzepte der Vererbung
* **Idee:** Vermeidung von Code-Duplikation. [cite_start]Eine Unterklasse (Spezialisierung) erbt Attribute und Methoden der Oberklasse (Abstraktion/Generalisierung) und ergänzt eigene[cite: 190, 247, 290].
* **`super`-Aufruf:** Konstruktoren der Unterklasse rufen per `super()` den Konstruktor der Oberklasse auf. [cite_start]Dies muss zwingend die erste Anweisung im Konstruktor sein[cite: 280, 281].
* **Methoden überschreiben:** Eine Methode in der Unterklasse wird mit derselben Signatur neu implementiert. [cite_start]Empfohlen: Die Annotation `@Override` verwenden[cite: 276, 363].

### Statische vs. Dynamische Typisierung
* **Statischer Typ:** Bei Deklaration angegeben. [cite_start]Ist dem Compiler bekannt und bestimmt, *welche* Methoden und Attribute aufrufbar sind[cite: 306, 307, 317].
* **Dynamischer Typ:** Der tatsächliche Typ des Objekts zur Laufzeit. [cite_start]Bestimmt, *welche Implementierung* einer überschriebenen Methode ausgeführt wird (Dynamische Bindung / Polymorphie)[cite: 309, 314, 318].
* **Zuweisungen:** Einer Variablen der Oberklasse darf ein Objekt der Unterklasse zugewiesen werden. [cite_start]Typprüfung erfolgt via `instanceof`, Typumwandlung durch Casting `(Typ)`[cite: 294, 297].

### Abstrakte Klassen vs. Interfaces
* **Abstrakte Klasse (`abstract`):**
  * [cite_start]Darf abstrakte Methoden (ohne Rumpf) enthalten[cite: 370].
  * [cite_start]Von ihr können keine Objekte (Instanzen) erzeugt werden[cite: 376].
  * [cite_start]Unterklassen müssen abstrakte Methoden implementieren oder selbst abstrakt sein[cite: 373].
  * [cite_start]Kann Attribute und Konstruktoren enthalten[cite: 531, 532].
* **Interfaces (`interface`):**
  * Reine Schnittstellen. [cite_start]Deklarieren, *was* Klassen leisten können müssen[cite: 386].
  * [cite_start]Bis Java 8: Alle Methoden sind implizit `public` und `abstract`, Attribute sind `public static final`[cite: 393, 394, 398, 399].
  * [cite_start]Ab Java 8: `default`-Methoden (mit Code) und `static`-Methoden sind erlaubt[cite: 395, 396].
  * Eine Klasse kann mehrere Interfaces implementieren (Schlüsselwort `implements`), aber nur von exakt einer Klasse erben (`extends`). [cite_start]Daher sind Interfaces die Lösung für fehlende Mehrfachvererbung in Java[cite: 388, 397, 511].

### Die Wurzelklasse `Object`
* [cite_start]Alle Klassen in Java erben automatisch (direkt oder indirekt) von der Klasse `Object`[cite: 536].
* [cite_start]**Wichtige Methoden:** `equals(Object)` (inhaltlicher Vergleich), `clone()`, `hashCode()`, `toString()` (textuelle Repräsentation)[cite: 538, 540, 541, 542].

---

## 5. Prinzipien des Objektorientierten Designs (OOD)


* **Kapselung (Encapsulation):** Trennung von interner Implementierung und äußerer Schnittstelle. [cite_start]Verbirgt, was sich ändern kann[cite: 23, 37].
* [cite_start]**Programmieren auf Schnittstellen:** "Code to an interface rather than to an implementation"[cite: 38].
* [cite_start]**Open-Closed Principle (OCP):** Klassen sollten offen für Erweiterungen, aber geschlossen für Änderungen sein (z.B. Erweiterung über Interfaces lösen statt bestehende Methoden abzuändern)[cite: 48, 53].
* [cite_start]**Don't repeat yourself (DRY):** Kopierten Code vermeiden, Gemeinsamkeiten in Methoden oder Basisklassen auslagern[cite: 72].
* [cite_start]**Single Responsibility Principle (SRP):** Eine Klasse sollte nur eine einzige Zuständigkeit haben und es sollte nur einen Grund geben, die Klasse zu ändern[cite: 82, 83].
* [cite_start]**Liskov Substitution Principle (LSP):** Ein Objekt einer Unterklasse muss an allen Stellen einsetzbar sein, an denen Objekte der Oberklasse erwartet werden, ohne das erwartete Verhalten zu zerstören (Beispiel: Ein Quadrat ist im OO-Sinn kein echtes Rechteck, da sich Breite und Höhe nicht unabhängig setzen lassen)[cite: 96, 99, 106].