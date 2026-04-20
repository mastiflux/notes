## Pakete

= Sammlung zusammengehöriger Klassen (Bibliothek), Kompilationseinheit
- Zweck: 
	- mehr Ordnung in Programme bringen 
	- bessere Kontrolle der Zugriffsrechte (wer darf auf was zugreifen)
	- Vermeidung von Namenskonflikten

*Beispiele:*

![[Pasted image 20260420120428.png]]

--- 

**Anlegen**

```java
package graphics; 

public class Circle {/* ... */ }

package graphics; 

public class Rectangle {/* ... */ }
```

![[Pasted image 20260420120609.png]]

-> Wenn die package-Zeile fehlt, gehören die Klassen zu einem namenlosen Standardpaket (Default-Paket)!

- Eindeutigkeit der Paketnamen garantiert Kollisionsfreiheit
- Hierarchie wird auf Unterverzeichnisse abgebildet

---

**Sichtbarkeitsgrenzen**

- was in einem Paket deklariert ist, ist in anderen Paketen unsichtbar

```java
package one; 
class C { /* ... */ } 
class D { /* ... */ }
```

```java
package two; 
class D { // gleicher Name stört nicht (verschiedene Klassen) 
	C obj; // Compilerfehler, C ist hier unsichtbar 
}
```

---

**Export von Namen**

- Namen können mit dem Zusatz public exportiert werden, sie sind dann in anderen Paketen sichtbar)
- Namen ohne Zusatz sind nur in diesem Paket sichtbar (default, Paketsichtbarkeit)
- public-Attribute und -Methoden werden nur exportiert, wenn die Klasse public ist
- lokale Variablen und Parameter können nicht exportiert werden

```java
package one; 
public class C { // public 
int x; // paketsichtbar 
public int y; // public 
void p() {/*... */} // paketsichtbar 
public void q() { /*... */ } // public 
C() { /*... */ } // paketsichtbar 
public C (int x, int y) {/* ... */ } // public 
}
```

--- 

**Import von Typnamen**

- exportierte Klassennamen können in anderen Paketen importiert werden

```java
package mypack; 

import graphics.Circle; 
import one.C; 

public class MyClass { 
	Circle c; 
	C obj; 
}
```

```java
package mypack; 

public class MyClass { 
	graphics.Circle c; 
	one.C obj; 
} 
```

-> Beim Import müssen alle importierten Klassen verschiedene Namen haben!

```java
package mypack; 

import graphics.Circle; 
import javafx.scene.shape.Circle; //Compilerfehler: Kollision 

public class MyClass { 
	Circle c; // welche Klasse ist gemeint? 
}
```

---

**Verzeichnisse**

- Pakete werden auf Verzeichnisse abgebildet, Klassen auf Dateien

![[Pasted image 20260420121656.png]]

---

**Geschachtelte Pakete**

*Import und Verwendung:*
```java
package graphics; 

import graphics.a.b.c.D; // import der Klasse D 
import graphics.a.*; // import aller public-Klassen aus graphics.a 
import graphics.*; // import aller public-Klassen aus graphics aber nicht aus den Subpaketen wie a, a.b, etc. 
public class A { 
	graphics.a.b.E myE; // Qualifikation einer Klasse 
}
```

---

**Geheimnisprinzip**

- Information Hiding
- Prinzip:
	- Verstecke die Implementierung komplexer Datenstrukturen vor Benutzern
	- Erlaube den Zugriff auf die Daten nur über Methoden
- Warum?
	- Erlaube den Zugriff auf die Daten nur über Methoden
	- Implementierung der Daten kann geändert werden, ohne dass Benutzer etwas merken
	- Schützt vor mutwilliger oder unabsichtlicher Zerstörung

*Sichtbarkeitsattribute:*

![[Pasted image 20260420122131.png]]


## Innere Klassen

- eine public Klasse C ist in einer eigenen Datei C.java definiert
- Ausnahmen:
	1. Eine Klasse, die nicht public ist, darf auch in einer anderen Datei deklariert werden, nicht empfehlenswert.
	2. Eine Klasse darf innerhalb einer anderen Klasse deklariert werden: Innere Klasse
	3. Eine innere Klasse muss keinen Klassennamen haben: Anonyme innere Klasse -> Klasse wird dort definiert, wo sie (einmalig) benötigt wird.

---

**Arten innerer Klassen**

1. Statische innere Klasse B 
	- Analog zu Klassenvariable in äußerer Klasse A definiert, Schlüsselwort static 
	- Objekterzeugung: A.B obj = new A.B(…); 
	- B hat Zugriff auf Klassenvariablen und Klassenmethoden von A
2. Innere Instanzklasse B
	- Analog zu Instanzvariablen in äußerer Klasse A definiert 
	- Objekterzeugung nur im Verbund mit Instanz der umgebenden Klasse A 
	- B hat Zugriff auf alle Variablen und Methoden von A
3. Lokale innere Klasse B
	- Analog zu lokaler Variablen in einer Methode, Konstruktor oder Block definiert 
	- Zusätzlich Zugriff auf effektiv finale Parameter des umgebenden Blocks / Methode
4. Anonyme innere Klasse 
	- Lokale Klasse, die ohne Klassenname in new-Anweisung definiert ist (Klassendefinition und Objekterzeugung sind in einer Anweisung zusammengefasst)  
	- Zugriff wie bei lokaler innerer Klasse

**Beispiele**:

