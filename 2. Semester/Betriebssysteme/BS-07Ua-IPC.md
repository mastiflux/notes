# Betriebssysteme: Dateisysteme

**Prof. Dr. Claus Fühner — Ostfalia Hochschule, Informatik**

---

## Übung: Interprozesskommunikation 1

---

## Aufgabe 1: Semaphoren und Ressourcen

Die Prozesse PA, PB und PC greifen mit den nachstehend angegebenen Operationen auf die zählenden Semaphore SA, SB und SC zu. Der Prozess PA hat die **höchste Priorität**, PB die nächste niedrigere und PC die **kleinste Priorität**. Der Scheduler arbeite prioritätsgesteuert und präemptiv.

| **Prozess PA** | **Prozess PB** | **Prozess PC** |
|:--------------:|:--------------:|:--------------:|
| (1) P(SA)      | (1) P(SB)      | (1) P(SC)      |
| (2) P(SA)      | (2) V(SC)      | (2) P(SC)      |
| (3) P(SA)      | (3) V(SA)      | (3) P(SC)      |
| (4) V(SB)      | (4) END        | (4) V(SB)      |
| (5) END        |                | (5) V(SB)      |
|                |                | (6) END        |

Die Semaphore sind für die vier Fälle wie in der Tabelle angegeben initialisiert:

| Semaphor | **(a)** | **(b)** | **(c)** | **(d)** |
|:--------:|:-------:|:-------:|:-------:|:-------:|
| SA       | 2       | 3       | 2       | 0       |
| SB       | 0       | 0       | 1       | 0       |
| SC       | 2       | 2       | 1       | 3       |

Vollziehen Sie für alle vier Teilaufgaben (a)–(d) analog zum Beispiel in der Vorlesung nach, wie die Prozesse ablaufen, welchen Status sie im Verlauf haben und welche Werte die Semaphoren haben. In welcher Reihenfolge terminieren die Prozesse? Falls es zu Nebenläufigkeitsproblemen kommt, benennen Sie diese!

### Grundregeln

- **P(S):** Dekrementiert S. Wenn S danach < 0 → Prozess **blockiert**.
- **V(S):** Inkrementiert S. Wenn ein Prozess auf S wartet → wird er **aufgeweckt**.
- Scheduler ist **präemptiv prioritätsgesteuert**: PA hat höchste, PC niedrigste Priorität. Sobald ein höherpriorer Prozess bereit wird, verdrängt er den laufenden sofort.
- Alle drei Prozesse starten gleichzeitig. Da PA die höchste Priorität hat, läuft **PA zuerst**.

### Lösung (a): SA=2, SB=0, SC=2

| Schritt | Aktion | SA | SB | SC | PA | PB | PC | Erklärung |
|---------|--------|----|----|----|----|----|----|-----------|
| 1 | PA: P(SA) | **1** | 0 | 2 | läuft | bereit | bereit | SA=1 ≥ 0, kein Block |
| 2 | PA: P(SA) | **0** | 0 | 2 | läuft | bereit | bereit | SA=0 ≥ 0, kein Block |
| 3 | PA: P(SA) | **-1** | 0 | 2 | **blockiert** | bereit | bereit | SA=-1 < 0 → PA blockiert |
| 4 | PB: P(SB) | -1 | **-1** | 2 | blockiert | **blockiert** | bereit | SB=-1 → PB blockiert |
| 5 | PC: P(SC) | -1 | -1 | **1** | blockiert | blockiert | läuft | SC=1 ≥ 0, kein Block |
| 6 | PC: P(SC) | -1 | -1 | **0** | blockiert | blockiert | läuft | SC=0 ≥ 0 |
| 7 | PC: P(SC) | -1 | -1 | **-1** | blockiert | blockiert | **blockiert** | SC=-1 → PC blockiert |

> ⚠️ **Deadlock!** Alle drei Prozesse blockieren. SA=-1, SB=-1, SC=-1. Niemand kann mehr V() aufrufen. Kein Prozess terminiert.

### Lösung (b): SA=3, SB=0, SC=2

