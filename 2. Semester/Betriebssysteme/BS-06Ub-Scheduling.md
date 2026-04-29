## Übung: Scheduling

---

## Aufgabe 1: Unix-Scheduler

Der Unix-Scheduler arbeitet für Prozess *j* in Schritt *i* mit den dynamischen Prioritäten:

$$P_j^i = P_{base} + P_{nice,j} + \frac{T_{CPU,j}(i)}{2} \quad \text{und} \quad T_{CPU,j}^i = \frac{T_{CPU,j}(i-1)}{2}$$

Die Basispriorität $P_{base}$ wird vom System bestimmt. Der Nice-Level für Prozess *j* kann vom Nutzer mit dem Kommando `nice` angepasst werden. $T_{CPU,j}$ ist ein Maß für die in der jüngeren Vergangenheit von Prozess *j* verbrauchte CPU-Zeit. Wir betrachten nur die Prozesszustände **Running** (Laufend, L) und **Ready/Runnable** (Bereit, B).

Gegeben seien 3 Prozesse mit den Anfangsprioritäten $P_1^0 = 40$, $P_2^0 = 40$, $P_3^0 = 50$. Der Timertakt, mit dem $T_{CPU}$ hochgezählt wird, sei $f = 100\,\text{Hz}$. Ein neuer Schritt mit Neuberechnung der Prioritäten werde jede Sekunde vorgenommen.

Bestimmen Sie für jeden Prozess $P_{base} + P_{nice,j}$ (Sie benötigen nur die Summe) und ergänzen Sie die nachstehende Tabelle:

| **Zeit** | **Zustand P1** | **P1ⁱ** | **T_CPU,1** | **Zustand P2** | **P2ⁱ** | **T_CPU,2** | **Zustand P3** | **P3ⁱ** | **T_CPU,3** |
| :------: | :------------: | :-----: | :---------: | :------------: | :-----: | :---------: | :------------: | :-----: | :---------: |
|    0     |       L        |   40    |    0…100    |       B        |   40    |      0      |       B        |   50    |      0      |
|    1     |       B        |   65    |     50      |       L        |   40    |      0      |       B        |   50    |      0      |
|    2     |       B        |   52    |     25      |       B        |   65    |     50      |       L        |   50    |      0      |
|    3     |       L        |   46    |     12      |       B        |   52    |     25      |       B        |   65    |     50      |
|    4     |       B        |   68    |     56      |       L        |   46    |     12      |       B        |   62    |     25      |
|    5     |       L        |   54    |     28      |       B        |   68    |     56      |       B        |   56    |     12      |
|    6     |       B        |   68    |     57      |       B        |   54    |     28      |       L        |   53    |      6      |
|    7     |       B        |   54    |     28      |       L        |   47    |     14      |       B        |   76    |     53      |
|    8     |       L        |   47    |     14      |       B        |   68    |     57      |       B        |   63    |     26      |
|    9     |       B        |   68    |     57      |       L        |   54    |     28      |       B        |   56    |     13      |
|    10    |       B        |   54    |     28      |       B        |   72    |     64      |       L        |   53    |      6      |
|    11    |       L        |   47    |     14      |       B        |   56    |     32      |       B        |   76    |     53      |
|    12    |       B        |   68    |     57      |       L        |   48    |     16      |       B        |   63    |     26      |

> **Formeln:**
> $P_j^i = P_{base} + P_{nice,j} + \dfrac{T_{CPU,j}(i)}{2}$ &nbsp;&nbsp; und &nbsp;&nbsp; $T_{CPU,j}^i = \dfrac{T_{CPU,j}(i-1)}{2}$

---

## Aufgabe 2: Scheduling-Verfahren und Metriken

### Prozessübersicht

| **Prozess** | **Arrival Time** | **Service Time** | **Priorität (1 = hoch)** |
|:-----------:|:----------------:|:----------------:|:------------------------:|
| **A**       | 0                | 3                | 9                        |
| **B**       | 2−ε              | 6                | 1                        |
| **C**       | 4−ε              | 4                | 3                        |
| **D**       | 6−ε              | 5                | 4                        |
| **E**       | 8−ε              | 2                | 2                        |

Gegeben seien die oben angegebenen Prozesse. Bestimmen Sie die Abfolge der Prozesse für die angegebenen Scheduling-Verfahren (→ Gantt-Diagramm, Dispatcher vernachlässigen). Eine Arrival Time 2−ε bedeutet, dass der Prozess sehr kurz vor dem Zeitpunkt 2 in das System kommt. Zum Zeitpunkt 2 steht er zur Verfügung.

Geben Sie jeweils die durchschnittliche **Turnaround Time** und **normalisierte Turnaround Time** an. Vergleichen Sie die normalisierte Turnaround Time für die verschiedenen Scheduling-Verfahren.

---

### 2.1 Shortest Process Next (SPN) — nicht-präemptiv

