### Hashverfahren Prinzip

- Speicherung in Feld 0 bis n-1, einzelne Positionen odt als "buckets" bezeichnet
- Hashfunktion h(e) bestimmt für Element e die Postition im Feld
- h sorgt für "gute", kollisionsfreie bzw. kollisionsarme Verteilung der Elemente -> kann nur annäherungsweise von Gleichverteilung erreicht werden (sonst Elemente unbekannt)

**Beispiel** 

- Array von 0 bis 9, h(i ) = i mod 10 
- Array nach Einfügen von 42 und 119
- Möglichkeit der Kollision: Versuch des Eintragens von 69

![[Pasted image 20260420090111.png]]

## Hashfunktion

**Grundlagen**

- hängen von Datentyp und Anwendung ab
- für int oft h(i) = i mod N (funktioniert gut wenn N = Primzahl)
- für andere Datentypen -> Rückführung auf int
	- float: addiere Mantisse und Exponent
	- Strings: addiere ASCII/Unicode-Werte der Buchstaben (evtl. jeweils mit Faktor gewichtet)
	![[Pasted image 20260420090553.png]]

---

**Anforderungen**

- sollen die Werte gut streuen
- eventuell von Besonderheiten der Eingabewerte abhängig (Buchstaben des Alphabets in Namen tauchen unterschiedlich häufig auf!)
- effizient berechenbar
- gleiche Objekte haben gleichen Hash-Wert
- Hash-Wert ist deterministisch (z.B. nicht zeitabhängig)

---

*Ungünstige Hashfunktionen:*

![[Pasted image 20260420090827.png]]

---

**Kollisionsstrategien**

- Verkettung der Überläufer bei Kollisionen
	- verkettete Listen
	- Idee: alle Einträge einer Array-Position werden in einer verketteten Liste verwaltet
	
	![[Pasted image 20260420091053.png]]

- lineares Sondieren (Suchen nach alternativer Positionen)
	- Idee: falls T[h(e)] besetzt ist, versuche T[h(e) + 1], T[h(e) + 2], ..., T[(h(e) + 1) mod N]
	- Varianten: 
	1. T [h(e) + c ], T [h(e) + 2c ], T [h(e) + 3c ], … für Konstante c
	2. T [h(e) + 1], T [h(e) − 1], T [h(e) + 2], T [h(e) − 2], ...
	![[Pasted image 20260420091444.png]]

- quadratisches Sondieren
	- Idee: falls T[h(e)] besetzt ist, versuche T [h(e) + 1], T [h(e) + 4], T [h(e) + 9], .., T [h(e) + i² ]... -> jeweils mod N
	- Variante:  T [h(e) + 1], T [h(e) − 1], T [h(e) + 4], T [h(e) − 4], ... -> wähle Primzahlen für N!
	![[Pasted image 20260420091750.png]]

---

**Aufwand beim Hashen**

- bei geringer Kollisionswahrscheinlichkeit: 
	- Suchen in O(1)
	- Einfügen in O(1)
	- Löschen bei Sondierverfahren: nur Markieren der Einträge als gelöscht O(1), oder Rehashen der gesamten Tabelle notwendig O(n)
- Füllgrad über 80%: Einfüge- / Suchverhalten wird schnell dramatisch schlechter aufgrund von Kollisionen (bei Sondieren)

*Verkettete Überläufer:*

- Füllgrad (load factor) $\alpha\,=\,m/N$ -> m Anzahl gespeicherter Elemente, N Anzahl Buckets
- erfolglose Suche (Hashing mit Überlaufliste): $O(1+\alpha)$ 
- erfolgreiche Suche ebenfalls in dieser Größenordnung

*Sondieren:*

- Aufwand beim Suchen: $\frac{1}{1-\alpha}$
- typische Werte: $\frac{1}{1-0.5} = 2$, oder $\frac{1}{1-0.9} = 10$
- exakte Analyse basiert auf Aufsummierung der Wahrscheinlichkeit, dass i Elemente den Eimer i belegen (bei Gleichverteilung)
	- Bucket belegt -> Wahrscheinlichkeit $\alpha$
	- Bucket belegt und sondiertes Bucket auch belegt -> ungefähr $\alpha^2$ 
	- $1 + \alpha + \alpha^2 + \alpha^3 + \dots = \frac{1}{1-\alpha}$ 

--- 

**Dynamische Hashverfahren**

- statische Hashverfahren:
	- fester Bildbereich
	- mehr Einträge erfordern Neuaufbau einer größeren Hash-Tabelle
	-> Kopieren aller Elemente
	-> Neuberechnung der Hash-Werte aller Elemente!
- dynamische Hashverfahren
	- dynamisch wachsende und schrumpfende Bildbereiche

--- 

*Übung:*

1. Erstellen Sie eine Hash-Tabelle mit N = 7 und Verkettung der Überläufer 
2. Füge ein (in dieser Reihenfolge): 10, 4, 14
3. Suche 13, suche 4 (welche Positionen werden betrachtet?)
4. Füge weiter ein (in dieser Reihenfolge): 17, 6, 13
5. Suche 11, suche 13, suche 15 
6. Lösche 6 
7. Suche 13

*Lösung:*

Hashfunktion: $h(e)=e\,mod\,7$

1. Indizes berechnen: 
	- 10: $10\,mod\,7=3$
	- 4: $4\,mod\,7=4$
	- 14: $14\,mod\,7=0$

Zustand der Tabelle:

| **Index** | **Kette (Liste)** |
| --------- | ----------------- |
| 0         | 14                |
| 1         | /                 |
| 2         | /                 |
| 3         | 10                |
| 4         | 4                 |
| 5         | /                 |
| 6         | /                 |
2. Suche:
	- 13: an Stelle 6 -> nichts gefunden
	- 4: an Stelle 4 -> 4 gefunden

3. Indizes berechnen:
	- 17: Position 3 -> Kollision -> an die Kette angehängt
	- 6: Position 6
	- 13: Position 6 -> an die Kette angehängt

Zustand der Tabelle:

| **Index** | **Kette (Liste)**   |
| --------- | ------------------- |
| 0         | 14                  |
| 1         | /                   |
| 2         | /                   |
| 3         | 10 $\rightarrow$ 17 |
| 4         | 4                   |
| 5         | /                   |
| 6         | 6 $\rightarrow$ 13  |
4. Suche:
	- 11: an Stelle 4 -> nur 4 gefunden
	- 13: an Stelle 6 -> Kette wird durchlaufen, 13 gefunden
	- 15: an Stelle 1 -> nichts gefunden

5. Löschen und Suchen
	- Lösche 6: an Stelle 6, 6 wird entfernt, nur 13 bleibt
	- Suche 13: an Stelle 6, wird sofort gefunden

**Endzustand der Tabelle**

|**Index**|**Kette (Liste)**|
|---|---|
|0|14|
|1|/|
|2|/|
|3|10 $\rightarrow$ 17|
|4|4|
|5|/|
|6|13|

