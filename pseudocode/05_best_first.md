# 5. Best-First Greedy Search

## Idea

Usa una **coda con priorità** ordinata solo per **h(n)** = euristica.
Espande sempre il nodo che sembra più promettente secondo l'euristica,
ignorando completamente il costo reale g(n).

- **Completo:** No — può perdersi in percorsi sbagliati
- **Ottimo:** No — segue l'euristica, non garantisce la soluzione migliore
- **Memoria:** O(b^d)
- **Tempo:** O(b^d) nel caso peggiore, ma spesso molto più veloce in pratica

---

## Euristica h(n)

```
// Stima del numero di contatti H-H ancora raggiungibili

FUNZIONE h(stato):
  // Conta gli H ancora da piazzare nella sequenza rimanente
  H_rimanenti = 0
  PER i = stato.indice + 1 FINO A lunghezza(stato.sequenza) - 1:
    SE stato.sequenza[i] == 'H':
      H_rimanenti = H_rimanenti + 1
  RETURN H_rimanenti

// Questa euristica è AMMISSIBILE:
// ogni H rimanente può formare al massimo alcuni contatti,
// ma non li sovrastima in modo assurdo.
// È una stima ottimistica → va bene per A* (vedi file 06).
```

---

## Pseudo-codice

```
FUNZIONE BestFirstGreedy(sequenza):
  stato_iniziale = {posizioni: [(0,0)], indice: 0, sequenza: sequenza}

  frontiera = coda_priorità_minima()
  // Priorità = -h(stato) perché vogliamo MASSIMIZZARE i contatti
  // (min-heap con valori negativi → estrae quello con h maggiore)
  frontiera.inserisci(stato_iniziale, priorità = -h(stato_iniziale))

  migliore_soluzione = NULL
  miglior_punteggio  = -1

  MENTRE frontiera non è vuota:
    stato = frontiera.estrai_minimo()   // stato con h(n) più ALTO

    SE è_completo(stato):
      punteggio = conta_contatti_HH(stato)
      SE punteggio > miglior_punteggio:
        miglior_punteggio  = punteggio
        migliore_soluzione = stato
      CONTINUA

    PER OGNI s IN successori(stato):
      frontiera.inserisci(s, priorità = -h(s))

  RETURN migliore_soluzione

// DIFFERENZA con A*: usa solo h(n), ignora g(n).
// Più veloce di A* ma non garantisce ottimalità.
// Può trovare una buona soluzione rapidamente su sequenze lunghe.
```

---

## Confronto Best-First vs A*

```
Best-First: f(n) = h(n)          solo euristica → veloce ma non ottimo
A*:         f(n) = g(n) + h(n)   costo reale + euristica → ottimo
```