| Schritt | Aktion | SA | SB | SC | PA | PB | PC | Erklärung |
|---------|--------|----|----|----|----|----|----|-----------|
| 1 | PA: P(SA) | **2** | 0 | 2 | läuft | bereit | bereit | |
| 2 | PA: P(SA) | **1** | 0 | 2 | läuft | bereit | bereit | |
| 3 | PA: P(SA) | **0** | 0 | 2 | läuft | bereit | bereit | SA=0, kein Block |
| 4 | PA: V(SB) | 0 | **1** | 2 | läuft | bereit | bereit | SB=1 |
| 5 | PA: END   | 0 | 1 | 2 | **fertig** | bereit | bereit | **PA terminiert (1.)** |
| 6 | PB: P(SB) | 0 | **0** | 2 | — | läuft | bereit | SB=0 ≥ 0, PB läuft (höhere Prio als PC) |
| 7 | PB: V(SC) | 0 | 0 | **3** | — | läuft | bereit | SC=3 |
| 8 | PB: V(SA) | **1** | 0 | 3 | — | läuft | bereit | SA=1 |
| 9 | PB: END   | 1 | 0 | 3 | — | **fertig** | bereit | **PB terminiert (2.)** |
| 10 | PC: P(SC) | 1 | 0 | **2** | — | — | läuft | |
| 11 | PC: P(SC) | 1 | 0 | **1** | — | — | läuft | |
| 12 | PC: P(SC) | 1 | 0 | **0** | — | — | läuft | |
| 13 | PC: V(SB) | 1 | **1** | 0 | — | — | läuft | |
| 14 | PC: V(SB) | 1 | **2** | 0 | — | — | läuft | |
| 15 | PC: END   | 1 | 2 | 0 | — | — | **fertig** | **PC terminiert (3.)** |

> ✅ **Kein Problem.** Reihenfolge: **PA → PB → PC**.

### Lösung (c): SA=2, SB=1, SC=1

| Schritt | Aktion | SA | SB | SC | PA | PB | PC | Erklärung |
|---------|--------|----|----|----|----|----|----|-----------|
| 1 | PA: P(SA) | **1** | 1 | 1 | läuft | bereit | bereit | |
| 2 | PA: P(SA) | **0** | 1 | 1 | läuft | bereit | bereit | |
| 3 | PA: P(SA) | **-1** | 1 | 1 | **blockiert** | bereit | bereit | SA < 0 → PA blockiert |
| 4 | PB: P(SB) | -1 | **0** | 1 | blockiert | läuft | bereit | SB=0 ≥ 0, PB läuft (höhere Prio als PC) |
| 5 | PB: V(SC) | -1 | 0 | **2** | blockiert | läuft | bereit | SC=2 |
| 6 | PB: V(SA) | **0** | 0 | 2 | **bereit** | läuft | bereit | SA=0, PA aufgeweckt → PA verdrängt PB sofort! |
| 7 | PA: V(SB) | 0 | **1** | 2 | läuft | bereit | bereit | PA läuft weiter (höchste Prio) |
| 8 | PA: END   | 0 | 1 | 2 | **fertig** | bereit | bereit | **PA terminiert (1.)** |
| 9 | PB: END   | 0 | 1 | 2 | — | **fertig** | bereit | PB setzt fort und endet. **PB terminiert (2.)** |
| 10 | PC: P(SC) | 0 | 1 | **1** | — | — | läuft | |
| 11 | PC: P(SC) | 0 | 1 | **0** | — | — | läuft | |
| 12 | PC: P(SC) | 0 | 1 | **-1** | — | — | **blockiert** | SC=-1 → PC blockiert! |

> ⚠️ **Deadlock für PC!** PA und PB terminieren (Reihenfolge: **PA → PB**), aber PC blockiert dauerhaft auf SC.

### Lösung (d): SA=0, SB=0, SC=3

