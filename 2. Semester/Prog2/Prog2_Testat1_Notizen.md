# Lernübersicht: Programmieren 2

---

## 1. Nützliches aus der Java-API

### Wrapper-Klassen

**Konzept:** Primitive Datentypen (wie `int`, `double`) werden in Objekten gekapselt.

**Klassen:** `Byte`, `Short`, `Integer`, `Long`, `Double`, `Float`, `Boolean`, `Character`

**Zweck:**
- Bereitstellung von Hilfsmethoden (z.B. Formatierung/Parsen)
- Nutzung in generischen Datenstrukturen (wie Listen), da diese nur Referenzen akzeptieren

**Auto(Un-)Boxing:** Java wandelt primitive Typen und Wrapper-Objekte automatisch ineinander um.

```java
int i = 5;
Integer boxed = i;       // Autoboxing
int unboxed = boxed;     // Unboxing
```

**Unveränderbarkeit (Immutable):** Einmal erzeugt, kann der Wert eines Wrapper-Objekts nicht mehr geändert werden.

**Erzeugung** (von bevorzugt zu veraltet):
1. Automatisches Boxing
2. Statische `valueOf()`-Methoden ✅ ← bevorzugt
3. Konstruktor (veraltet, `new Integer(5)`) ❌

> ⚠️ **Achtung:** Kleine Integer-Werte (typischerweise −128 bis 127) werden per `valueOf()` in einem Pool vorgehalten, d.h. `Integer.valueOf(5) == Integer.valueOf(5)` ist `true`. Außerhalb dieses Bereichs kann das `false` sein!

---

### Zeichenketten (Strings)

**Eigenschaften:** Strings sind Objekte und ebenfalls unveränderbar (immutable).

**String-Pool:** String-Literale im Quellcode werden vom Compiler in einer Tabelle (String-Pool) verwaltet, weshalb identische Literale oft auf dasselbe Objekt zeigen.

**Vergleich:**
- Referenztypen (Objekte) → immer mit `.equals()` auf inhaltliche Gleichheit prüfen
- Primitive Typen → mit `==`

> ⚠️ `"abc" == "abc"` kann zufällig `true` sein (String-Pool), aber `new String("abc") == new String("abc")` ist `false`. Immer `.equals()` verwenden!

**Wichtige Methoden:**

| Methode | Beschreibung |
|---|---|
| `charAt(int i)` | Zeichen an Position i |
| `substring(int from, int to)` | Teilstring |
| `trim()` | Whitespace am Anfang/Ende entfernen |
| `startsWith(String s)` | Prüft Präfix |
| `equalsIgnoreCase(String s)` | Vergleich ohne Groß-/Kleinschreibung |
| `toLowerCase()` | Umwandlung in Kleinbuchstaben |
| `replace(char old, char new)` | Zeichen ersetzen |
| `split(String regex)` | String aufteilen |

**StringBuilder:** Bei vielen String-Manipulationen sollte aus Effizienzgründen ein `StringBuilder` verwendet werden, da jede direkte String-Konkatenation ein neues Objekt erzeugt.

```java
StringBuilder sb = new StringBuilder();
sb.append("Hallo");
sb.append(" Welt");
String result = sb.toString();
```

---

### JAR-Archive

**Definition:** Ein Java ARchive (JAR) ist ein ZIP-Archiv für `.class`-Dateien und Ressourcen, das Meta-Informationen in einer `MANIFEST.MF`-Datei enthält.

**Konsolen-Befehle:**

| Befehl | Funktion |
|---|---|
| `java -jar <name>.jar <args>` | JAR ausführen |
| `jar -tvf <name>.jar` | Inhalt auflisten |
| `jar -xvf <name>.jar` | Inhalt entpacken |

---

### I/O Grundlagen (Streams)

**Definition:** Ein Stream befördert Daten sequentiell nach dem FIFO-Prinzip von einer Quelle zu einem Ziel.

