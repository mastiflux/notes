# Praktikum 03: Geräte und Systemprogrammierung in C

Die Aufgaben sind als **Hausarbeit vorzubereiten**. Bitte notieren Sie Ihre Kommandos, Ausgaben und Antworten.

---

### 1. Gerätetypen im /dev-Verzeichnis

a) Listen Sie mit `ls -l /dev` die Gerätedateien auf. Identifizieren Sie jeweils ein konkretes Beispiel für ein **blockorientiertes** und ein **zeichenorientiertes** Gerät. Woran erkennen Sie den Typ?

Befehl: ls -l /dev

Blockgerät (Beispiel): brw-rw----   1 root disk    259,     0 Mar 17 08:25 nvme0n1

Zeichengerät (Beispiel): crw-r--r--   1 root root     10,   235 Mar 17 08:25 autofs

Erkennungsmerkmal: b oder c am Anfang

---

b) Was passiert, wenn Sie `echo "Hallo" > /dev/null` ausführen? Was passiert bei `cat /dev/null`? Erklären Sie.

Ausgabe/Beobachtung: keine Ausgabe

Erklärung: /dev/null entfernt jeglichen output

<div style="page-break-after: always;"></div>

---

### 2. Virtuelle Dateien in /proc und /sys

a) Geben Sie den Inhalt von `/proc/cpuinfo` aus. Wie viele CPU-Kerne hat Ihr System, und wie heißt das CPU-Modell?

Befehl: 'cd/proc' dann 'cat cpuinfo'

Anzahl Kerne: 6 

CPU-Modell: AMD Ryzen 5 5500U with Radeon Graphics

---

b) Ermitteln Sie mit Hilfe von `/proc/meminfo`, wie viel Gesamtspeicher (MemTotal) und wie viel freier Speicher (MemFree) Ihrem System zur Verfügung stehen. Geben Sie das Kommando `free` ein und vergleichen Sie.

Befehl: in /proc: 'cat meminfo'

MemTotal: MemTotal:        7429872 kB

MemFree: MemFree:         2559096 kB

free: Mem:         total 7429872    used 2810236    free 2564236      shared 17908    buff/cache 2374456    available 4619636

---

c) Vergleichen Sie die Ausgabe von `cat /proc/version` mit der Ausgabe von `uname -a`. Was fällt auf?
in /proc
Befehl 1: 'cat version'

Befehl 2: 'uname -a'

Vergleich: version: Linux version 6.17.0-19-generic (buildd@lcy02-amd64-019) (x86_64-linux-gnu-gcc-13 (Ubuntu 13.3.0-6ubuntu2~24.04.1) 13.3.0, GNU ld (GNU Binutils for Ubuntu) 2.42) #19~24.04.2-Ubuntu SMP PREEMPT_DYNAMIC Fri Mar  6 23:08:46 UTC 2                                                                                                  uname: Linux fhaese-HP-Laptop-15s-eq2xxx 6.17.0-19-generic #19~24.04.2-Ubuntu SMP PREEMPT_DYNAMIC Fri Mar  6 23:08:46 UTC 2 x86_64 x86_64 x86_64 GNU/Linux --> uname hat deutlich merh infos, wie z.B. den build


<div style="page-break-after: always;"></div>

---

### 3. Formatierung mit printf()

Erstellen Sie die Datei `format.c` mit folgendem Inhalt, kompilieren und führen Sie das Programm aus:

```c
#include <stdio.h>

int main(void) {
    int semester = 2;
    double pi = 3.14159;
    char note = 'B';

    printf("Semester: %f\n", semester);
    printf("Pi: %d\n", pi);
    printf("Pi (2 Stellen): %.2f\n", pi);
    printf("Note: %c\n", note);
    printf("Alles zusammen: Sem %d, Pi=%.4f, Note=%c\n",
           semester, pi, note);
    return 0;
}
```

a) Was ist die Ausgabe des Programms?

Ausgabe: 
Semester: 2
Pi: 3.141590
Pi (2 Stellen): 3.14
Note: B
Alles zusammen: Sem 2, Pi=3.1416, Note=B


---

b) Was passiert, wenn Sie `%d` statt `%f` für die Variable `pi` verwenden? Was passiert dann? Erklären Sie, warum C diesen Fehler nicht als Compilerfehler meldet (ggf. `-Wall` weglassen).

Ausgabe: 
format.c: In function ‘main’:
format.c:9:18: warning: format ‘%d’ expects argument of type ‘int’, but argument 2 has type ‘double’ [-Wformat=]
    9 |     printf("Pi: %d\n", pi);
      |                 ~^     ~~
      |                  |     |
      |                  int   double
      |                 %f


Erklärung: printf kann nur ganze Zahlen ausgeben, mit %d übergibt man allerdings ein double, also eine Kommazahl

<div style="page-break-after: always;"></div>

---

c) Vervollständigen Sie das folgende C-Fragment so, dass es die angegebene Ausgabe erzeugt. Testen Sie das Programm.

```c
#include <stdio.h>

// Ausgabe: NCC-1701-D Warp 8.0 Captain Jean Luc Picard

char capt_firstname[] = "Jean Luc";
char capt_name[] = "Picard";
int ship_serial = 1701;
char ship_mod = 'D';
float max_speed = 8.0f;

int main() {
  printf("NCC-%d-%c Warp %.1f Captain %s %s\n",
         ship_serial, ship_mod, max_speed,
         capt_firstname, capt_name);
  return 0;
}
```

<div style="page-break-after: always;"></div>

---


### 4. Call by Reference

Vervollständigen Sie das folgende Programm:

```c
#include <stdio.h>
#include <stdlib.h>

void quadrat(float *zahl) {
  // Hier fehlt was
}

int main() {
  float seitenlaenge = 3.7;
  // Hier fehlt was
}
```

Kompilieren Sie das Programm mit `cc -Wall quadrat.c -o quadrat` und führen Sie es aus. Vervollständigen und testen Sie das Programm.

Vollständiges Programm:

#include <stdio.h>
#include <stdlib.h>
```c
void quadrat(float *zahl) {
  *zahl = (*zahl) * (*zahl);
}

int main() {
  float seitenlaenge = 3.7;
  quadrat(&seitenlaenge);
  printf("Quadrat: %.2f\n", seitenlaenge);
  return 0;
}
```
Test: Quadrat: 13.69

Wie funktioniert die Parameterübergabe?
zahl ist Zeiger auf float Zahl --> funktion bekommt addresse der zahl
&seitenlänge gibt speicheradresse der variable zurück (nicht variable selbst)
*zahl gibt wert an der adresse zahl zurück
quadrat ändert wert an adresse zahl, also den ursprünglichen wert und keine kopie

<div style="page-break-after: always;"></div>

---

## Hilfestellung

### Wichtige Header-Dateien

| Header       | Enthält                                                |
|--------------|--------------------------------------------------------|
| `<stdio.h>`  | `printf`, `perror`                                     |
| `<fcntl.h>`  | `open`, Flags (`O_RDONLY`, `O_WRONLY`, `O_CREAT`, ...) |
| `<unistd.h>` | `read`, `write`, `close`, `STDOUT_FILENO`              |

### Kompilieren und Ausführen

```bash
cc -Wall programm.c -o programm
./programm
```