| Schritt | Aktion | SA | SB | SC | PA | PB | PC | Erklärung |
|---------|--------|----|----|----|----|----|----|-----------|
| 1 | PA: P(SA) | **-1** | 0 | 3 | **blockiert** | bereit | bereit | SA=-1 → PA sofort blockiert |
| 2 | PB: P(SB) | -1 | **-1** | 3 | blockiert | **blockiert** | bereit | SB=-1 → PB blockiert |
| 3 | PC: P(SC) | -1 | -1 | **2** | blockiert | blockiert | läuft | |
| 4 | PC: P(SC) | -1 | -1 | **1** | blockiert | blockiert | läuft | |
| 5 | PC: P(SC) | -1 | -1 | **0** | blockiert | blockiert | läuft | |
| 6 | PC: V(SB) | -1 | **0** | 0 | blockiert | **bereit** | läuft | SB=0, PB aufgeweckt → PB verdrängt PC! |
| 7 | PB: V(SC) | -1 | 0 | **1** | blockiert | läuft | bereit | |
| 8 | PB: V(SA) | **0** | 0 | 1 | **bereit** | läuft | bereit | SA=0, PA aufgeweckt → PA verdrängt PB sofort! |
| 9 | PA: P(SA) | **-1** | 0 | 1 | **blockiert** | bereit | bereit | SA=-1 → PA blockiert wieder! |
| 10 | PB: END   | -1 | 0 | 1 | blockiert | **fertig** | bereit | **PB terminiert (1.)** |
| 11 | PC: V(SB) | -1 | **1** | 1 | blockiert | — | läuft | |
| 12 | PC: END   | -1 | 1 | 1 | blockiert | — | **fertig** | **PC terminiert (2.)** |

> ⚠️ **Deadlock für PA!** PB und PC terminieren (Reihenfolge: **PB → PC**), aber PA blockiert dauerhaft auf SA.

### Zusammenfassung Aufgabe 1

| Fall | Terminierungsreihenfolge | Problem |
|------|--------------------------|---------|
| (a)  | keiner | **Deadlock** – alle drei Prozesse blockieren |
| (b)  | PA → PB → PC | ✅ kein Problem |
| (c)  | PA → PB, PC blockiert | **Deadlock** – PC blockiert dauerhaft auf SC |
| (d)  | PB → PC, PA blockiert | **Deadlock** – PA blockiert dauerhaft auf SA |

---

## Aufgabe 2: Wechselseitiger Ausschluss

**(a)** Kontoführung: Zwei Threads führen eine Funktion aus, die die gemeinsame Variable `balance` modifiziert:

```c
int balance = 100;

void transfer(int amount) {
    int temp = balance;   // 1
    temp = temp - amount; // 2
    balance = temp;       // 3
}
```

Gleichzeitig ruft Thread A `transfer(30)` und Thread B `transfer(50)` auf. Welche Endwerte kann `balance` aufnehmen? Wie nennt man das Problem? Nennen Sie für jeden möglichen Endwert eine Verzahnung (Interleaving), die dazu führt.

**Lösung:** Das korrekte Ergebnis wäre 100 − 30 − 50 = **20**. Durch Verzahnung der Schritte sind jedoch drei Endwerte möglich:

