## Skalarprodukt

- Produkt zweier Vektoren -> Ergebnis Skalar
- Abbildung $s:V\,\times\,V\rightarrow\mathbb R$ 
- Symmetrie: $S(x,y)=S(y,x)$ 
- Positive Definiertheit: $S(x,y)>0$ für $x\neq0$ 
- Linearität: $s(x,y+z) = s(x,y)+s(x,z)$ und $s(c\cdot x,y)=c\cdot s(x,y)$

---
**Spezialfall: Standardskalarprodukt**

$$s:\mathbb R^n \times \mathbb R^n \rightarrow \mathbb R$$
$$s(x,y)=x_1\cdot y_1+x_1\cdot y_2+...+x_n\cdot y_n$$
-> symmetrisch, positiv definiert, linear

---
## Euklidischer Raum und Vektorraum

==Euklidischer Raum:== reeller Vektorraum mit Skalarprodukt

==Euklidischer Vektorraum:== reeller Vektorraum mit Standardskalarprodukt

---
**Schreibweise des Skalarproduktes im euklidischen Vektorraum:**

Varianten: $<x,y>$ , $x^T\cdot\,y$ 
Symmetrie: $x^T\cdot\,y=y^T\cdot\,x$
Positive Definiertheit: $x^T\cdot\,x<0$
Linearität: $x^T\cdot(y+z)=x^T\cdot y+x^T\cdot z$ und $(c\cdot x)^T\cdot y=c\cdot(x^T\cdot y)$

---
## Norm und Orthogonalität

Norm/Länge/Betrag von x: $$||x||=\sqrt{x^T\cdot\,x}$$
- Vektor ist normiert, wenn gilt: $||x||=1$ 

*Beispiel siehe Vorlesungsfolien und Goodnotes*

- Euklidischer Raum $\mathbb R^n:$
$$||x||=\sqrt{\sum^n_{i=1}x^2_i}$$ 
- für n = 1: Betrag einer Zahl
- für n = 2: Satz des Pythagoras
- für n = 3: Länge eines Ortsvektors *x* in einem kartesischen Koordinatensystem
- für kanonischen Einheitsvektor: $||e_i||=1$ für $i=1,...,n$

---
 **Cauchy-Schwarzsche Ungleichung**

$$|x^T\cdot y|\leq ||x||\cdot||y||$$
---
**Eigenschaften der Norm**

1. $$||x||\geq0\,\,\, \forall x\in V$$
2. $$||x||=0\Leftrightarrow x=o\,\,\,\forall x \in V$$
3. $$||c\cdot x||=|c|\cdot ||x||\,\,\,\forall x\in V,c\in\mathbb R$$
4. *Dreiecksungleichung*
$$||x+y|| \leq ||x||+||y||\,\,\,\forall x,y\in V$$

-> Nachweis siehe Skript und Vorlesung

---
**Erzeugung normierter Vektoren**

- aus jedem Vektor $x$ mit $x\neq o$ normierter Vektor erzeugen:
$$\frac{1}{||x||}\cdot x$$
---
## Winkel zwischen Vektoren

- aus Cauchy-Schwarzscher Ungleichung folgt (für $x,y\neq o$): 
$$cos(\alpha)=\frac{x^T\cdot y}{||x||\cdot ||y||} ;\,\, \alpha\in[0,\pi]$$
- orthogonal wenn gilt:
$$x^T\cdot y= 0$$
-> vor allem zur Erzeugung von Basen interessant (immer l.u. und einfacher zu rechnen)

---
*gegeben: Euklidischer Vektorraum $V$ und eine Menge $M = \{v_1, ... , v_r\}$ von Vektoren aus $V$ mit $v_i\neq0$ für $i=1,...,r$* 

- $M$ heißt ==Orthogonalsystem== wenn je zwei Vektoren orthogonal sind, also wenn $v^T_i \cdot v_j = 0$ für $i\neq j;\, i,j=1,...,r$ gilt
- $M$ heißt ==Orthonormalsystem== wenn sie ein Orthogonalsystem ist und aus normierten Vektoren besteht

- wenn $M$ = Orthogonalsystem und Basis von $V$, dann ist die Basis eine ==Orthogonalbasis== 
- wenn $M$ = Orthonormalsystem und Basis von $V$, dann ist die Basis eine ==Orthonormalbasis==

***=> Die Vektoren jedes Orthogonalsystems sind linear unanbhängig!*** (Nachweis siehe Skript)

---
