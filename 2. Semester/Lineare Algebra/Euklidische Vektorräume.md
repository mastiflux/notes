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

***=> Für jeden beliebigen Vektor $x\in V$ ($V$ ist ONS) gilt:*** 
$$\displaystyle x=(x^T\cdot v_1)v_1+(x^T\cdot v_2)v_2+...(x^T\cdot v_n)v_n$$ 
---
### Zerlegung von Vektoren, Projektion

- Betrachtung: zwei Vektoren $x$ und $y$, die Winkel $\alpha\neq0$ einschließen

![[Pasted image 20260506142913.png|503]]

Zerlegung: $$y = y_{||} + y_{\perp}$$
Norm von $y_{||}$: $$||y_{||}||=||y||\cdot cos(\alpha)=\frac{1}{||x||}\cdot x^T\cdot y$$
Richtung: $$\frac{1}{||x||}\cdot x^T\cdot y$$
Länge: $$y_{||}=\frac{x^T\cdot y}{||x||^2}\cdot x$$

---
## Vektorprodukt und Spatprodukt im $\mathbb R^3$

Definition: Das Vektorprodukt zweier Vektoren $a\times b$ wird definiert als:

$$a\times b = \begin{pmatrix}a_1\\a_2\\a_3\end{pmatrix}\times \begin{pmatrix}b_1\\b_2\\b_3\end{pmatrix}=\begin{pmatrix}a_2b_3-a_3b_2\\a_3b_1-a_1b_3\\a_1b_2-a_2b_1\end{pmatrix}$$
**Eigenschaften des Vektorprodukts:**

1. $a\times b$ ist orthogonal zu $a$ und $b$
2. $a\times b=-(b\times a)$ 
3. $a\times a =o$
4. Distributivität ist erfüllt
5. Vektorprodukt dreier Vektoren: $a\times(b\times c)=b\cdot(a^T\cdot c)-c\cdot (a^T\cdot b)$ 
6. $a,b$ und $a\times b$ bilden ein Rechtssystem
7. Norm von $a\times b$ entspricht Flächeninhalt des von $a$ und $b$ aufgespannten Parallelogramm $$||a\times b||=||a||\cdot||b||\cdot sin(\alpha)$$

---

**Spatprodukt**

Definition ($a,b,c\in \mathbb R^3$):
$$(a\times b)^T\cdot c$$

Berechnung mithilfe folgender Determinante: $$(a\times b)^T\cdot c=\begin{vmatrix}a_1&a_2&a_3\\b_1&b_2&b_3\\c_1&c_2&c_3\end{vmatrix}$$
