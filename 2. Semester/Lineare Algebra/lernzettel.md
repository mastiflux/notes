# Lernzettel Lineare Algebra Klausur 2

---

## 1. Darstellungsmatrizen & Computergrafik

### Was ist das?
Eine **Darstellungsmatrix** übersetzt eine lineare Abbildung in eine Matrix. Da Vektoren in unterschiedlichen Koordinatensystemen (Basen) ausgedrückt werden können, hängt die Matrix davon ab, welche Basis im Startraum (Urbild) und welche im Zielraum (Bild) verwendet wird.

### Typische Klausuraufgabe: Darstellungsmatrix $A_{B_1, B_2}(f)$ bestimmen

**Lösungsrezept:**

1. **Einsetzen** – Nimm den ersten Basisvektor aus dem Startraum ($B_1$) und setze ihn in die Abbildungsvorschrift $f(x)$ ein.
2. **Linearkombination bilden** – Stelle das Ergebnis als Linearkombination der Basisvektoren des Zielraums ($B_2$) dar (kleines LGS lösen).
3. **Eintragen** – Die gefundenen Koeffizienten als **erste Spalte** in die Darstellungsmatrix eintragen.
4. **Wiederholen** – Für alle weiteren Basisvektoren aus $B_1$ und die restlichen Spalten füllen.

### Verkettung von Abbildungen
Bei $g \circ f$ (erst $f$, dann $g$) werden die Darstellungsmatrizen multipliziert – die Reihenfolge dreht sich um:
$$A(g \circ f) = A(g) \cdot A(f)$$

### Computergrafik (homogene Koordinaten)
Um Translationen als Matrixmultiplikation schreiben zu können, wird ein 2D-Vektor $(x, y)$ um eine 1 erweitert: $(x, y, 1)$.

| Transformation | Besonderheit |
|---|---|
| **Streckung (S)** | Faktoren stehen auf der Hauptdiagonale |
| **Rotation (D)** | Sinus/Cosinus in der Matrix; Drehung gegen den Uhrzeigersinn ist mathematisch positiv |
| **Translation (T)** | Verschiebungen stehen in der letzten Spalte |

> ⚠️ **Stolpersteine**
> - Koeffizienten immer als **Spalten** eintragen, nie als Zeilen!
> - Ist die Zielbasis die **kanonische Basis**, kann man sich das LGS sparen – die Ergebnisse aus Schritt 1 sind direkt die Spalten.

---

## 2. Lineare Gleichungssysteme (LGS)

### Theorie: Struktur des Lösungsraums
- $\dim\ker(f) + \dim\operatorname{Bild}(f) = n$ (Anzahl der Spalten)
- Der **Rang** der Matrix = Dimension des Bildes
- Ein LGS $A \cdot x = b$ ist nur lösbar, wenn $\operatorname{rg}(A) = \operatorname{rg}(A|b)$

### Typische Klausuraufgabe 1: LGS mit Gauß / Gauß-Jordan lösen

**Lösungsrezept:**

1. Erweiterte Matrix $(A|b)$ in **Zeilenstufenform** bringen.
2. **Lösbarkeit prüfen** – Steht in der letzten Zeile $0 = 4$, ist das LGS nicht lösbar.
3. **Gauß-Jordan** – Leitkoeffizienten auf 1 setzen und auch oberhalb der Stufen Nullen erzeugen.
4. Nullzeilen einfügen, damit die Stufen auf der Hauptdiagonale liegen. Hauptdiagonale mit $-1$ auffüllen.
5. Vektor ganz rechts → **spezielle Lösung**; Spalten mit $-1$ → **Basisvektoren des Kerns**.

### Typische Klausuraufgabe 2: LGS mit Parametern untersuchen

1. System in Zeilenstufenform bringen.
2. Parameterzeile (meist letzte Zeile) analysieren:

| Fall | Bedingung | Ergebnis |
|---|---|---|
| **Nicht lösbar** | Linke Seite = 0, rechte Seite ≠ 0 | Keine Lösung |
| **Eindeutig lösbar** | Linke Seite ≠ 0 | Genau eine Lösung |
| **Unendlich viele** | Gesamte Zeile = 0 | Lösungsschar |

> ⚠️ **Stolpersteine**
> - Rechenoperation neben jede Zeile schreiben (z. B. $\text{III} - 2 \cdot \text{I}$). Nur die abzuziehende Zeile multiplizieren!
> - Lösungsmenge vollständig angeben: $x_{inh} + c_1 v_1 + \dots$

