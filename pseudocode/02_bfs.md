# 2. Breadth First Search (BFS)

## Idea

Usa una **coda FIFO**: espande i nodi livello per livello.
Prima tutti con 1 aminoacido piazzato, poi 2, poi 3, ecc.

- **Completo:** Sì — trova sempre una soluzione se esiste
- **Ottimo:** Sì rispetto al numero di passi (tutti i cammini hanno costo uniforme)
- **Memoria:** O(b^d) — deve tenere in memoria tutti i nodi del livello corrente
- **Tempo:** O(b^d)

---

## Pseudo-codice

```
FUNZIONE BFS(sequenza):
  stato_iniziale = {posizioni: [(0,0)], indice: 0, sequenza: sequenza}

  coda = [ stato_iniziale ]        // struttura FIFO
  migliore_soluzione = NULL
  miglior_punteggio  = -1

  MENTRE coda non è vuota:
    stato = coda.dequeue()         // prendi il nodo in TESTA alla coda (il più vecchio)

    SE è_completo(stato):
      // Tutti gli aminoacidi piazzati: valuta la soluzione
      punteggio = conta_contatti_HH(stato)
      SE punteggio > miglior_punteggio:
        miglior_punteggio  = punteggio
        migliore_soluzione = stato
      CONTINUA                     // non espandere oltre questo nodo

    // Espandi: aggiungi i successori in FONDO alla coda
    PER OGNI s IN successori(stato):
      coda.enqueue(s)              // FIFO → verrà esplorato dopo tutti quelli già in coda

  RETURN migliore_soluzione

// DIFFERENZA con DFS: coda invece di pila.
// BFS esplora per "onde" concentriche di profondità crescente.
// Svantaggio: tiene in memoria TUTTI i nodi di un livello → molto memoria.
```

---

## Confronto BFS vs DFS

```
Albero con b=2, d=3:

BFS esplora: A → B,C → D,E,F,G → H,I,J,K,L,M,N,O
             (livello per livello)

DFS esplora: A → B → D → H → I → E → J → K → C → F → L → ...
             (in profondità prima)
```
