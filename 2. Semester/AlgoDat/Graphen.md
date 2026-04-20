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


