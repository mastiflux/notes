- Netzwerke aus Knoten und Kanten
- vielfältiger Einsatz:
	- Verbindungsnetzwerke: Bahnnetz, Flugverbindungen, etc.
	- Verweise: WWW, Wikipedia, Literaturverweise
	- technische Modelle: Platinenlayouts, Computergraphik
	- Softwaredokumentation
- Bäume und Listen sind spezielle Graphen

## Definition

**Ein ungerichteter Graph G = (V, E) besteht aus einer endlichen Menge V von Knoten (vertices, auch Ecken) und einer Menge E von zwei-elementigen Teilmengen von V, den Kanten (edges).**

-> Bei einer Kante k = {a, b} mit a, b ∈ V heißen a, b Endpunkte von k.
- a und b sind benachbart und mit k indiziert

*Beispiel für ungerichteten Graphen:*

![[Pasted image 20260420101654.png]]

--- 

**Ein gerichteter Graph (Digraph) G = (V, E) besteht aus einer endlichen Menge V von Knoten und einer Menge E von geordneten Paaren (a, b) ∈ V × V mit a ≠ b, den gerichteten Kanten.**

-> Bei einer Kante k = (a, b) heißt a Anfangspunkt, b Endpunkt von k.
- a und b sind benachbart und mit k inzident

- E: (irreflexive) Relation in V -> irreflexiv => keine Schleifen
- Ungerichteter Graph ~> gerichteter Graph: jede Kante durch zwei gerichtete Kanten ersetzen -> symmetrische Relation

*Beispiel für gerichteten Graphen:*

![[Pasted image 20260420102157.png]]

---

**Gewichtete Graphen**

- Graph + Kantengewicht $\gamma$, z.B. natürliche Zahlen
- $G=(V,E,\gamma)$ mit $\gamma:E\rightarrow\mathbb{N}$ (gerichtet oder ungerichtet)
- Anwendungen: Suche kürzeste Wege, maximalen Fluss, minimale Transportkosten, etc.

*Beispiele für gewichteten Graphen*

![[Pasted image 20260420102751.png]]
![[Pasted image 20260420102919.png]]

---
## Realisierung von Graphen

- Übliche Datenstrukturen: Realisierungen unterscheiden sich im Platzbedarf bei der Speicherung und bei der Komplexität der Basisoperationen (Kante einfügen / löschen, Knoten einfügen / löschen)
	- dynamische Datenstruktur
	- Kanten- und Knotenlisten
	- Matrixdarstellung
- direkte Realisierung als dynamische Datenstruktur
	- analog zu Bäumen
	- Knoten mit Zeiger-Liste
	- unüblich da komplexe Strukturen mit vielen Fehlermöglichkeiten (beim Speichern)

---
## Kanten- und Knotenlisten

- einfache Realisierunf bei durchnummerierten Knoten
- als Austauschformat geeignet
- Auflistung nach Knoten oder nach Kanten sortiert
- Knoten von 1 bis |V| durchnummeriert
- Kanten als Paare von Knotenzahlen dargestellt

### Kantenlisten

- 1. Element: Knotenanzahl
- 2. Element: Kantenanzahl
- danach Liste der Kanten als jeweils 2 Zahlen dargestellt

*Beispiel:*
-> Kantenliste für G<sub>g</sub>: 6, 11, 1, 2, 1, 3, 3, 1, 4, 1, 3, 4, 3, 6, 5, 3, 5, 5, 6, 5, 6, 2, 6, 4

![[Pasted image 20260420105044.png]]

### Knotenlisten

- 1. Element: Knotenanzahl
- 2. Element: Kantenanzahl
- danach Liste der Kanteninformationen: Anzahl der von einem Knoten ausgehenden Kanten und die Liste der Zielknoten
- dabei geht man Knoten der Reihe nach durch

*Beispiel:*
-> Knotenliste für G<sub>g</sub>: 6, 11, 2, 2, 3, 0, 3, 1, 4, 6, 1, 1, 2, 3, 5, 3, 2, 4, 5

![[Pasted image 20260420105413.png]]

### Vergleich

- Knotenlisten benötigen weniger Speicher als Kantenlisten
- Kantenlisten: $2+2*|E|$
- Knotenlisten: $2+|V|+|E|$
- Erinnerung: $|V|-1 ≤ |E| ≤ |V|^2$ 

### Adjazenmatrix

