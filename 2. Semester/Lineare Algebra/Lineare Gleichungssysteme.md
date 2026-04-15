## Lösbarkeit

## Struktur der Lösungsmenge

	Zu jedem inhomogenen Gleichungssystem gehört ein homogenes Gleichungssystem.

Inhomogen:
$A * \vec{x} = \vec{b}$
Homogen:
$A * \vec{x} = \vec{o}$

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

### Bestimmung der Inversen einer Matrix

- $n * n$ Matrix A

- $A * A^{-1} = E$ 

A ist invertierbar <=> n inhomogenes GLS ist lösbar <=> rg(A)=n

![[Pasted image 20260415204613.png]]
![[Pasted image 20260415204642.png]]






