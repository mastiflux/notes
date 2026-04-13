## B-Bäume (s. [[11.AuD_Bäume3.pdf]])

![[Pasted image 20260413082940.png]]

- ein Verweis mehr für bspw. Vergleiche

### Einfügen

![[Pasted image 20260413083634.png]]

- immer auf Blattseite einfügen
- ggf. Überlauf behandeln (mittleres Element eine Ebene nach oben, bis es passt)
### Suchen

![[Pasted image 20260413084841.png]]

- es werden nur die Seiten geladen, in denen tatsächlich gesucht wird
- bspw. bei Schlüssel 38 -> es werden nur die Seiten 0, 2, 7 geladen

### Löschen

*Ausgleichen bei m=2*![[Pasted image 20260413085628.png]]
*Zusammenlegen bei m=2* 
![[Pasted image 20260413090320.png]]

- bei weniger als m Elementen auf Seite -> Unterlauf behandeln! (durch Ausgleichen mit benachbarter Seite oder Zusammenlegen zweier Seiten zu einer Seite)

### Komplexität der Operationen

- Aufwand bei allen Operationen im B-Baum immer O(log<sub>m</sub> n)
## Digitale Bäume (s. [[12.AuD_Bäume4.pdf]])

### Einschub: BucketSort 

- Vorraussetzung: Schlüssel bestehen aus Zeichen eines endlichen Alphabets, für das ein totale Ordnung definiert ist, Schlüssel können „punktweise“ verglichen werden
- sortiert zuerst nach letzter Dezimalstelle, dann nach vorletzter etc.

![[Pasted image 20260413101120.png]]

### HeapSort

- Priority Queue ist eine abstrakte Datenstruktur, die folgende Operationen effizient erlaubt: 
	- Object getMinimum(): liefert und entfernt das kleinste Element der Queue -> ist immer unten links _O(log n)_ 
	- void add(Object o): fügt ein Element in die Queue ein _O(log n)_
	- PriorityQueue(Object[] array): erzeugt aus dem Feld eine Prioritätswarteschlange

**Heap** 
- Binärbaum, der folgende Eigenschaften hat:
	- Der Baum ist vollständig, d.h. die Blattebene ist von links nach rechts gefüllt 
	- Der Schlüssel eines jeden Knotens ist kleiner (oder gleich) als die Schlüssel seiner Kinder
	- Die Wurzel ist immer das kleinste Element

![[Pasted image 20260413112732.png]]

### Tries

- digitaler Baum mit Zeichenketten
- Index, der alle Wörter umfasst
- Knoten: Feld mit der Kardinalität des verwendeten Alphabets
- Buchstaben selbst nicht abgespeichert!

![[Pasted image 20260413104044.png]]
*Beispiel:*
![[Pasted image 20260413104151.png]]

#### Binäre Tries

- Suchen schneller als binäre Bäume 8in jedem Schritt nur ein Zeichen verglichen)
- Nachteil: viele Kombinationen von Buchstaben ungenutzt
- Binärer Trie: Schlüssel als Binärzahl auffassen

*Beispiel, Schlüssel sind char:*
![[Pasted image 20260413104546.png]]

##### Implementierung Java
![[Pasted image 20260413105116.png]]
![[Screenshot_20260413_105147.png]]
![[Pasted image 20260413105250.png]]
![[Pasted image 20260413105333.png]]