**Richtung:**
- `InputStream` → liest aus einer Quelle **ins** Programm
- `OutputStream` → schreibt aus dem Programm **in** eine Senke

**Datentypen:**

| Typ | Verwendung | Klassen |
|---|---|---|
| Byte-Streams | Binärdaten (Bilder, `.class`-Dateien) | `InputStream`, `OutputStream` |
| Character-Streams | Textdaten | `Reader`, `Writer` |

**Pufferung:** `BufferedReader` und `BufferedWriter` reduzieren die Anzahl der tatsächlichen I/O-Aufrufe und erhöhen die Effizienz erheblich.

```java
BufferedReader br = new BufferedReader(new FileReader("datei.txt"));
String zeile = br.readLine();   // liest eine Zeile

BufferedWriter bw = new BufferedWriter(new FileWriter("datei.txt"));
bw.write("Text");
bw.newLine();
bw.flush();
```

**Ressourcen:** Streams müssen immer mit `.close()` geschlossen werden – am besten mit einem *Try-with-Resources*-Block, der das automatisch erledigt:

```java
try (BufferedReader br = new BufferedReader(new FileReader("datei.txt"))) {
    String zeile;
    while ((zeile = br.readLine()) != null) {
        System.out.println(zeile);
    }
} // br.close() wird automatisch aufgerufen
```

---

## 2. Enumerations (Enums)

**Definition:** Ein Enumerationstyp beschränkt eine Variable auf eine vorab explizit definierte Menge von Werten (z.B. Himmelsrichtungen, Farben, Wochentage).

**Verwendung:** Werte werden mit dem Typnamen qualifiziert. Der Compiler sichert die Typsicherheit ab.

```java
Color c = Color.RED;
```

**`switch`-Anweisung:** Enums können als `case`-Marken verwendet werden – dort **ohne** qualifizierenden Typnamen:

```java
switch (c) {
    case RED:   System.out.println("Rot");  break;
    case GREEN: System.out.println("Grün"); break;
}
```

**Klasse Enum:** Alle Enums sind intern Unterklassen der speziellen abstrakten Klasse `Enum`.

**Wichtige Methoden:**

| Methode | Beschreibung |
|---|---|
| `.ordinal()` | Positionsnummer beginnend bei 0 |
| `.toString()` | Name als String |
| `.valueOf(String)` | String → Enum-Wert |
| `.values()` | Array aller Werte |
| `.compareTo()` | Vergleich anhand der Definitionsposition |

**Erweiterbarkeit:** Enums sind vollwertige Klassen. Sie können eigene Konstruktoren, Attribute und Methoden besitzen:

```java
public enum Planet {
    EARTH(5.972e24, 6.371e6),
    MARS (6.390e23, 3.390e6);

    private final double mass;
    private final double radius;

    Planet(double mass, double radius) {
        this.mass   = mass;
        this.radius = radius;
    }
}
```

---

## 3. UML-Modellierung

### Klassendiagramme

**Aufbau:** Ein Rechteck, unterteilt in drei Abschnitte: Klassenname | Attribute | Methoden

**Sichtbarkeits-Modifikatoren:**

| Symbol | Bedeutung |
|---|---|
| `+` | `public` |
| `-` | `private` |
| `#` | `protected` |
| `~` | package-private |

**Syntax:**
- Methoden: `Sichtbarkeit Name(Parameterliste): Rückgabetyp`
- Attribute: `Sichtbarkeit Name: Typ`

---

### Beziehungen zwischen Klassen

