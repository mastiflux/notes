# Prozesse

**Ein Prozess ist ein Programm (ausführbare Datei) in Ausführung.**

- haben eigenen Bereich im RAM
- wird ein Teil der verfügbaren CPU-Zeit zugeordnet (-> eigener Kontrollfluss)

---

**Speicherabbild eines Prozesses**
![[Pasted image 20260420142413.png]]


---

## Prozessverwaltung

- interaktive Prozesse (z.B. Maus, Tastatur) vs. Hintergrundprozesse (*Daemon*)
- Quasiparallelität durch schnelle Umschaltung der CPU zwischen Prozessen
- zur Verwaltung: *Prozesskontrollblock* (PCB) <- eigene Datenstruktur

**Prozesskontrollblock**

- Identifikator (Process-ID, PID)
- Zustand (bereit, laufen, blockiert)
- Priorität
- Programmzähler
- Kontext 
- Speicherabbild
- I/O Zustandsinformationen (bspw. zugeordnete Geräte)
- Allgemeine Verwaltungsinformationen (bspw. Ressourcen)

--- 

**Prozesse unter Linux anzeigen**

```bash
ps -eo user,pid,ppid,stat,tty,time,cmd | grep -v "\[" # nur ein Moment
top # Echtzeit mit Updates
```

- üblicherweise ein *init* Prozess mit PID 1

## Zustände

### Zustandsdiagramm

![[Pasted image 20260420145231.png]]

- genaues Diagramm abhängig vom Betriebssystem
- JVM hat eigenes Zustandsdiagramm
- *ps* zeigt Zustand in Spalte *stat* an

---

**Zustand: Blockiert**

- Systemaufrufe können Prozess blockieren (Zustand *Blocked*)
- die meisten Prozesse sind fast immer blockiert (bspw. durch Warten auf Eingabe eines Gerätes oder auf das Ergebnis eines anderen Prozesses)

-> Beispielablauf *read()*
1. Prozess P ist im Zustand *Running* und ruft *read()* auf
2. Kernel-Funktion *read* fordert Daten bei Disk-Controller an und kennzeichnet Prozess als *Blocked*
3. System führt andere Prozesse aus
4. Disk-Controller erzeugt Interrupt, wenn Daten bereit
5. Interrupt-Serviceroutine (ISR) im Kernel weckt P, Zustandswechsel nach *Ready*
6. System kann P wieder ausführen

---

**Wie häufig blockieren Prozesse - eher Ready oder Blocked?**

![[Pasted image 20260420150943.png]]

- Wahrscheinlichkeit dafür, dass alle *n* Prozesse blockieren: $p^n$
- CPU-Ausnutzung ist dann $1-p^n$ 

---

**Warteschlangen**

- zur effizienten Prozessverwaltung
- je eine separate Block-Queue pro I/O-Event

![[Pasted image 20260420151417.png]]

## Dispatching

=> Prozessumschaltung

**Ablauf**
1. Prozesskontext einschließlich PC und aller Register (z.B. im PCB) sichern 
2. Aktualisieren des PCB des aktiven Prozesses (z. B. State Running ➔ Blocked, warten auf Festplatte) 
3. Verschieben des PCB (oder der PID) in die entsprechende Warteschlange (z. B. Blocked Queue) 
4. Scheduling-Entscheidung: Auswahl eines anderen Prozesses für die Ausführung 
5. Aktualisieren des PCB des neuen Prozesses (State ➔ Running) 
6. Speicherverwaltungsstrukturen aktualisieren (➔ VL Speicherverwaltung) 
7. Prozesskontext des neuen Prozesses wieder herstellen

![[Pasted image 20260420151932.png]]

---

**Gantt-Diagramm mit Zuständen der Prozesse über die Zeit**

![[Pasted image 20260420152141.png]]

- Vorgang relativ teuer
- Zeit für Dispatching oft vernachlässigt (wird relativ selten gemacht)


## Prozesse erzeugen, beenden und unterbrechen

### Erzeugung

- _Prozessverkettung_: der laufende Prozess startet einen neuen Prozess und terminiert sich damit selbst
- _Prozessvergabelung_: der laufende Prozess startet einen neuen Prozess, beide Prozesse laufen danach weiter
- _Prozesserzeugung_: der laufende Prozess startet einen unabhängigen neuen Prozess (ähnlich der Prozessvergabelung)

