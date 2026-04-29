## Permutationen

**Eine bijektive Abbildung $\pi$ einer endlichen Menge $X$ in sich wird als _Permutation_ von $X$ bezeichnet. Man schreibt: $\pi\,:\,X\rightarrow X$ 

---
### Eigenschaften

1. **Schreibweise**

- in 2xn Matrix -> erste Zeile = Elemente von X, zweite Zeile = Bild des Elements der ersten Zeile

	$\pi = \begin{pmatrix} 1&2&3&...&n \\ \pi(1) & \pi(2) & \pi(3) & ... & \pi(n) \end{pmatrix}$

2. **Verknüpfung**

- Verknüpfung zweier Permutationen
- Beipiel: 

	$\pi_{1} = \begin{pmatrix} 1 & 2 & 3 \\ 2 & 3 & 1 \end{pmatrix} \,,\, \pi_{2}= \begin{pmatrix} 1 & 2 & 3 \\ 3 & 2 & 1 \end{pmatrix}$

![[Pasted image 20260422082854.png]]

--> nicht kommutativ

_Ausnahme:_ 
$X = {1,2}$ 

3. **Symmetrische Gruppe**

=> Menge aller Permutationen von $X={{1,...,n}} : S_{n}$ 
$S_{n}$ : Symmetrische Gruppe auf n Elementen
- bildet bzgl. der Komposition von Abbildungen einer Gruppe:
	- neutrales Element
	- Inverses
	- Assoziativität

---
### Transposition

**Eine Permutation $\pi$, die nur die Elemente  i und i,j vertauscht und alle anderen Elemente unverändert lässt, bezeichnet man als Transposition. Man gibt sie auch kurz an mit: $\pi = (i,j)$**

-> mittels Kompositionen von Transpositionen kann man jede Permutation darstellen
--> für eine feste Permutation ist die Anzahl Transpositionen immer gerade/ungerade

- man kann jeder Permutation ein _Signum_ zuordnen ($sign(\pi)$)

![[Pasted image 20260422084753.png]]

"Anzahl Tauschen gerade/ungerade -> Signum gerade/ungerade"

_Eigenschaften des Signum_

- $sign(\pi_{1}\circ\pi_{2})=sign(\pi_{1})*sign(\pi_{2})$
- $sign(\pi^{-1})=sign(\pi)$

---
## Leibnizsche Determinantenformel

**Jeder quadratischen Matrix lässt sich eindeutig eine Zahl zuordnen, die Determinate. Man bezeichnet sie als:
$A=n\,x\,n\,Matrix\,über\,Körper\,K$
$det(A)=|A|$**

---
## Berechnung der Determinanten

- bilde alle Permutationen von {1, 2,..., n}
- bestimme für jede Permutation $\pi$ das Produkt
	$a_{1,\pi(1)}\,*\,a_{2,\pi(2)}\,*\,...\,*\,a_{n,\pi(n)}$
- summiere alle Produkte unter Berücksichtigung von $sign(\pi)$

**_Formel:_**

$\det(A) = \sum_{\pi \in S_n} \text{sign}(\pi) \prod_{i=1}^{n} a_{i,\pi(i)}$

---

_Beispiele:_

**2x2 Matrix**
![[Pasted image 20260422090944.png]]
![[Pasted image 20260422091012.png]]

**3X3 Matrix**
![[Pasted image 20260422091055.png]]
![[Pasted image 20260422091200.png|289]]

=> Regel von Sarrus (**nur bei 3x3 Matritzen!**)

-> durchgezogene Linie = positives Vorzeichen, gestrichelte Linie = negatives Vorzeichen

---
## Eigenschaften von Determinanten

**Linearität von Determinanten**

- $det(A)$ linear in jeder Zeile und Spalte
- Spalten: 
	$det(s_1,...,s_j+s^*_j,...,s_n)$ 
	$=det(s_1,...,s_j,...,s_n)+det(s_1,...,s^*_j,...,s_n)$
	$det(s_1,...,k\cdot s_j,...,s_n)$ 
	$=k\cdot det(s_1,...,s_j,...,s_n)$
