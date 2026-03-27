# 4. Iterative Deepening Search (IDS)

## Idea

Ripete DFS con un **limite di profondità crescente**: 0, 1, 2, 3, ...
Combina i vantaggi di BFS (completo, ottimo) con quelli di DFS (poca memoria).

- **Completo:** Sì — appena lim = d trova la soluzione
- **Ottimo:** Sì rispetto al numero di passi (se costi uniformi)
- **Memoria:** O(b·d) — come DFS, solo il cammino corrente
- **Tempo:** O(b^d) — lo spreco per riesplorazione è piccolo (~11% con b=10)

---

## Pseudo-codice

```
FUNZIONE IDS(sequenza):
  // Prova limiti crescenti da 0 fino alla lunghezza massima possibile
  PER lim = 0, 1, 2, ..., lunghezza(sequenza) - 1:

    risultato = DFS_limitato(sequenza, lim)

    SE risultato != NULL:
      RETURN risultato             // trovata soluzione al limite lim = d

  RETURN NULL                     // nessuna soluzione esiste


FUNZIONE DFS_limitato(sequenza, limite):
  // DFS classico ma si ferma alla profondità = limite
  stato_iniziale = {posizioni: [(0,0)], indice: 0, sequenza: sequenza}

  pila = [ (stato_iniziale, profondità=0) ]
  migliore_soluzione = NULL
  miglior_punteggio  = -1

  MENTRE pila non è vuota:
    (stato, prof) = pila.pop()

    SE è_completo(stato):
      punteggio = conta_contatti_HH(stato)
      SE punteggio > miglior_punteggio:
        miglior_punteggio  = punteggio
        migliore_soluzione = stato
      CONTINUA

    SE prof < limite:              // ← UNICA DIFFERENZA rispetto a DFS normale
      PER OGNI s IN successori(stato):
        pila.push( (s, prof + 1) )

    // Se prof == limite: non espandere (nodo "tagliato")

  RETURN migliore_soluzione


// PERCHÉ FUNZIONA?
// Al limite lim=0: esplora solo la radice
// Al limite lim=1: esplora radice + profondità 1
// ...
// Al limite lim=d: esplora fino alla soluzione → la trova!
//
// I nodi ai livelli superiori vengono riesplorati più volte,
// ma siccome la maggior parte dei nodi è all'ultimo livello,
// lo spreco totale è piccolo (vedi analisi complessità).
```

---

## Analisi complessità temporale

```
Con fattore di ramificazione b=10 e soluzione a profondità d=5:

BFS:  10 + 100 + 1.000 + 10.000 + 100.000 = 111.110 nodi

IDS:  50 + 400 + 3.000 + 20.000 + 100.000 = 123.450 nodi → solo +11% !

Motivo: la maggior parte dei nodi (100.000 su 123.450) è all'ultimo livello,
        e viene esplorata una sola volta.
```