---

**Prozesserzeugung mit _fork()_**

```c
#include <unistd.h> 
#include <stdio.h> 
int main(void) { 
	printf("Spass mit fork()\n"); 
	pid_t ret = fork(); 
	
	if (ret < 0) { 
		perror("Fehler in fork()"); 
		return 1; 
	} else if (ret == 0) { 
		printf("Hallo vom Kindprozess\n"); 
	} else { 
		printf("Elternprozess von Kindprozess %d\n", ret); 
	} 
	printf("Prozess beenden\n"); 
	return 0; 
}
```

- _fork()_ kehrt in beiden Prozessen mit unterschiedlichen Rückgabewerten zurück
- erzeugt identische Kopie vom Ursprungsprozess
	- aufrufender Prozess heißt Elternprozess
	- neu erzeugter Prozess heißt Kindprozess
- Code und Variablen werden vererbt
- Kindprozess nutzt meist zunächst den gleichen physikalische Speicher
- Betriebssystem erzeugt erst bei Schreibzugriff echte Kopie (_copy-on-write_)

---
### Terminierung

- Prozesszusammenführung zum beenden
	- Prozessvereinigung (_join_)
	- Prozesstreffen (_wait_)

![[Pasted image 20260420154307.png]]

```c
#include <unistd.h> 

void _exit(int status); 
```

- beenden der _main()_-Funktion ruft automatisch _exit()_ auf

-> Prozess blockieren, bis ein Kind den Status ändert (endet / angehalten wird):
```c
#include <sys/wait.h> 

pid_t wait(int *status); 
```

---
### Unterbrechung

**Signale**
-> asynchrone Benachrichtigungen an einen Prozess
- je eine Nummer (und zugeordneten Namen)

Signale senden mit:
```c
#include <signal.h> 

int kill(pid_t pid, int sig); // 0=ok 
```

![[Pasted image 20260420154826.png]]

---
### Threads

- leichtgewichtige Alternative zu Prozessen
- Ausführungspfad innerhalb eines Prozesses
- keine eigenen Ressourcen
- enthält Elemente: 
	- Zustand
	- Kontext
	- Stack
	- Speciherplatz für lokale Variablen
	- Zugriff auf gemeinsame Ressourcen innerhalb eines Prozesses

```c
#include <pthread.h> 

int pthread_create(pthread_t *thread, const pthread_attr_t *attr, void *(*start_routine)(void *), void *arg);
```

```cpp
#include <thread>

Thread myThread(myfunc, arg1, arg2, ...);
//...
// wait for thread to finish (blocking)
myThread.join();
```
---
## Prozessorzuteilung (Scheduling)

- ==Scheduler== wählt Prozess aus, der als nächstes laufen soll -> ist Teil des Betriebssystems
- Umschlatung durch Dispatcher
- trifft Entscheidungen nur für je einen Prozessorkern
- viele vers. Algortihmen und Strategien -> kommt auf Einsatzgebiet an
	Kategorien von Strategien:
	*1. Stapelverarbeitung:* Gehaltsabrechnungen, Zinsgutschriften, KI-Lernen (keine Interaktion mit Benutzern, eher Server)
	*2. Interaktive Systeme:* Textverarbeitung, IDE (betrifft auch Serveranwendungen, z.B. Buchungssysteme)
	*3. Echtzeitsysteme:* Steuerung Industrieanlage, Autopilot

---
**Kriterien und Ziele:**

- Fairness (jeder Prozess bekommt fairen Anteil der CPU-Zeit)
- Balance (alles ausgelastet)
- Durchsatz (max. Jobs pro Zeiteinheit)
- Durchlaufzeit (min. Zeit von Start bis Ende)
- CPU-Ausnutzung
- Antwortzeit
- Proportionalität (Erwartung Benutzer an Aufwand)
- Deadlines
- Vorhersagbarkeit

---
**Wann schedulen?**

- Erzeugung Kindprozess
- Terminierung Prozess
- Blockierender Systemaufruf
- Aufhebung der Blockierung eines Prozesses
- Timerinterrupt

---
**Varianten:**

==Kooperative Prozessorvergabe==

- Umschaltung nur, wenn Prozess freiwillig Prozessor abgibt oder terminiert
- CPU-intensiver Prozess kann ewig laufen, wenn er nicht kooperiert
- Reaktion anderer Prozesse auf externe Ereignisse verspätet

