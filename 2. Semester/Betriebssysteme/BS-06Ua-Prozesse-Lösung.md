# Betriebssysteme: Übung – Prozesse

## Aufgabe 1: Prozesse in der Shell

### sleep 120 im Vordergrund anhalten

- **Tastenkombination:** `Strg+Z` – sendet das Signal **SIGTSTP** an den Vordergrundprozess und hält ihn an, ohne ihn zu beenden.
- **Zustand prüfen:**
  - `jobs` zeigt den Job als `[1]+ Stopped   sleep 120`
  - `ps -l` zeigt in der Spalte **S** den Buchstaben **T** (stopped/traced)
- **Im Hintergrund weiterlaufen lassen:** `bg` – sendet SIGCONT an den Prozess.  
  Danach zeigt `jobs` den Status `Running` und `ps -l` wieder den Status **S** (sleeping).

---

### Signale an sleep 300 senden (`kill -SIGNAL PID`)

| Signal    | Wirkung                                                                 |
|-----------|-------------------------------------------------------------------------|
| SIGSTOP   | Prozess wird sofort **angehalten** (nicht abfangbar)                    |
| SIGCONT   | Angehaltener Prozess wird **fortgesetzt**                               |
| SIGALRM   | Prozess wird **beendet** (Standardaktion: Terminierung)                 |
| SIGTERM   | Prozess wird **beendet** (kann abgefangen/ignoriert werden)             |
| SIGKILL   | Prozess wird **sofort zwangsbeendet** (nicht abfangbar, nicht ignorierbar) |

---

### Können Signale an Prozesse anderer Nutzer gesendet werden?

Nein (in der Regel nicht). Nur **root** kann Signale an beliebige Prozesse senden.  
Normale Nutzer dürfen Signale nur an eigene Prozesse (gleiche UID) senden.  
`kill()` liefert sonst `EPERM` (Permission denied). Das ist sinnvoll, da andernfalls ein Benutzer die Prozesse anderer Nutzer manipulieren oder beenden könnte.

---

### Unterschied SIGTERM vs. SIGKILL

| Merkmal           | SIGTERM (15)                              | SIGKILL (9)                          |
|-------------------|-------------------------------------------|--------------------------------------|
| Abfangbar         | ✅ Ja – Programm kann Handler definieren  | ❌ Nein – direkt vom Kernel behandelt |
| Ignorierbar       | ✅ Ja                                     | ❌ Nein                               |
| Aufräumen möglich | ✅ Ja – z. B. Dateien schließen           | ❌ Nein – Prozess stirbt sofort       |
| Empfehlung        | Erst SIGTERM senden, dann ggf. SIGKILL    | Nur als letztes Mittel               |

---

### Wozu dienen SIGSTOP und SIGCONT?

- **SIGSTOP:** Hält einen Prozess sofort an (suspendiert ihn). Der Prozess verbleibt im Speicher und kann später fortgesetzt werden. Kann weder abgefangen noch ignoriert werden.
- **SIGCONT:** Setzt einen angehaltenen Prozess (SIGSTOP oder SIGTSTP) fort.  
  Typische Verwendung: Job-Kontrolle in der Shell (`bg`, `fg`), Debugging (z. B. mit `gdb`).

---

### Welche Signale können nicht abgefangen werden?

**SIGKILL (9)** und **SIGSTOP (19)** können von keinem Programm abgefangen, ignoriert oder blockiert werden. Das Betriebssystem erzwingt ihre Wirkung direkt im Kernel.

---

### Wie entsteht ein Zombie-Prozess? Wie verhindert man ihn?

**Entstehung:**  
Wenn ein Kindprozess endet (`exit()`), gibt er seinen Exit-Status zurück. Der Kernel hält diesen Status vor, bis der **Elternprozess** ihn mit `wait()` oder `waitpid()` abgeholt hat. In der Zwischenzeit ist der Kindprozess ein **Zombie**: Er existiert noch in der Prozesstabelle (mit Status `Z`), belegt aber keine CPU oder nennenswerte Ressourcen.

**Verhinderung:**
1. **`wait()` / `waitpid()` im Elternprozess aufrufen** – holt den Exit-Status ab und entfernt den Zombie.
2. **SIGCHLD-Signal-Handler** im Elternprozess registrieren, der `wait()` aufruft, sobald ein Kind endet.
3. **`signal(SIGCHLD, SIG_IGN)`** setzen – der Kernel verwirft Exit-Status automatisch und erzeugt gar keinen Zombie.

**Rolle von Signal-Handlern:**  
Ein SIGCHLD-Handler wird ausgelöst, wenn ein Kindprozess seinen Zustand ändert (endet, gestoppt wird etc.). Ruft der Handler `wait()` auf, wird der Zombie sofort aufgeräumt, ohne dass der Elternprozess blockiert.

---

## Aufgabe 2: Prozesshierarchie

### Bedeutung der Spalten

