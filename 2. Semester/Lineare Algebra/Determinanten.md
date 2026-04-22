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
## Leibinzsche Determinantenformel

**Jeder quadratischen Matrix lässt sich eindeutig eine Zahl zuordnen, die Determinate. Man bezeichnet sie als:
$A=n\,x\,n\,Matrix\,über\,Körper\,K$
$det(A)=|A|$**

---
### Berechnung der Determinanten

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