- Graphen G=(V,E) ohne Schleifen und ohne Mehrfachkanten
- Knoten und Kanten nummeriert
	- V = {v<sub>1</sub>,..., v<sub>n</sub>}
	- E = {e<sub>1</sub>,..., e<sub>n</sub>}

**Definition**

Die Adjazenzmatrix eines gerichteten Graphen G ist eine n×n Matrix mit den Elementen:
-> a<sub>i,j</sub> = 1 falls v<sub>i</sub>, v<sub>j</sub> ∈ E
-> a<sub>i,j</sub> = 0

- ungerichtete Graphen: symmetrisch
- keine Schleifen => Hauptiagonale 0

**Operationen**

- Feststellen ob eine Kante (a,b) existiert: O(1)
- Matrixmultiplikation A$^k$ -> nur 0 oder 1 (transitive Hülle)
- Speicherbedarf: O(|V|$^2$) -> meist viele Nullen
- Initialisierung Zeit: O(|V|$^2$) -> wenn nur O(|V|) Kanten ineffizient

*Beispiel:*

![[Pasted image 20260420113047.png]]

### Adjazenlisten

- für jeden Knoten eine Liste von benachbarten Knoten
- Platzbedarf O(|V| + |E|)
- vorteilhaft, wenn |E| << |V|$^2$ 
- Test, ob a und b direkt verbunden sind: O(deg), wobei deg der maximale Grad des Graphen ist

![[Pasted image 20260420110555.png]]

### Transformation zwischen den Darstellungen

- jede Darstellung kann in jede andere ohne Informationsverlust transformiert werden
- Aufwand varriert

- seien n = |V| und m = |E|

| **Operation**   | **Kantenliste** | **Knotenliste** | **Adjazenmatrix** | **Adjazenliste** |
| --------------- | --------------- | --------------- | ----------------- | ---------------- |
| Einfügen Kante  | O(1)            | O(n+m)          | O(1)              | O(1)/O(n)        |
| Löschen Kante   | O(m)            | O(n+m)          | O(1)              | O(n)             |
| Einfügen Knoten | O(1)            | O(1)            | O(n$^2$)          | O(1)             |
| Löschen Knoten  | O(m)            | O(n+m)          | O(n$^2$)          | O(n+m)           |

---
## Implementierung in Java

- Node
	- Name(String)
	- Nachbarn als ArrayList
	- intern vergebene Knotennummern
- Edge
	- Kante: Ziel, Gewicht
	- Quelle: implizit bekannt
- HashMap -> Zuordnung Knotenname

```java
public class Edge { 
	int dest, cost; public Edge(int d, int c) { 
	dest = d; 
	cost = c; 
	} 
}

public class Node { 
String name; 
ArrayList<Edge> edges; 

public Node(String n) { 
	name = n; 
	edges = new ArrayList<>(); 
} 

public List<Edge> getEdges() { 
	return edges; 
} 

public String toString() { 
	StringBuilder sb = new StringBuilder(name); 
	for (Edge k : edges) { 
	sb.append(" -- " + k.toString()); 
	} 
	return sb.toString(); 
	} 
} 

public class Graph { 
	private HashMap<String, Integer> labels; 
	private ArrayList<Node> nodes; 
	
	public Graph() { 
		labels = new HashMap<String, Integer>(); 
		nodes = new ArrayList<Node>(); 
	} 
	
	public void addNode(String label) throws NodeAlreadyDefinedException { 
		if (labels.containsKey(label)) throw new NodeAlreadyDefinedException(); 
		nodes.add(new Node(label)); 
		int idx = nodes.size() - 1; 
		labels.put(label, idx); 
	}
	
	public int getNodeID(String label) { 
		Integer i = labels.get(label); 
		if (i == null) throw new NoSuchElementException(); 
		return i.intValue(); 
	} 
	
	public void addEdge(String src, String dest, int cost) { 
		Node node = nodes.get(getNodeID(src)); 
		node.getEdges().add(new Edge(getNodeID(dest), cost)); 
	}
	
	public Iterator<Edge> getEdges(int node) { 
		return nodes.get(node).getEdges().iterator(); 
	}
	
	public String toString() { 
		StringBuilder sb = new StringBuilder(); 
		for (int i = 0; i < nodes.size(); i++) { 
		sb.append(i + ": "); sb.append(nodes.get(i).toString() + "\n"); 
		} 
		return sb.toString(); 
	}
}
```

---
## Spannbäume

- ist ein Baum, der alle Knoten von einem Graphen G und nur Kanten aus G enthält

**Breitendurchlauf**