| Spalte  | Bedeutung                                                              |
|---------|------------------------------------------------------------------------|
| UID     | User-ID des Prozessbesitzers (0 = root, 30 = anderer Benutzer)        |
| PID     | Prozess-ID (eindeutige Kennung des Prozesses)                          |
| PPID    | Parent-PID – PID des Elternprozesses                                   |
| PRI     | Priorität (niedriger = höhere Priorität)                               |
| NI      | Nice-Wert (Einfluss auf Scheduling-Priorität, 0 = neutral)             |
| WCHAN   | Wartekanal – Kernelfunktion, auf die der Prozess wartet                |
| STAT    | Prozesszustand (S = sleeping, R = running, T = stopped, Z = zombie)   |
| TIME    | Akkumulierte CPU-Zeit (MM:SS)                                          |
| COMMAND | Name/Pfad des ausgeführten Programms inkl. Argumente                   |

---

### Prozesshierarchie als Baum

```
PID 1 – init [3]
├── PID 159 – /usr/sbin/httpd          (UID 0)
│   ├── PID 160 – /usr/sbin/httpd      (UID 30)
│   └── PID 443 – /usr/sbin/httpd      (UID 30)
├── PID 173 – /usr/sbin/inetd          (UID 0)
│   └── PID 537 – in.rshd -L           (UID 0)
│       └── PID 538 – rxvt             (UID 0)
│           └── PID 539 – -bash        (UID 0)
│               └── PID 591 – ./iperf -s  (UID 0)
└── PID 323 – java                     (UID 0)
    └── PID 360 – java                 (UID 0)
```

**Erläuterung:**
- `init` (PID 1) ist der Urprozess aller anderen Prozesse (PPID = 0).
- `httpd` (PID 159) wurde von `init` gestartet und hat zwei Worker-Kindprozesse (160, 443).
- `inetd` (PID 173) ist ein Netzwerkdienst, der bei eingehenden Verbindungen `in.rshd` (537) startet.
- `rxvt` → `bash` → `iperf` bildet eine interaktive Shell-Session mit einem laufenden Netzwerktest.
- Die beiden `java`-Prozesse (323, 360) sind eine JVM mit einem nativen Thread.

---

## Aufgabe 3: Prozesse mit fork() & Co in C

### Funktionsbeschreibungen (man-Pages)

**`signal(int signum, sighandler_t handler)`**  
Registriert eine Funktion (`handler`) als Signal-Handler für das Signal `signum`.  
Rückgabe: vorheriger Handler oder `SIG_ERR` bei Fehler.  
Sonderwerte für `handler`: `SIG_IGN` (ignorieren), `SIG_DFL` (Standardaktion).

**`kill(pid_t pid, int sig)`**  
Sendet das Signal `sig` an den Prozess (oder die Prozessgruppe) mit der PID `pid`.  
Rückgabe: 0 bei Erfolg, -1 bei Fehler (z. B. EPERM, ESRCH).

**`sleep(unsigned int seconds)`**  
Legt den aufrufenden Prozess für `seconds` Sekunden schlafen.  
Rückgabe: verbleibende Sekunden, falls durch ein Signal unterbrochen (sonst 0).

**`wait(int *wstatus)`**  
Blockiert den Elternprozess, bis ein Kindprozess seinen Zustand ändert (z. B. endet).  
Schreibt Exit-Status in `*wstatus` (falls nicht NULL). Entfernt Zombie-Einträge.  
Rückgabe: PID des beendeten Kindprozesses, -1 bei Fehler.

---

### fork()-Beispiel mit globalen Variablen a und b

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>

int a = 10, b = 20;  // globale Variablen

int main(void) {
    pid_t pid;

    printf("Vor fork:         a = %d, b = %d\n", a, b);

    pid = fork();

    if (pid == 0) {
        /* === Kindprozess === */
        printf("Kind  (t=0s):     a = %d, b = %d\n", a, b);
        sleep(2);
        a++;  // nur im Kindprozess
        printf("Kind  (t=2s, a++): a = %d, b = %d\n", a, b);
        sleep(2);
        printf("Kind  (t=4s):     a = %d, b = %d\n", a, b);
    } else {
        /* === Elternprozess === */
        printf("Eltern (t=0s):    a = %d, b = %d\n", a, b);
        sleep(2);
        b++;  // nur im Elternprozess
        printf("Eltern (t=2s, b++): a = %d, b = %d\n", a, b);
        sleep(2);
        printf("Eltern (t=4s):    a = %d, b = %d\n", a, b);
        wait(NULL);  // Zombie verhindern
    }
    return 0;
}
```

**Erwartete Ausgabe:**
```
Vor fork:            a = 10, b = 20
Kind  (t=0s):        a = 10, b = 20
Eltern (t=0s):       a = 10, b = 20
Kind  (t=2s, a++):   a = 11, b = 20
Eltern (t=2s, b++):  a = 10, b = 21
Kind  (t=4s):        a = 11, b = 20
Eltern (t=4s):       a = 10, b = 21
```

**Erklärung:**  
`fork()` erstellt eine vollständige Kopie des Adressraums (Copy-on-Write). Eltern- und Kindprozess besitzen danach **unabhängige Kopien** von `a` und `b`. Änderungen des Kindes an `a` (→ 11) sind im Elternprozess nicht sichtbar, und umgekehrt. Es gibt kein gemeinsames Shared Memory.

---

### Zombie verhindern

Im obigen Code ist `wait(NULL)` bereits im Elternprozess eingebaut. Alternativ:

```c
// Variante 2: SIGCHLD-Handler
signal(SIGCHLD, SIG_IGN);  // Kernel räumt Zombies automatisch auf