| Beziehung | Symbol | Bedeutung | Java-Realisierung |
|---|---|---|---|
| **Assoziation** | Durchgezogene Linie mit Pfeil | Allgemeinste Beziehung; Multiplizität (z.B. `1`, `0..*`) zeigt, wie viele Objekte in Beziehung stehen | Array oder Liste |
| **Aggregation** | Leere Raute ◇ | „Teil-Ganzes"-Relation; Teile können unabhängig vom Ganzen existieren | Referenz im Konstruktor/Setter |
| **Komposition** | Ausgefüllte Raute ◆ | Strenge „Teil-Ganzes"-Relation; Lebensdauer der Teile ist zwingend an das Ganze gekoppelt | Erzeugung im Konstruktor der äußeren Klasse |
| **Vererbung** | Linie mit leerer, geschlossener Pfeilspitze → Superklasse | „Ist-ein"-Beziehung (Generalisierung/Spezialisierung) | `extends` |
| **Realisierung** | Gestrichelte Linie mit leerer, geschlossener Pfeilspitze | Klasse implementiert ein Interface | `implements` |

> 💡 **Merkhilfe Aggregation vs. Komposition:** Ein Auto *hat* Räder (Aggregation – Räder können auch woanders existieren). Ein Haus *hat* Räume (Komposition – Räume ohne Haus ergeben keinen Sinn).

---

### Use-Case-Diagramme

**Zweck:** Zeigt das externe Systemverhalten aus Akteurssicht auf einer groben, abstrakten Ebene.

**Bestandteile:**
- **Akteur** → Strichmännchen
- **Use Case** → Oval
- **Systemgrenze** → umschließender Kasten

**Beziehungen:**
- `<<include>>` → Zwingender Aufruf eines anderen Use Cases (immer ausgeführt)
- `<<extend>>` → Optionale Erweiterung eines Use Cases (wird nur unter bestimmten Bedingungen ausgeführt)

---

## 4. Vererbung & Interfaces

### Konzepte der Vererbung

**Idee:** Vermeidung von Code-Duplikation. Eine Unterklasse (Spezialisierung) erbt Attribute und Methoden der Oberklasse (Abstraktion/Generalisierung) und ergänzt eigene.

**`super`-Aufruf:** Konstruktoren der Unterklasse rufen per `super()` den Konstruktor der Oberklasse auf.

> ⚠️ `super()` muss zwingend die **erste Anweisung** im Konstruktor sein!

```java
public class Hund extends Tier {
    private String rasse;
	
    public Hund(String name, String rasse) {
        super(name); // muss erste Zeile sein
        this.rasse = rasse;
    }
}
```

**Methoden überschreiben:** Eine Methode in der Unterklasse wird mit derselben Signatur neu implementiert. Die Annotation `@Override` sollte immer verwendet werden – der Compiler meldet dann einen Fehler, falls versehentlich eine neue Methode statt einer überschriebenen erstellt wird.

---

### Statische vs. Dynamische Typisierung

| | Statischer Typ | Dynamischer Typ |
|---|---|---|
| **Wann** | Zur Compile-Zeit (Deklaration) | Zur Laufzeit |
| **Bestimmt** | Welche Methoden/Attribute *aufrufbar* sind | Welche *Implementierung* einer überschriebenen Methode ausgeführt wird |
| **Prinzip** | — | Dynamische Bindung / Polymorphie |

```java
Tier t = new Hund("Bello", "Labrador");
// Statischer Typ: Tier  → nur Tier-Methoden aufrufbar
// Dynamischer Typ: Hund → Hund-Implementierung von lautGeben() wird aufgerufen
```

**Zuweisungen:** Einer Variablen der Oberklasse darf ein Objekt der Unterklasse zugewiesen werden.
- Typprüfung: `instanceof`
- Typumwandlung: Casting mit `(Typ)`

```java
if (t instanceof Hund h) {   // Pattern Matching (ab Java 16)
    h.bellen();
}
```

---

### Abstrakte Klassen vs. Interfaces

|  | Abstrakte Klasse | Interface |
|---|---|---|
| **Schlüsselwort** | `abstract class` | `interface` |
| **Instanziierbar?** | ❌ Nein | ❌ Nein |
| **Abstrakte Methoden** | ✅ Erlaubt | ✅ Alle Methoden implizit abstract (bis Java 8) |
| **Konkrete Methoden** | ✅ | ✅ `default`-Methoden ab Java 8 |
| **Attribute** | ✅ beliebig | Nur `public static final` |
| **Konstruktoren** | ✅ | ❌ |
| **Mehrfachvererbung** | ❌ Nur eine Superklasse (`extends`) | ✅ Mehrere implementierbar (`implements`) |