**Endwert 70** (B's Ergebnis wird überschrieben):
```
A1: temp_A = balance     → temp_A = 100
A2: temp_A = 100 - 30    → temp_A = 70
B1: temp_B = balance     → temp_B = 100
B2: temp_B = 100 - 50    → temp_B = 50
B3: balance = temp_B     → balance = 50
A3: balance = temp_A     → balance = 70  ← A überschreibt B's Ergebnis
```

**Endwert 50** (A's Ergebnis wird überschrieben):
```
A1: temp_A = balance     → temp_A = 100
B1: temp_B = balance     → temp_B = 100
B2: temp_B = 100 - 50    → temp_B = 50
A2: temp_A = 100 - 30    → temp_A = 70
A3: balance = temp_A     → balance = 70
B3: balance = temp_B     → balance = 50  ← B überschreibt A's Ergebnis
```

**Endwert 20** (korrekter sequentieller Ablauf):
```
A1: temp_A = balance     → temp_A = 100
A2: temp_A = 100 - 30    → temp_A = 70
A3: balance = temp_A     → balance = 70
B1: temp_B = balance     → temp_B = 70
B2: temp_B = 70 - 50     → temp_B = 20
B3: balance = temp_B     → balance = 20
```

Das Problem heißt **Race Condition** (Wettlaufsituation): Das Ergebnis hängt von der nicht-deterministischen Ausführungsreihenfolge ab.

**(b)** Identifizieren Sie in (a) den kritischen Abschnitt minimaler Größe.

**Lösung:** Der kritische Abschnitt umfasst die Zeilen, in denen auf die gemeinsam genutzte Variable `balance` zugegriffen wird — also Lesen (Zeile 1) und Schreiben (Zeile 3). Zeile 2 arbeitet nur auf der lokalen Variable `temp` und ist unkritisch. In der Praxis schützt man alle drei Zeilen gemeinsam, da Lesen und Schreiben atomar zusammengehören müssen:

```c
// kritischer Abschnitt (minimal):
int temp = balance;   // 1  ← liest balance  (kritisch)
temp = temp - amount; // 2  ← nur lokal      (unkritisch, aber im Schutzbereich)
balance = temp;       // 3  ← schreibt balance (kritisch)
```

**(c)** Ergänzen Sie den untenstehenden Pseudocode um geeignete Semaphor-Operationen `P()` und `V()`, um den Zugriff auf die Variable zu synchronisieren (inkl. Initialisierung):

| **Prozess PA**          | **Prozess PB**        |
|-------------------------|-----------------------|
| `void write() { V = ...; }` | `void read() { printf(v); }` |

**Lösung:**

```c
// Initialisierung:
Semaphor mutex = 1;  // binärer Semaphor, anfangs "frei"

// Prozess PA:
void write() {
    P(mutex);      // kritischen Abschnitt betreten
    V = ...;
    V(mutex);      // kritischen Abschnitt verlassen
}

// Prozess PB:
void read() {
    P(mutex);      // kritischen Abschnitt betreten
    printf(V);
    V(mutex);      // kritischen Abschnitt verlassen
}
```

`mutex = 1` bedeutet: der Abschnitt ist frei. Das erste `P(mutex)` setzt ihn auf 0 (belegt). Jeder weitere `P()`-Aufruf blockiert, bis `V(mutex)` den Semaphor wieder freigibt.

**(d)** Erklären Sie den Begriff **Race Condition**. Kann es bereits zu Race Conditions kommen, wenn nur eine einzige Variable (z.B. `V` aus dem obigen Aufgabenteil) genutzt wird und PA den Wert von `V` nur schreibt, ohne zu lesen?

**Lösung:** Eine **Race Condition** liegt vor, wenn das Ergebnis eines Programms von der zeitlichen Reihenfolge nicht synchronisierter Zugriffe mehrerer Threads auf gemeinsame Daten abhängt — das Ergebnis ist also nicht-deterministisch.

**Ja**, Race Conditions können auch bei einer einzigen Variable auftreten, selbst wenn nur geschrieben wird: Auf den meisten Architekturen ist das Schreiben einer Variable kein atomarer Vorgang — es besteht intern aus mehreren Maschineninstruktionen. Liest PB mitten in diesem Vorgang, kann er einen inkonsistenten Zwischenwert sehen (*torn read*). Beispiel: PA schreibt einen 64-Bit-Wert auf einer 32-Bit-Architektur in zwei Schritten; PB könnte die ersten 32 Bit des neuen Werts zusammen mit den letzten 32 Bit des alten Werts lesen.

**(e)** Nennen Sie Beispiele für in dieser Hinsicht **problematische** und **unproblematische** Datentypen!

**Lösung:**

**Unproblematisch** (atomar auf typischer Hardware):
- `char` (8 Bit) — ein einziger Maschinenzugriff
- `int` / `uint32_t` auf 32-Bit-Architektur, wenn speicherausgerichtet
- In C11/C++11: `_Atomic int` / `std::atomic<int>` — Atomizität durch Sprachstandard garantiert

**Problematisch**:
- `long long` (64 Bit) auf 32-Bit-Architektur — Schreiben benötigt zwei Maschinenbefehle
- `double` / `float` — Gleitkommaoperationen oft nicht atomar
- `struct` / zusammengesetzte Typen — mehrere Felder, niemals atomar
- Arrays / Strings — viele Einzelzugriffe, nie atomar

**Faustregel:** Nur Typen, die in einem einzigen ausgerichteten Maschinenwort-Zugriff gelesen/geschrieben werden, sind potenziell sicher — im Zweifelsfall immer synchronisieren.

**(f)** Wie unterscheidet sich ein **Deadlock** von einer **Race Condition**? Nennen Sie ein Beispiel für einen Deadlock!

**Lösung:**

| | **Race Condition** | **Deadlock** |
|---|---|---|
| **Was passiert?** | Falsches/unvorhersehbares Ergebnis durch unkontrollierte Zugriffe | Alle beteiligten Prozesse warten ewig aufeinander |
| **Programm läuft?** | Ja, aber mit falschem Ergebnis | Nein — vollständiger Stillstand |
| **Ursache** | Fehlende Synchronisation bei gemeinsamen Daten | Zirkuläres Warten auf Ressourcen |
| **Erkennbar?** | Schwer — tritt nicht-deterministisch auf | Leichter — System reagiert nicht mehr |

**Beispiel Deadlock:**

```c
Semaphor S1 = 1, S2 = 1;

// Thread A:          // Thread B:
P(S1);               P(S2);
P(S2);  // wartet    P(S1);  // wartet
...                  ...
V(S2);               V(S1);
V(S1);               V(S2);
```

Thread A hält S1 und wartet auf S2. Thread B hält S2 und wartet auf S1. Beide warten ewig — **zirkuläres Warten** (eine der vier Coffman-Bedingungen für Deadlocks).

---

## Aufgabe 3: Thread und Semaphore

Ähnlich zur Aufgabe über wechselseitigen Ausschluss soll der lesende Prozess PB nun so lange blockieren, bis ein neuer Wert in die Variable geschrieben wurde. Umgekehrt soll der schreibende Prozess so lange blockieren, bis die Variable ausgelesen wurde. Sie benötigen nun zwei Semaphoren, z.B. `voll` (hiermit wartet PB auf PA) und `leer` (für das umgekehrte Warten von PA auf PB). `write()` und `read()` werden von den entsprechenden Prozessen mehrfach aufgerufen.

| **Prozess PA**              | **Prozess PB**            |
|-----------------------------|---------------------------|
| `void write() { V = ...; }` | `void read() { printf(v); }` |

Ergänzen Sie den Pseudocode um die notwendigen Semaphor-Operationen (inkl. Initialisierung).

### Lösung

Dies ist das klassische **Produzent-Konsument-Problem** mit Puffergröße 1:

- PB soll warten, bis PA etwas geschrieben hat → Semaphor `voll`, init = **0**
- PA soll warten, bis PB den alten Wert gelesen hat → Semaphor `leer`, init = **1**

`leer = 1` bedeutet: der Puffer ist anfangs frei, PA darf sofort schreiben. `voll = 0` bedeutet: noch kein neuer Wert vorhanden, PB muss warten.

```c
// Initialisierung:
Semaphor voll = 0;   // PB wartet darauf, dass PA geschrieben hat
Semaphor leer = 1;   // PA wartet darauf, dass PB gelesen hat (1 = Puffer frei)

// Prozess PA (Produzent):
void write() {
    P(leer);         // warten, bis Puffer leer (PB hat gelesen)
    V = ...;         // neuen Wert schreiben
    V(voll);         // PB signalisieren: neuer Wert verfügbar
}

// Prozess PB (Konsument):
void read() {
    P(voll);         // warten, bis Puffer voll (PA hat geschrieben)
    printf(V);       // Wert lesen
    V(leer);         // PA signalisieren: Puffer wieder frei
}
```

**Ablaufbeispiel (mehrere Iterationen):**
```
PA: P(leer)  → leer=0  (Puffer belegen)
PA: V = 42             (schreiben)
PA: V(voll)  → voll=1  (PB aufwecken)
PB: P(voll)  → voll=0  (Wert abholen)
PB: printf(42)
PB: V(leer)  → leer=1  (PA freigeben)
PA: P(leer)  → leer=0  (nächste Runde)
...
```

Dieses Muster stellt sicher: PA schreibt nie einen Wert, bevor PB den vorherigen gelesen hat — und PB liest nie denselben Wert zweimal.
