# E.A.3.3 — HP 2D-Protein Folding

**Corso:** Intelligenza Artificiale — Sapienza Università di Roma  
**Prof:** Toni Mancini  
**Esercitazione:** E.A.3.3

---

## Descrizione del Problema

Il problema **HP 2D-Protein Folding** modella una versione semplificata del protein folding.

Data una sequenza di aminoacidi su alfabeto `{H, P}` di lunghezza `n`:
- **H** = *hydrophobic* (idrofobico)
- **P** = *polar/hydrophilic* (idrofilico)

L'obiettivo è trovare una **conformazione 2D** sulla griglia discreta, cioè piazzare gli aminoacidi su coordinate intere `[-(n-1), (n-1)]`, partendo da `(0,0)`, tale che:
- il cammino sia **non-crossing** (nessuna posizione occupata due volte)
- il numero di **contatti H-H** sia **massimizzato**

Un **contatto** è una coppia di aminoacidi H non consecutivi nella sequenza, la cui distanza Euclidea sulle posizioni è esattamente 1 (su/giù/sinistra/destra — NON diagonale).

L'**energia** della conformazione è l'opposto del numero di contatti (da minimizzare).

---

## Struttura del Repository

```
ea33-protein-folding/
│
├── README.md
└── pseudocode/
    ├── 00_common.md          ← struttura comune a tutti gli algoritmi
    ├── 01_dfs.md             ← Depth First Search
    ├── 02_bfs.md             ← Breadth First Search
    ├── 03_min_cost.md        ← Min Cost Search (Uniform Cost)
    ├── 04_ids.md             ← Iterative Deepening Search
    ├── 05_best_first.md      ← Best-First Greedy Search
    └── 06_astar.md           ← A*
```

---

## Algoritmi Implementati

| # | Algoritmo | Struttura | Completo | Ottimo | Memoria |
|---|-----------|-----------|----------|--------|---------|
| 1 | DFS | Pila LIFO | No* | No | O(b·m) |
| 2 | BFS | Coda FIFO | Sì | Sì (passi) | O(b^d) |
| 3 | Min Cost | Heap su g(n) | Sì | Sì (costo) | O(b^d) |
| 4 | IDS | Pila + limite | Sì | Sì (passi) | O(b·d) |
| 5 | Best-First | Heap su h(n) | No | No | O(b^d) |
| 6 | A* | Heap su f=g+h | Sì | Sì** | O(b^d) |

\* DFS completo solo su alberi finiti senza cammini ridondanti  
\** A* ottimo solo se l'euristica h è **ammissibile** (non sovrastima)

---

## Euristica usata in Best-First e A*

```
h(stato) = numero di H ancora da piazzare nella sequenza rimanente
```

Questa euristica è **ammissibile**: ogni H rimanente può al massimo formare
un contatto, quindi non sovrastima mai i contatti ancora raggiungibili.

---

## Esempio

Sequenza: `PHHPHPPHP`  
Soluzione ottima: conformazione che massimizza i contatti H-H non consecutivi.

```
Contatto H-H = due H con distanza Euclidea = 1, NON adiacenti nella sequenza
Esempio: H[1] e H[4] sono a distanza 1 sulla griglia → contatto valido
         H[1] e H[2] sono consecutivi nella sequenza → NON conta
```
