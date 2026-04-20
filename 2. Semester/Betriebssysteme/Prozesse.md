
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
