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


## Realisierung von Graphen

- Übliche Datenstrukturen: Realisierungen unterscheiden sich im Platzbedarf bei der Speicherung und bei der Komplexität der Basisoperationen (Kante einfügen / löschen, Knoten einfügen / löschen)
	- dynamische Datenstruktur
	- Kanten- und Knotenlisten
	- Matrixdarstellung
- direkte Realisierung als dynamische Datenstruktur
	- analog zu Bäumen
	- Knoten mit Zeiger-Liste
	- unüblich da komplexe Strukturen mit vielen Fehlermöglichkeiten (beim Speichern)

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