---

## 3. Determinanten & Inverse Matrizen

### Was ist das?
Eine **Determinante** ist eine Kennzahl einer quadratischen Matrix. Ist sie ungleich null, ist die Matrix regulär (invertierbar) und das LGS eindeutig lösbar.

### Typische Klausuraufgabe 1: Determinante berechnen

| Größe | Methode |
|---|---|
| **2×2** | $\det = a \cdot d - b \cdot c$ |
| **3×3** | Regel von Sarrus |
| **Größere Matrizen** | Laplace-Entwicklung (Zeile/Spalte mit vielen Nullen wählen) |
| **Mit Gauß** | Obere Dreiecksform → Produkt der Hauptdiagonale (**Achtung:** Zeilentausch ändert Vorzeichen!) |

**Laplace:** Für jeden Eintrag der gewählten Zeile/Spalte: Zeile und Spalte streichen → Unterdeterminante mit Eintrag und Schachbrett-Vorzeichen ($+-+-\dots$) multiplizieren.

### Typische Klausuraufgabe 2: Inverse berechnen

**Gauß-Jordan** (empfohlen ab 3×3):
- $(A \mid E_n)$ aufschreiben, Gauß-Jordan anwenden bis links $E_n$ steht → rechts steht $A^{-1}$.

**Adjunkten-Methode** (besser für 2×2 / 3×3 mit Parametern):
$$A^{-1} = \frac{1}{\det(A)} \cdot \operatorname{adj}(A)$$
Adjunkte: Für jedes Element Unterdeterminante mit Schachbrett-Vorzeichen berechnen, dann transponieren.

### Cramersche Regel
Nur sinnvoll für kleine Systeme ohne Nullzeilen:
$$x_j = \frac{\det(A_j)}{\det(A)}$$
wobei $A_j$ entsteht, indem die $j$-te Spalte durch den Lösungsvektor $b$ ersetzt wird.

> ⚠️ **Stolpersteine**
> - **Sarrus nur bei 3×3!** Bei 4×4 zwingend Laplace oder Gauß nutzen.
> - Bei der Adjunkten-Methode das **Transponieren** am Ende nicht vergessen!
> - Beim Gauß: Zeile mit $c$ multiplizieren → Determinante multipliziert sich mit $c$. Besser: nur addieren/subtrahieren.

---

## 4. Euklidische Vektorräume & Orthogonalität

### Was ist das?
Das **Skalarprodukt** ermöglicht die Berechnung von Winkeln und Längen (Normen) in Vektorräumen.

### Typische Klausuraufgabe 1: Ist eine Abbildung ein Skalarprodukt?

Prüfe die 3 Axiome:

1. **Symmetrie:** $s(x,y) = s(y,x)$ – $x$ und $y$ vertauschen, Ergebnis vergleichen.
2. **Linearität:** $s(c \cdot x, y) = c \cdot s(x,y)$ und $s(x+y, z) = s(x,z) + s(y,z)$.
3. **Positive Definitheit:** $s(x,x) > 0$ für alle $x \neq 0$.

> ⚠️ **Fiese Falle bei der Positiven Definitheit:** Ergibt $s(x,x)$ z. B. $(x_1 + x_2)^2$, scheint das immer $\ge 0$ zu sein. Aber für $x_1 = -x_2$ (z. B. $(-1, 1)^T$) wird der Ausdruck $0$ – obwohl $x \neq 0$. Damit ist die Bedingung **nicht erfüllt**!

### Typische Klausuraufgabe 2: Winkel und Orthogonalität

**Orthogonalität:** Zwei Vektoren sind orthogonal, wenn ihr Skalarprodukt null ist:
$$x^T \cdot y = 0$$

**Winkel berechnen:**
$$\cos(\alpha) = \frac{x^T \cdot y}{\|x\| \cdot \|y\|}, \quad \|x\| = \sqrt{x_1^2 + x_2^2 + \dots}$$

### Orthogonal- und Orthonormalsysteme

| Begriff | Eigenschaft |
|---|---|
| **Orthogonalsystem** | Alle Vektoren paarweise senkrecht → automatisch linear unabhängig |
| **Orthonormalsystem** | Wie oben, zusätzlich hat jeder Vektor die Länge 1 |

**Normieren:** Vektor durch seine Länge teilen:
$$x_{\text{normiert}} = \frac{1}{\|x\|} \cdot x$$
