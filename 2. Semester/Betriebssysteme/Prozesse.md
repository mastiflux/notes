
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