// Variante 3: expliziter Handler
void sigchld_handler(int sig) {
    while (waitpid(-1, NULL, WNOHANG) > 0);  // alle beendeten Kinder abholen
}
signal(SIGCHLD, sigchld_handler);
```

---

## Aufgabe 4: fork() & Co für Fortgeschrittene

### Programmanalyse

**Ausgangssituation (t = 0):**  
Das ursprüngliche Programm (Prozess **P**, PID = A) führt zwei `fork()`-Aufrufe durch:

1. `ids[0] = fork()` → erzeugt **C1** (PID = B)
2. Beide (P und C1) rufen nochmals `fork()` auf:
   - P erzeugt **C2** (PID = C), in P: `ids[2] = C`
   - C1 erzeugt **C3** (PID = D), in C1: `ids[2] = D`

**Werte in den Prozessen nach den Forks:**

| Prozess | ids[0] | ids[1] | ids[2] |
|---------|--------|--------|--------|
| P       | B (>0) | A      | C (>0) |
| C1      | 0      | B      | D (>0) |
| C2      | B (>0) | A      | 0      |
| C3      | 0      | B      | 0      |

**Welchen Zweig betritt jeder Prozess?**

| Prozess | Bedingung erfüllt               | Zweig           |
|---------|---------------------------------|-----------------|
| P       | `ids[1] == getpid()` (A == A)   | **3. Zweig**    |
| C1      | `ids[1] == getpid()` (B == B)   | **3. Zweig**    |
| C2      | `ids[1] == getppid()` (A == A)  | **2. Zweig**    |
| C3      | `ids[0] == ids[2]` (0 == 0)     | **1. Zweig**    |

---

### Zeitlicher Ablauf

```
t=0   P:  schläft 1s (3. Zweig)
      C1: schläft 1s (3. Zweig)
      C2: schläft 3s (2. Zweig)
      C3: values[1]++ → [0,1,0], schläft 4s (1. Zweig)

t=1   P:  values[2]++ → [0,0,1]
          kill(C2, SIGUSR1)  →  C2-Handler: values[0]++ → [1,0,0]
          C2: sleep(3) wird unterbrochen → values[2]++ → [1,0,1], schläft noch 2s
          P:  wait(NULL)  [wartet auf C2]

      C1: values[2]++ → [0,0,1]
          kill(C3, SIGUSR1)  →  C3-Handler: values[0]++ → [1,1,0]
          C3: sleep(4) wird unterbrochen → fällt durch zum printf
          C1: wait(NULL)  [wartet auf C3]

t≈1   C3 gibt aus und endet:   printf → "D B | 1 1 0"
      C1: wait() kehrt zurück, ids[0]==0 → kein 2. wait
          C1 gibt aus und endet: printf → "B A | 0 0 1"

t=3   C2: wacht auf, gibt aus und endet: printf → "C A | 1 0 1"
          P: wait() (1. wait) kehrt zurück → ids[0]=B>0 → values[1]++ → [0,1,1]
          P: wait() (2. wait) → C1 schon beendet, kehrt sofort zurück
          P  gibt aus: printf → "A 99 | 0 1 1"
```

---

### Ausgabe des Programms (mit symbolischen PIDs)

| Prozess | Zeit t | PID | PPID | values[0] | values[1] | values[2] |
|---------|--------|-----|------|-----------|-----------|-----------|
| C3      | ≈ 1 s  | D   | B    | 1         | 1         | 0         |
| C1      | ≈ 1 s  | B   | A    | 0         | 0         | 1         |
| C2      | 3 s    | C   | A    | 1         | 0         | 1         |
| P       | 3 s    | A   | 99*  | 0         | 1         | 1         |

*\* PPID von P = PID der aufrufenden Shell*

**Konkrete Beispielausgabe** (mit fiktiven PIDs A=100, B=101, C=102, D=103, Shell=99):
```
103 101 | 1 1 0
101 100 | 0 0 1
102 100 | 1 0 1
100 99  | 0 1 1
```

---

### Erklärung der values-Werte

- **C3:** SIGUSR1 löst `values[0]++` aus (→1). `values[1]++` im 1. Zweig (→1). `values[2]` bleibt 0.
- **C1:** Eigene Kopie: `values[2]++` im 3. Zweig (→1). `values[0]` und `values[1]` unberührt.
- **C2:** SIGUSR1 löst `values[0]++` aus (→1). `values[2]++` im 2. Zweig (→1). `values[1]` = 0.
- **P:** `values[2]++` im 3. Zweig (→1). `values[1]++` nach dem 1. `wait()` (→1). `values[0]` = 0 (P selbst erhält kein SIGUSR1).
