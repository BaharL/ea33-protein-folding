# 3. Min Cost Search (Uniform Cost Search)

## Idea

Usa una **coda con priorità (min-heap)** ordinata per **g(n)** = costo reale del cammino.
Espande sempre il nodo con costo accumulato minore.

- **Completo:** Sì
- **Ottimo:** Sì rispetto al costo reale g(n)
- **Memoria:** O(b^d)
- **Tempo:** O(b^d)

> **Nota per questo problema:** tutti i passi hanno costo uniforme = 1
> (ogni aminoacido piazzato costa 1), quindi g(stato) = stato.indice.
> In questo caso Min Cost Search si comporta come BFS,
> ma la struttura con heap è diversa e più generale.

---

## Pseudo-codice

```
FUNZIONE g(stato):
  // Costo reale = numero di aminoacidi già piazzati = profondità nel albero
  RETURN stato.indice

FUNZIONE MinCostSearch(sequenza):
  stato_iniziale = {posizioni: [(0,0)], indice: 0, sequenza: sequenza}

  frontiera = coda_priorità_minima()
  frontiera.inserisci(stato_iniziale, priorità = g(stato_iniziale))  // = 0

  migliore_soluzione = NULL
  miglior_punteggio  = -1

  MENTRE frontiera non è vuota:
    stato = frontiera.estrai_minimo()   // estrae il nodo col g(n) più basso

    SE è_completo(stato):
      punteggio = conta_contatti_HH(stato)
      SE punteggio > miglior_punteggio:
        miglior_punteggio  = punteggio
        migliore_soluzione = stato
      CONTINUA

    PER OGNI s IN successori(stato):
      frontiera.inserisci(s, priorità = g(s))   // g(s) = stato.indice + 1

  RETURN migliore_soluzione

// DIFFERENZA con BFS: usa heap ordinato per g(n) invece di coda FIFO.
// Utile quando i passi hanno costi diversi (es. versione con pesi sugli archi).
// In questo problema costi uniformi → risultato identico a BFS.
```

---

## Quando Min Cost è diverso da BFS?

```
Se i passi avessero costi diversi, es:
  - piazzare H costa 1
  - piazzare P costa 2

Allora Min Cost espanderebbe prima i cammini con meno P piazzati,
mentre BFS espanderebbe comunque per profondità.

Min Cost garantisce ottimalità sul COSTO TOTALE.
BFS garantisce ottimalità sul NUMERO DI PASSI.
```
