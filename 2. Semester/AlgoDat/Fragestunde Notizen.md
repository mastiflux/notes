## Komplexität

**O-Notation**

- Man sagt: f hat eine Komplexität von O(g)
	- f wächst höchstens so schnell wie g.
	- Statt f ∈ O(g) oft auch f=O(g) geschrieben.  
	- Landau Symbole O, o, Ω, ω, Θ

- f, g: $\mathbb N\rightarrow \mathbb R^+$ sind zwei Funktionen
-> dann gilt: $f \in \mathcal{O}(g) \iff \exists\, n_0 \in \mathbb{N},\; \exists\, c \in \mathbb{R}^+,\; \forall\, n \in \mathbb{N}: \quad n > n_0 \Rightarrow f(n) \leq c \cdot g(n)$ 
	- f wächst höchstens so schnell wie g
	- nur auf große n gucken, was vorher passiert ist egal
	- positive Konstante mit der man g multiplizieren darf um f zu überdecken
	- Punkt, ab dem gilt, dass f unter einer skalierten Version von g liegt

**Aufgaben**

1) $100\cdot\sqrt{n}+0,01\cdot\,n\in\ O\,(?)$ --> engste Schranke $O(n)$ 
2) $2^n + n^{100} + n! \in\, O(?)$ -> engste Schranke $O(n!)$ 
3) $n^{log_2\,n}+2^n\in\,O(?)$ -> engste Schranke $O(2^n)$ 
4) $2^{n+10}+5\cdot2^n\in\,O(?)$ -> engste Schranke $O(2^n)$
5) $2^{n+10}+5\cdot2^n+3x^2\in\,O(?)$ -> $O(2^n+x^2)$ 

---