- erzeuge eine Warteschlange
- markiere alle Kanten, die vom aktuellen Knoten zu noch nicht markierten Knoten führen, und packe diese Nachbarn in die Warteschlange
- gehe zum nächsten Knoten in der Warteschlange -> bis sie leer ist

- Warteschlange dient als Zwischenspeicher
- Farbmarkierungen geben Status an (weiß = unbearbeitet, grau = in Bearbeitung, schwarz = abgearbeitet)
- pro Knoten wird Entfernung zum Start berechnet
- Vorgängerkanten bilden aufgespannten Baum mit Startknoten als Wurzel

*Pseudocode*

```
algorithm BFS(G, s) 
Eingabe: ein Graph G, ein Startknoten s des Graphen aus V[G] 
foreach Knoten u aus V[G] \ s do 
	farbe[u] := weiß; d[u] := unendlich; pi[u] := null 
od 
farbe[s] := grau; d[s] := 0; pi[s] := null; 
Q = new Queue; Q.enter(s); 
while ! Q.isEmpty() do 
	u := Q.leave(); 
	foreach v in „Zielknoten ausgehender Kanten“ von u do 
		if farbe[v] = weiß then 
			farbe[v] := grau; d[v] := d[u] + 1; pi[v] := u; 
			Q.enter(v) 
		fi 
	od 
	farbe[u] := schwarz 
od 
```

*Beispiel: Schritt für Schritt*

![[Pasted image 20260427101742.png]]

---

**Tiefendurchlauf**

- realisiert rekursiven Durchlauf durch gerichteten Graphen
- Farbmarkierungen identisch

- rekursiver Abstieg
- pro Knoten zwei Werte (+ Farbe)
- rekursiver Aufruf nur bei weißen Knoten
- Komplexität: $O(|V|+|E|)$
- alle von dem Knoten ausgehenden Kanten werden genau einmal aufgerufen

*Pseudocode*

```
algorithm DFS(G) 
	Eingabe: ein Graph G 
	foreach Knoten u aus V[G] do 
		farbe[u] := weiß; 
		pi[u] := null 
	od 
	zeit := 0; 
	foreach Knoten u aus V [G] do 
		if farbe[u] = weiß then 
			DFSvisit(u) 
		fi 
	od 

algorithm DFSvisit(u) 
	Eingabe: ein Knoten u 
	farbe[u] := grau; zeit := zeit+1; 
	d[u] := zeit;
	foreach v in Zielknoten ausgehender Kanten von u do 
		if farbe[v] = weiß then 
			pi[v] := u; 
			DFSvisit(v) 
		fi 
	od 
farbe[u] := schwarz; zeit := zeit+1; 
f[u] := zeit;
```

*Beispiel: Schritt für Schritt*

![[Pasted image 20260427102556.png]]
![[Pasted image 20260427102616.png]]

---
### Topologisches Sortieren

- sortiert nach ==Nachbarschaft==, nicht nach totaler Ordnung
- DFS erstellt topol. Ordnung "on-the-fly"
- Sortierung nach f-Wert (invers) ergibt Reihenfolge
- keine explizite Sortierung!

---

*Beispiel zertstreuter Professor*

![[Pasted image 20260427104526.png]]
![[Pasted image 20260427104539.png]]

---
## Kürzeste Wege

- Graph G = (V,E,$\gamma$) mit einer Gewichtsfunktion $\gamma : E \rightarrow \mathbb N$ 
- Weg durch G ist Liste von aneinanderstoßenden Kanten: $$P = (v_1,v_2),(v_2,v_3), ... , (v_{n-1}, v_n)$$
- Gewicht oder Länge des Pfades: Aufsummierung der einzelnen Kantengewichte $$w(P)=\sum^{n-1}_{i=1}\gamma((v_i,v_i+1))$$
---
### Kruskals Algorithmus

- gegeben: Teilmenge der Knoten/Kanten, optimaler Spannbaum
- füge Knoten/Kante hinzu
- Erweiterung des Spannbaums, dass er immernoch optimal ist
- möglich mit *Kruskal-Algorithmus*

***Kruskal:***

	Führe den folgenden Schritt so oft wie möglich aus: Wähle unter den noch nicht ausgewählten Kanten von G (dem Graphen) die kürzeste Kante, die mit den schon gewählten Kanten keinen Kreis bildet.

