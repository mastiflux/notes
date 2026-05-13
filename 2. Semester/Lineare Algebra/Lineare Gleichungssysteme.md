## Lösbarkeit

## Struktur der Lösungsmenge

	Zu jedem inhomogenen Gleichungssystem gehört ein homogenes Gleichungssystem.

Inhomogen:
$A * \vec{x} = \vec{b}$
Homogen:
$A * \vec{x} = \vec{o}$

---
### Räumliche Struktur der Lösungsräume

$f:\mathbb R^n \rightarrow \mathbb R^m$
Kern(f) = $\{ v\in\mathbb R^n|f(v)=0\}$
Bild(f) = $\{ f(v) \in \mathbb R^n|v\in\mathbb R^n\}$

---
**Orthogonalität von Untervektorräumen**

Wenn $\forall u\in U$

---
**Urbildraum $\mathbb R^n$**

- Vektoren aus Kern(f) - lösen $A\cdot x =o$ -> sind orthogonal zu Zeilenvektoren
- Zeilenvektoren von A - spannen Zeilenraum auf
=> *Kern(f) ist orthogonal zum Zeilenraum*
$$dim Kern(f)=n-rg(A)$$ 
- Dimension Zeilenraum: $rg(A)$
---
**Zielraum $\mathbb R^m$**

- $\mathbb R^m$ - Orthogonalbasis $\{b_1,...,b_m\}$
- Bild(f) - Teilmenge des $\mathbb R^m$, $dim Bild(f) = rg(A) = r$, Basisvektoren $b_1,...b_r$, wird von Spaltenvektoren von A aufgespannt ("Spaltenraum")
- Raum, in dem keine Bilder liegen - Basisvektoren $b_{r+1},...b_m$

> Bild(f) und Raum, in dem keine Bilder sind, sind *orthogonal*

---

![[Pasted image 20260513084300.png]]
![[Pasted image 20260513084650.png]]

*Wenn f injektiv:*
- Kern(f)=$\{o\}$
- Dimension von Bild(f): $m-r$

Darf $m>n$ gelten? -> ja

Bsp. $f:\mathbb R^2\rightarrow\mathbb R^3$ --> A = 3 x 2 Matrix
- dim Kern(f) = n - rg(A) = 0

*Wenn f surjektiv:*
- keine Bilder von f -> $o$
- dim Bild(f): $m$

*Wenn f bijektiv:*

- Kern(f)=$\{o\}$ => dim Kern(f) = 0
- wegen $n = dim Kern(f) + dim Bild (f)$ folgt $dim Bild(f) = n$
- $dimBild(f)=m$ (da surjektiv) -> `n = m`
$$f:\mathbb R^n\rightarrow \mathbb R^n$$
==Argumentation über Dimensionen!!==
---
## Bestimmung der Lösungsmenge mit Gauß-Jordan-Algorithmus

- Die Lösungsmenge eines lineare GLS bleibt bei elementaren Zeilenumformungen unverändert!

### Gauß-Jordan-Form einer Matrix A

**Vorraussetzungen:**

1. A ist in Zeilenstufenform
2. Alle Leitkoeffizienten sind $1$
3. Über den Leitkoeffizienten stehen nur Nullen

*Beispiel:*

![[Pasted image 20260415203137.png]]
![[Pasted image 20260415203215.png]]
![[Pasted image 20260415204416.png]]

- Matrix in Zeilenstufenform bringen mit Gauß-Algorithmus
- Leitkoeffizienten auf $1$ bringen 
- Zahlen über Leitkoeffizienten auf $0$ bringen mit elementaren Zeilenumformungen
--> Gauß-Jordan-Form erreicht

*Trick:*

- Nullzeilen einschieben, damit Diagonale durch Leitkoeffizientem entsteht (wenn schon da, einfach benutzen, sonst erzeugen)
- Auf Diagonalen in den neuen Zeilen $-1$ einfügen -> Basisvektoren des Kerns ablesbar

Anhand der Lösungsmenge:

$n\,Komponenten \Rightarrow f:\mathbb{R}^n \rightarrow \mathbb{R}^{Anzahl\,der\,Nicht-Null-Komponenten}$
$m\,Basisvektoren\,des\,Kerns\Rightarrow dim(Kern(f))=m$

*Ohne Trick:*

- Gleichungssystem normal lösen
- Basisvektoren bestehen aus unbestimmbaren Variablen

![[Pasted image 20260415203957.png]]
![[Pasted image 20260415204235.png]]
![[Pasted image 20260415204306.png]]


--- 
### Bestimmung der Inversen einer Matrix

- $n * n$ Matrix A

- $A * A^{-1} = E$ 

A ist invertierbar <=> n inhomogenes GLS ist lösbar <=> rg(A)=n

![[Pasted image 20260415204613.png]]
![[Pasted image 20260415204642.png]]