==Präemptive Prozessorvergabe==

- Umschaltung durch Interrupts
- wenig Beeinflussung der Prozesse untereinander
- Reaktionszeit auf externe Ereignisse kurz

---
### Prozessoraffinität auf Multicore-Systemen

**Problem**

- schnellste CPU-Caches sind nur lokal für einen einzigen Kern
- bei Scheduling desselben Prozesses auf einem neuen Kern -> keine relevanten Daten im Cache -> Cache Miss kostet viel Zeit

**Lösung**

- implzite Prozessoraffinität (Kernel versucht einem Kern immer den gleichen Prozess zuzuordnen)
- Linux unterstützt auch explizite Prozessoraffinität (erfordert root-Rechte)

---
### Batchverarbeitung: FCFS, SPN, SRTF, RR
#### First Come First Serve (FCFS)

- rechenbereite Prozesse werden in Wartschlange eingeordnet und gemäß Ankunftszeit abgearbeitet
- wenn Prozess blockiert -> wenn wieder rechenbereit, neu am Ende einsortiert
- sehr einfach
- hohe Durchlaufzeit bei I/O-intensiven Prozessen

---
#### Shortest Process Next (SPN)

- Prozesse werden nach ursprünglich geplanter Laufzeit sortiert und abgearbeitet
- bevorzugt kurze Prozesse
- meist nicht präemptiv (SRTF als präemptive Variante, *Shortest Remaining Time First*)

---
#### Round Robin (RR)

- jeder Prozess darf nur gewisse Zeit Z laufen, wird dann unterbrochen und ans Ende der Warteschlange gestellt
- Timeslice: feste Zeitspanne Z (Quantum), typisch 1-100 ms -> wenn zu kurz -> hoher Dispatching-Overhead, wenn zu lang -> ruckelt

![[Pasted image 20260427150638.png]]

---
### Prioritätsbasiertes Scheduling

**statische Prioritäten**

- jeder Prozess hat eindeutige Priorität
- Scheduler wählt immer Prozess mit höchster Priorität
- mögliches Problem: Starvation

**Multi-Level Scheduling(ML)**

- jedem Prozess wird Priorität zugeordnet
- erst alle Prozesse eine Stufe durchlaufen, dann nächste Prioriätsebene

**Multi-Level Feedback**

- Prozess wird je nach bisheriger Laufzeit priorisiert
- neuer Prozess -> höchste Priorität, dann schrittweise herabgestuft
- präemptiv mit festem Zeitintervall

---
### Klassischer Unix-Scheduler

- Multilevel Feedback, präemptiv mit festem Zeitintervall für Repriorisierung
- Priorität wird abhängig z.B. von seiner verbrauchten CPU-Zeit periodisch berechnet

$T_{CPU}(i)=(T_{CPU}(i-1))\div2\,;\,P(i)=P_{base}+P_{nice}+(T_{CPU}(i))\div2$

$P$: Priorität
$P_{base}$: Basispriorität
$P_{nice}$: Nice-Level 
$T_{CPU}$: CPU-Zeit
$i$: Nummer des Scheduling-Intervalls

Beispiel siehe [[BS-06V-Prozesse.pdf]] 

**Typische Umsetzung**

![[Pasted image 20260427152818.png]]

---
### Metriken und Vergleich

- Prozesse werden beschrieben durch
	- $T_a$ Arrival Time: Zeitpunkt, wo Prozess erstmalig bereit wird
	- $T_s$ Service Time: Rechenzeit
	- $T_c$ Completion Time: Zeitpunkt, zu dem der Prozess beendet wird
	- $T_r$ Turnaround Time: $T_c - T_a$ (normalisiert: $T_r\div T_s$)

---
# Interprozesskommunikation

## Nebenläufigkeit und kritische Abschnitte

**Nebenläufigkeit** bezeichnet parallele oder quasi-parallele Ausführung von Programmen.

- keine echte Parallelität nötig -> präemptives Umschalten reicht aus
- realisiert durch mehrere Prozesse und/oder Threads
- man muss mit jeder möglichen Verzahnung der Ausführungsstränge rechnen
---
*Beispiel:*

![[Pasted image 20260504142554.png]]

