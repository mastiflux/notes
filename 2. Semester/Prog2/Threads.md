
- notwendig zur Parallelisierung von Prozessen
- durch Objekte der Klasse *java.lang.Threads* repräsentiert
- *Thread* implementiert *Runnable

---
**Erzeugung von Threads**

- Klasse schreiben, die die Anweisungen des Threads definiert -> muss entweder von *Thread* abgeleitet sein oder direkt *Runnable* implementieren (notwendig wenn bereits eigene Subklasse)
- run() Methode implementieren mit Programmcode des Threads -> start() ruft run() auf

---
*Beispiel:*

```java
public class ABCThread extends Thread { 
	
	@Override 
	public void run() { 
		for (char b = 'A'; b <= 'Z'; b++) { 
			// Gib den Buchstaben aus 
			System.out.print(b); 
			// Verbringe 100 Millisekunden mit “Pause” 
			MachMal.pause(100); 
		} 
		System.out.println(); 
	} 
}
```

```java
public class ManyThreads { 
	public static void main(String[] args) { 
		ABCThread t1 = new ABCThread(), t2 = new ABCThread(); 
		t1.start(); 
		t2.start(); 
	} 
}
```

Output: AABBCCDDEEFFGGHHIIJJKKLLMMNNOOPPQQRRSSTTUUVVWWXXYYZZ 

---
**Lebenszyklus eines Threads**

![[Pasted image 20260429101854.png]]