- Zeilen analog

**Determinante der Transponierten**

- bei $n\,x\,n\,Matrix\,A$ 
- $det(A)=det(A^T)$ 

**Determinante spezieller Matrizen**

- ist ein Zeilen- oder Spaltenvektor $0$, gilt $det(A)=0$
- ist A in Zeilenstufenform, so gilt: $det(A)=a_{11}\cdot a_{22}\,\cdot\, ...\,\cdot\, a_{nn}$ (nur Diagonale multiplizieren) und $det(E_n)=1$
- sind zwei Zeilen- oder Spaltenvektoren gleich, gilt $det(A)=0$ 

---
### Determinante und elementare Umformung der Matrix A

	gegeben: n x n Matrix A,B über Körper K

1. Zeilen/Spalten in A tauschen liefert B
-> $det(A)=-det(B)$
2. Zeile/Spalte in A mit c $\in$ K multiplizieren liefert B
-> $det(B)=c\cdot det(A)$
3. $c\cdot A = B$ 
-> $det(B)=c^n\cdot det(A)$
4. eine Zeile/Spalte ist Vielfaches einer anderen Zeile/Spalte
-> $det(A)=0$
5. Vielfaches einer Zeile/Spalte von A zu einer anderen addieren liefert B
-> $det(B)=det(A)$ 

---
### Determinante und Matrixprodukt

$det(A\cdot B)=det(A)\cdot det(B)$
$det(A\cdot B)=det(B\cdot A)$

bei invertierbarer Matrix A:
$det(A^{-1})=1\div det(A)$ 

---
### Weitere Eigenschaften

- A hat genau dann den Rang n wenn $det(A)\neq 0$ gilt
- A ist genau dann invertierbar, wenn $det(A)\neq 0$ gilt
- A heißt reguläre Matrix, wenn $det(A)\neq0$ gilt

---
## Berechnung der Determinanten mit Gauß-Algorithmus

1. Matrix A mithilfe des Gauß-Algorithmus in Zeilenstufenform bringen
2. Anzahl der Zeilenvertauschungen $k$ bestimmen
3. Determinante berechnen: $det(A)=(-1)^k\cdot a_{11}\cdot\,a_{22}\,\cdot\,...\,\cdot\,a_{nn}$ 

*Beispiel:*

![[Pasted image 20260429085547.png]]

---
## Entwicklungssatz von Laplace

- Ziel: Determinante einer $n\,x\,n$ Matrix auf $2\,x\,2$ Determinanten zurückführen
 
---
**Adjunkte eines Matrixelements**

Bestimmung der Adjunkten von $a_{ij}$:
- streiche i-te Zeile und j-te Spalte
- Restmatrix heißt $M_{ij}$ 
- Adjunkte $A_{ij}$ von $a_{ij}$ -> $A_{ij}=(-1)^{i+j}det(M_{ij})$ (schachbrettartiges Muster entsteht)
![[Pasted image 20260429091742.png]]

---

1. Entwicklung nach i-ter Zeile
-> $det(A)=a_{i1}A_{i1}+a_{i2}A_{i2}+...\,=\sum^{n}_{j=1}{a_{ij}A_{ij}}$ 

2. Entwicklung nach j-ter Spalte
-> $det(A)=a_{j1}A_{j1}+a_{j2}A_{j2}+...\,=\sum^n_{i=1}{a_{ij}A_{ij}}$ 

---
### Berechnung der Inversen mit der Adjunkten einer Matrix

**Adjunkte einer Matrix**

- Adjunkte von A $adj(A)=(A_{ij})^T$ 
- ==wichtig:== Transponieren erst zum Schluss!

*Beispiel:*

![[Pasted image 20260429094046.png]]

---