| **Prozess** | **Arrival Time** | **Service Time** | **Priorität (1 = hoch)** |
|:-----------:|:----------------:|:----------------:|:------------------------:|
| **A**       | 0                | 3                | 9                        |
| **B**       | 2−ε              | 6                | 1                        |
| **C**       | 4−ε              | 4                | 3                        |
| **D**       | 6−ε              | 5                | 4                        |
| **E**       | 8−ε              | 2                | 2                        |

**Gantt-Diagramm:**

| 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   | 10  | 11  | 12  | 13  | 14  | 15  | 16  | 17  | 18  | 19  | 20  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| A   | A   | A   | B   | B   | B   | B   | B   | B   | E   | E   | C   | C   | C   | D   | D   | D   | D   | D   |     |     |


**Metriken:**

| **Prozess** | **Completion Time** | **Turnaround Time** | **norm. Turnaround Time** |
|:-----------:|:-------------------:|:-------------------:|:-------------------------:|
| A           |          3          |          3          |             1             |
| B           |          9          |          7          |           1,17            |
| C           |         15          |         11          |           2,75            |
| D           |         20          |         14          |            2,8            |
| E           |         11          |          3          |            1,5            |
| **avg**     |                     |         7,6         |           1,84            |

---

### 2.2 Shortest Remaining Time First (SRTF) — präemptiv

| **Prozess** | **Arrival Time** | **Service Time** | **Priorität (1 = hoch)** |
|:-----------:|:----------------:|:----------------:|:------------------------:|
| **A**       | 0                | 3                | 9                        |
| **B**       | 2−ε              | 6                | 1                        |
| **C**       | 4−ε              | 4                | 3                        |
| **D**       | 6−ε              | 5                | 4                        |
| **E**       | 8−ε              | 2                | 2                        |

**Gantt-Diagramm:**

| 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   | 9   | 10  | 11  | 12  | 13  | 14  | 15  | 16  | 17  | 18  | 19  | 20  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| A   | A   | A   | B   | C   | C   | C   | C   | E   | E   | B   | B   | B   | B   | B   | D   | D   | D   | D   | D   |     |

**Metriken:**

| **Prozess** | **Completion Time** | **Turnaround Time** | **norm. Turnaround Time** |
|:-----------:|:-------------------:|:-------------------:|:-------------------------:|
| A           |          3          |          3          |             1             |
| B           |         15          |         13          |           2,17            |
| C           |          8          |          4          |             1             |
| D           |         20          |         14          |            2,8            |
| E           |         10          |          2          |             1             |
| **avg**     |                     |         7,2         |           1,59            |

---

### 2.3 Round-Robin (RR) — Zeitscheibenlänge Z = 2

Einreihung neuer Prozesse kurz (ε) vor der angegebenen Arrival Time.

| **Prozess** | **Arrival Time** | **Service Time** | **Priorität (1 = hoch)** |
|:-----------:|:----------------:|:----------------:|:------------------------:|
| **A**       | 0                | 3                | 9                        |
| **B**       | 2−ε              | 6                | 1                        |
| **C**       | 4−ε              | 4                | 3                        |
| **D**       | 6−ε              | 5                | 4                        |
| **E**       | 8−ε              | 2                | 2                        |

**Gantt-Diagramm:**

| 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 |
|---|---|---|---|---|---|---|---|---|---|----|----|----|----|----|----|----|----|----|----|-----|
|   |   |   |   |   |   |   |   |   |   |    |    |    |    |    |    |    |    |    |    |    |

**Metriken:**

| **Prozess** | **Completion Time** | **Turnaround Time** | **norm. Turnaround Time** |
|:-----------:|:-------------------:|:-------------------:|:-------------------------:|
| A           |                     |                     |                           |
| B           |                     |                     |                           |
| C           |                     |                     |                           |
| D           |                     |                     |                           |
| E           |                     |                     |                           |
| **avg**     |                     |                     |                           |

---

### 2.4 Statische Prioritäten (SP) — präemptiv

| **Prozess** | **Arrival Time** | **Service Time** | **Priorität (1 = hoch)** |
|:-----------:|:----------------:|:----------------:|:------------------------:|
| **A**       | 0                | 3                | 9                        |
| **B**       | 2−ε              | 6                | 1                        |
| **C**       | 4−ε              | 4                | 3                        |
| **D**       | 6−ε              | 5                | 4                        |
| **E**       | 8−ε              | 2                | 2                        |

**Gantt-Diagramm:**

| 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 |
|---|---|---|---|---|---|---|---|---|---|----|----|----|----|----|----|----|----|----|----|-----|
|   |   |   |   |   |   |   |   |   |   |    |    |    |    |    |    |    |    |    |    |    |

**Metriken:**

| **Prozess** | **Completion Time** | **Turnaround Time** | **norm. Turnaround Time** |
|:-----------:|:-------------------:|:-------------------:|:-------------------------:|
| A           |                     |                     |                           |
| B           |                     |                     |                           |
| C           |                     |                     |                           |
| D           |                     |                     |                           |
| E           |                     |                     |                           |
| **avg**     |                     |                     |                           |
