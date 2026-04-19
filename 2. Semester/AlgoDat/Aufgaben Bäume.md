**Wann erreicht der Baum eine maximale Höhe von `n-1`?**

Wenn die eingefügten Elemente bereits sortiert sind (auf- oder absteigend). Jedes neue Element ist dann immer größer (oder immer kleiner) als alle vorherigen, sodass der Baum entartet – er wird zur verketteten Liste:

```
insert: A → B → C → D

A
 \
  B
   \
    C
     \
      D   ← Höhe = n-1 = 3
```

Bei `n` Elementen braucht `get` dann im Worst Case `n` Vergleiche, da der gesamte "Ast" durchlaufen werden muss.

---

**Was müsste man tun damit die Höhe konsequent `log n` bleibt?**

Man braucht einen **selbstbalancierenden Baum**. Nach jedem `insert` oder `delete` wird die Baumstruktur durch Rotationen umgebaut sodass kein Ast unverhältnismäßig lang werden kann. Konkrete Strategien: AVL-Baum oder Rot-Schwarz-Baum.

---

**AVL vs. Rot-Schwarz – welcher wann?**

||AVL-Baum|Rot-Schwarz-Baum|
|---|---|---|
|**Balance**|strenger: Höhenunterschied zwischen linkem/rechtem Teilbaum maximal 1|lockerer: Höhe kann bis zu 2× `log n` betragen|
|**Suche**|schneller, da Baum flacher|minimal langsamer|
|**Einfügen/Löschen**|mehr Rotationen nötig|weniger Rotationen, günstiger|

→ **Primär gesucht wird:** AVL-Baum, da die striktere Balance kürzere Suchpfade garantiert.

→ **Primär eingefügt wird:** Rot-Schwarz-Baum, da weniger Rotationen pro `insert` anfallen und der Overhead geringer ist.

---

**Was passiert wenn der Comparator für ungleiche Flüge `0` zurückgibt?**

Im aktuellen Code wird bei `cmp >= 0` immer nach **rechts** gegangen (kein separater `cmp == 0`-Fall in `insert`):

```java
if (comp < 0) {
    // links
} else {          // ← greift auch bei comp == 0 !
    // rechts
}
```

Das bedeutet: ein Flug der laut Comparator gleich ist (aber `!equals()`) wird trotzdem **rechts eingefügt** – er wird also als Duplikat behandelt und landet im Baum. Der Baum wächst weiter, aber `get` gibt bei `cmp == 0` sofort den **ersten gefundenen** Knoten zurück – alle anderen Flüge mit demselben Comparator-Wert sind dann über `get` nicht mehr erreichbar. Die Datenstruktur wird damit inkonsistent: `size` steigt, aber Einträge sind faktisch verloren.