> 💡 **Wann welches?**
> - **Abstrakte Klasse** → wenn gemeinsamer Zustand (Attribute) oder Teilimplementierungen geteilt werden sollen.
> - **Interface** → wenn nur ein Verhalten/Vertrag definiert wird, das verschiedene, unverwandte Klassen erfüllen sollen.

**Ab Java 8** sind in Interfaces erlaubt:
- `default`-Methoden (mit Implementierung, als Fallback)
- `static`-Methoden

Eine Klasse kann mehrere Interfaces implementieren, aber nur von **exakt einer Klasse** erben. Interfaces sind daher die Lösung für fehlende Mehrfachvererbung in Java.

---

### Die Wurzelklasse `Object`

Alle Klassen in Java erben automatisch (direkt oder indirekt) von der Klasse `Object`.

**Wichtige Methoden:**

| Methode | Beschreibung |
|---|---|
| `equals(Object o)` | Inhaltlicher Vergleich (standardmäßig Referenzvergleich – sollte überschrieben werden!) |
| `hashCode()` | Hashwert; muss konsistent mit `equals()` sein |
| `toString()` | Textuelle Repräsentation (sollte überschrieben werden!) |
| `clone()` | Erzeugt eine flache Kopie des Objekts |

> ⚠️ **Vertrag:** Wenn `equals()` überschrieben wird, **muss** auch `hashCode()` überschrieben werden (und umgekehrt), sonst funktionieren `HashMap`, `HashSet` etc. nicht korrekt.

---

## 5. Prinzipien des Objektorientierten Designs (OOD)

### Kapselung (Encapsulation)
Trennung von interner Implementierung und äußerer Schnittstelle. Verbirgt, was sich ändern kann – Änderungen der Implementierung haben so keinen Einfluss auf den Aufrufer.

### Programmieren auf Schnittstellen
> *„Code to an interface rather than to an implementation."*

Variablen sollten den abstraktesten passenden Typ haben (z.B. `List<String>` statt `ArrayList<String>`), damit die konkrete Implementierung jederzeit austauschbar bleibt.

### Open-Closed Principle (OCP)
> *„Offen für Erweiterungen, geschlossen für Änderungen."*

Neue Funktionalität sollte durch Erweiterung (z.B. neue Klasse, neues Interface) hinzugefügt werden, nicht durch Änderung bestehenden Codes.

### Don't Repeat Yourself (DRY)
Kopierten Code vermeiden. Gemeinsamkeiten in Methoden oder Basisklassen auslagern. Jedes Stück Wissen sollte im System nur **eine** autoritative Repräsentation haben.

### Single Responsibility Principle (SRP)
Eine Klasse sollte nur **eine einzige Zuständigkeit** haben und es sollte nur **einen Grund** geben, die Klasse zu ändern.

> 💡 Wenn man die Aufgabe einer Klasse nur mit „und" beschreiben kann, hat sie wahrscheinlich zu viele Verantwortlichkeiten.

### Liskov Substitution Principle (LSP)
Ein Objekt einer Unterklasse muss an **allen Stellen** einsetzbar sein, an denen Objekte der Oberklasse erwartet werden, ohne das erwartete Verhalten zu zerstören.

> ⚠️ **Klassisches Gegenbeispiel:** Ein Quadrat ist geometrisch ein Rechteck, aber im OO-Sinn **kein** korrektes Rechteck: Wenn man Breite und Höhe unabhängig setzen kann (wie bei `Rechteck`), bricht das beim `Quadrat` das Verhalten. LSP verletzt → kein `Quadrat extends Rechteck`!