![[Pasted image 20260504142617.png]]

- Abschnitt in Thread A vollständig durchlaufen, dann B
-> counter = 2
---
![[Pasted image 20260504142636.png]]

- Abschnitt nach Anweisung 2 unterbrochen
-> counter = 1 ==Lost Update==
---

### Atomare Anweisungen

- unteilbare Anweisung
- können nicht unterbrochen werden
- meiste Assembler-Instruktionen
- meiste Hochsprachen verbinden mehrere Assembler Befehle -> nicht atomare Anweisungen

---
**Folge von Anweisungen atomar machen?**

- möglich durch globales Sperren und wieder Freischalten von Interrupts (schlecht)
- ==Mutual Exclusion== (besser):
	- es darf sich immer nur höchstens ein Thread/Prozess in einem kritischen Abschnitt bzgl. einer Ressource befinden
	- andere Programmteile unproblematisch
	- simuliert atomare Anweisungsfolge! 

*Beispiel:*

![[Pasted image 20260504143829.png]]

---
*Grundsätze:*

1. Mutual exclusion: Zwei oder mehr Prozesse dürfen sich nicht gleichzeitig im gleichen kritischen Abschnitt befinden.

2. Es dürfen keine Annahmen über die Abarbeitungsgeschwindigkeit und die Anzahl der Prozesse bzw. CPUs gemacht werden. Der kritische Abschnitt muss unabhängig davon geschützt werden.
 
3. Kein Prozess außerhalb eines kritischen Abschnitts darf einen anderen nebenläufigen Prozess blockieren.
 
4. Fairness Condition: Jeder Prozess, der am Eingang eines kritischen Abschnitts wartet, muss ihn irgendwann betreten dürfen. (kein ewiges Warten/Verhungern/Starvation
---
### Realisierung kritischer Abschnitte

- mit Sperrvariablen (*Lock*)
	- Lock = 1 -> Prozess im kritischen Abschnitt
	- Lock = 0 -> kein Prozess im kritischen Abschnit
	- wenn kritischer Abschnitt blockiert: Aktives Warten mit Spinlock (fragt ständig ab, braucht CPU-Zeit)
- Prozesszustand *Blockiert* zum Warten nutzen -> relativ aufwendig, aber meist besser
---

- Abfragen und Setzen der Sperrvariablen muss atomar sein
- spezielle Assembler Instruktionen wie *TSL* 
	- liest Inhalt der Sperrvariablen
	- schreibt einen Wert ungleich 0 in die Sperrvariable

*Implementierung mit Spinlock*
```ASM
//Lock:

TSL R1, LOCK
CMP R1, #0
JNE Lock
RET

//Unlock:

MOVE LOCK, #0
RET
```

---
### Hauptprobleme Nebenläufigkeit

- Nebenläufigkeit beherrschen schwierig
- schwer zu reproduzieren und zu debuggen

**Fehlerklassen**

1. ==Race Condition:== Situation, in denen zwei oder mehr Tasks eine gemeinsame Ressource nutzen und das Endergebnis von der (generell unbekannten) zeitlichen Abfolge der Operationen abhängt 

2. ==Deadlock (Verklemmung):== Zwei oder mehr Tasks warten wechselseitig aufeinander; Gegenseitige Blockade, weil beide Tasks Ressourcen besitzen, die von der jeweils anderen Task benötigt werden 

3. ==Starvation (Verhungern):== Rechenbereite Task wird vom Scheduler konsequent übergangen bei unfairem Synchronisationsmechanismus
---
## Semaphoren

- Absicherung von Ressourcen und Synchronisation von Prozessen
- durch Betriebssystem realisierte Zählvariable
- unterstützt zwei atomare Operationen P() und V()
	- P()/down()/acquire(): Wenn Zähler > 0: Zähler dekrementieren, sonst Prozess blockiert, in Warteschlange einreihen
	- V()/up()/release(): Zähler inkrementieren und ggf. Prozess aus Warteschlange wecken -> bereit

*Typen:*

- binäre Semaphor: nur 0 und 1 erlaubt, ideal für kritische Abschnitte
- Zählende Semaphor: positive Zählwerte erlaubt, ermöglicht auch komplexere Use Cases

==! sehr fehleranfällig !==
siehe Fehler in:[[BS-07V-IPC.pdf]] 

---