==Formalisierung:==
```
algorithm Kruskal(V, E, w)
	Eingabe V: Knotenmenge, E: Kantenmenge, w: E → ℝ Gewicht 
	E1 := ∅
	while (V,E1) nicht zusammenhängend do 
		wähle die kleinste Kante e ∈ E \ E1, 
		die zwei Komponenten in (V, E1) verbindet
		{oder die keinen Kreis erzeugen}
	E1 := E1 ∪ {e}
od
```

==Komplexität:== $O(|V|^2\cdot |E|)$

==Optimierungen:==

1. Suche nach kleinster Kante: wird immer wieder benötigt mithilfe anderer Datenstrukturen (bspw. sorted list, priority queue, heap)   -> $O(|V|\cdot |E|+|E|\cdot log(|E|))$

2. Testen, ob (V, E1 ∪ {e}) keinen Kreis enthält: KeinKreis(V,E), z.B. Tiefensuche, geeignete Operation *Union-Find*

---

**Union-Find**
==Verkettete Listen:==
- find: $O(n)$
- union: $O(n)$

![[Pasted image 20260511084729.png]]

==Bäume:==
- find: $O(h)$ (maximal n, balanciert log(n))
- union: $O(1)$

- Union: kleineren Baum an größeren hängen
- Find: Kind direkt an Wurzel hängen (Pfadkompression)

![[Pasted image 20260511085050.png]]

---

==Gesamtverfeinerung Kruskal:==
![[Pasted image 20260511085247.png]]

*Beispiel:*

![[Pasted image 20260511085350.png]]
![[Pasted image 20260511085413.png]]

---
### Djiktras Algorithmus

- Suche nach optimalem Pfad zu jedem Knoten, nicht optimaler Spannbaum
- iterative Erweiterung einer Menge von "billig" ereichbaren Knoten

![[Pasted image 20260511085923.png]]

*Initialisierung:*
Q = < (s:0), (u: ∞), (v: ∞), (x: ∞), (y: ∞) >
![[Pasted image 20260511090244.png]]
*Schrittfolge:*

Q = < (x: 5), (u: 10), (v: ∞), (y: ∞) >
![[Pasted image 20260511090316.png]]

Q = < (y: 7), (u: 8), (v: ∞) >
![[Pasted image 20260511090333.png]]

Q = < (u: 8), (v: 13) >
![[Pasted image 20260511090352.png]]

Q = < (v: 9) > 
![[Pasted image 20260511090425.png]]

*Übung:*
![[Pasted image 20260511091523.png]]

A=0, B=2, C=7, D=6, E=10, F=3, G=5, H=9, I=7, J=6, K=10

---
### A*-Algorithmus

- g(u): bisher berechnete Kosten vom Startpunkt zu u
- h(u): geschätzte minimale Kosten von u zum Ziel
	- h für Heuristik
	- sollte leicht zu berechnen sein
	- h(u)<=d(u)
	- h(u)<=h(v)+$\gamma$(u,v)
- f(u)=g(u)+h(u)

- OPEN: Liste aller Knoten, zu denen ein Weg gefunden wurde (mit f-Werten)
	- Operationen: create, contains, enqueue, extractMinimal, isEmpty
- CLOSED: Knoten, zu denen der kürzeste Weg gefunden wurde
- jeder Knoten: 
	- h(u) Konstante 
	- π(u) Zeiger zum aktuellen Vorgänger
	- g(u) wird berechnet

![[Pasted image 20260511094158.png]]

*Ablauf:*
- Startknoten in OPEN Liste eintragen
- solange OPEN nicht leer und Ziel nicht erreicht ist:
	- Knoten mit kleinstem f in OPEN wählen
	- u = Ziel: fertig
	- Nachbarn von u und v betrachten: 
		- v schon in CLOSED: ignorieren
		- f-Wert bei Weg über u und v berechnen (wenn schon besser vorhanden -> ignorieren, sonst v in OPEN eintragen und f und g aktualisieren)
	- u aus OPEN in CLOSED transferieren

Beispiel siehe Vorlesungsfolien mit Notizen

---
### Bellman-Ford-Algorithmus

- geht auch mit negativen $\gamma(u,v)$ 

![[Pasted image 20260511100806.png]]

*Beispiel:*
![[Pasted image 20260511100841.png]]

| Iteration | 0        | 1                     | 2   | 3   |
| --------- | -------- | --------------------- | --- | --- |
| A         | 0        | 0                     | 0   | 0   |
| B         | $\infty$ | 3                     | 3   | 3   |
| C         | $\infty$ | $\infty+6$            | 9   | 9   |
| D         | $\infty$ | $\infty+5,\,\infty-2$ | 8,7 | 7   |

---