*statische innere Klasse*
```java
public class Account { 
	private int userId; 
	private Permissions perm; 
	
	public Account(int userId) { 
		this.userId = userId; 
		perm = new Permissions(); 
		} 
		
	public int getUserId() { return userId; } 
	
	public static class Permissions { 
		public boolean canRead; 
		public boolean canWrite; 
	} 
	
	public Permissions getPermissions() { return perm; }
	
	public static void main(String[] args) { 
		Account account = new Account(4711); 
		Account.Permissions p = new Account.Permissions(); 
		p = account.getPermissions(); 
	} 
} 
```

*innere Instanzklasse*
```java
public class Konto {
    private int knummer;
    private double saldo;
    private Transaktion last;
	
    public Konto(int k, double s) {
        knummer = k;
        saldo = s;
    }
	
    public class Transaktion {
        private String name;
        private double betrag;
		
        public Transaktion(String n, double b) {
            name = n;
            betrag = b;
	    }
		
        public String toString() {
            return knummer + " " + name + " " + betrag + " " + saldo;
        }
	}
	
    public Transaktion getLast() {
        return last;
    }
	
    public void zahleEin(double betrag) {
        saldo += betrag;
        last = new Transaktion("Einzahlung", betrag);
    }
	
    public void zahleAus(double betrag) {
        saldo -= betrag;
        last = new Transaktion("Auszahlung", betrag);
    }
	
    public static void main(String[] args) {
        Konto k = new Konto(4711, 1000.0);
        k.zahleEin(500.0);
        k.zahleAus(700.0);
        
        Konto.Transaktion t1 = k.new Transaktion("Einzahlung", 100.0);
        Konto.Transaktion t2 = k.getLast();
        
        System.out.println(t1.toString());
        System.out.println(t2.toString());
    }
}
```

*lokale innere Klasse*
```java
public class Liste {
    private Element start, ende;
	
    private static class Element {
        private Object data;
        private Element next;
    }
	
    public void add(Object o) {
        if (o == null) return;
        Element neu = new Element();
        neu.data = o;
        if (start == null) {
            start = ende = neu;
        } else {
            ende.next = neu;
            ende = neu;
        }
    }
	
    public Iterator iterator() {
        class IteratorImpl implements Iterator {
            private Element e = start;
			
            public boolean hasNext() {
                return e != null;
            }
			
            public Object next() {
                if (e == null) return null;
                Object o = e.data;
                e = e.next;
                return o;
            }
        }
        return new IteratorImpl();
    }
}
```

*anonyme innere Klasse*
```java
public Iterator iterator2() {
    return new Iterator() {
        private Element e = start;
		
	    public boolean hasNext() {
            return e != null;
        }
		
        public Object next() {
            if (e == null) {
                return null;
            }
            Object o = e.data;
            e = e.next;
            return o;
        }
    };
}
```

## Module

- neue Abstraktionsebene
- Java Platform Module System (JPMS) oder Jigsaw
- Ziele: 
	- Kapselung, feinere Zugriffskontrolle auch bei öffentlichen Klassen 
	- Abstraktion, nur ein Teil der Klassen des Moduls wird auch als Schnittstelle angeboten  
	- Explizite Abhängigkeiten zwischen Modulen

---

**Sichtbarkeiten**

- Klassen, Pakete und Module lassen sich als Container mit unterschiedlichen Sichtbarkeiten betrachten
	- Ein Typ (Klasse oder Schnittstelle) enthält Attribute und Methoden
	- Ein Paket enthält Typen. 
	- Ein Modul enthält Pakete. 
	- Private Eigenschaften in einem Typ sind nicht in anderen Typen sichtbar. 
	- Nicht öffentliche Typen sind in anderen Paketen nicht sichtbar. 
	- Nicht exportierte Pakete sind außerhalb eines Moduls nicht sichtbar.

---

**Modultypen**

- Benannte Module
- Automatische Module
- Unbenannte Module

```
code> java --list-modules 
java.base@17.0.2 
java.compiler@17.0.2 
java.datatransfer@17.0.2 
java.desktop@17.0.2 
java.instrument@17.0.2 
java.logging@17.0.2 
java.management@17.0.2 
java.management.rmi@17.0.2 
java.naming@17.0.2 
… 
jdk.unsupported.desktop@17.0.2 
jdk.xml.dom@17.0.2 
jdk.zipfs@17.0.2 
```

-> Werkzeug *jmod* zeigt Informationen
	-> Name
	-> exports
	-> requires
	-> Plattform-Informationen

--- 

**Modulübersicht Java SE**

![[Pasted image 20260420124642.png]]

---

*Beispiele*

```java
module Ostfalia.Greeter {
    exports de.ostfalia.greeter;
    requires java.base;
}
```

```java
module Ostfalia.Main {
    exports de.ostfalia.main;
    requires Ostfalia.Greeter;
    requires java.base;
}
```

*Experimente:*

![[Pasted image 20260420124936.png]]

*Übersetzen und Packen von der Kommandozeile:

```bash
eclipse-workspace> cd .\Ostfalia.Greeter\
Ostfalia.Greeter> mkdir lib
Ostfalia.Greeter> javac -d .\bin\ .\src\module-info.java .\src\de\ostfalia\greeter\Greeter.java
Ostfalia.Greeter> jar --create --file=lib/Ostfalia.Greeter@1.0.jar --module-version=1.0 -C bin .
```

```bash
eclipse-workspace> cd .\Ostfalia.Main\
Ostfalia.Main> mkdir lib
Ostfalia.Main> javac -d bin --module-path ..\Ostfalia.Greeter\lib\ .\src\module-info.java .\src\de\ostfalia\main\Main.java
Ostfalia.Main> jar -c --file=lib/Ostfalia.Main@1.0.jar --main-class=de.ostfalia.main.Main --module-version=1.0 -C bin .
Ostfalia.Main> java -p 'lib;..\Ostfalia.Greeter\lib' -m Ostfalia.Main
```

