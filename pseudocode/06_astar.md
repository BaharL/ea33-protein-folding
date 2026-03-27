# 6. A* Search

## Idea

Combina **costo reale g(n)** e **euristica h(n)**: `f(n) = g(n) + h(n)`

- g(n) = contatti H-H già ottenuti (quello che abbiamo guadagnato finora)
- h(n) = stima dei contatti H-H ancora ottenibili (quello che speriamo di guadagnare)
- f(n) = stima del valore totale della soluzione finale

Espande sempre il nodo con **f(n) maggiore** (massimizziamo i contatti).

- **Completo:** Sì
- **Ottimo:** Sì — se h è **ammissibile** (non sovrastima mai)
- **Memoria:** O(b^d)
- **Tempo:** O(b^d) nel caso peggiore, molto meglio in pratica con buona euristica

---

## Funzioni g, h, f

```
FUNZIONE g(stato):
  // Costo reale = contatti H-H già formati nella conformazione attuale
  RETURN conta_contatti_HH(stato)

FUNZIONE h(stato):
  // Euristica ammissibile = H ancora da piazzare
  // (stesso di Best-First, vedi file 05)
  H_rimanenti = 0
  PER i = stato.indice + 1 FINO A lunghezza(stato.sequenza) - 1:
    SE stato.sequenza[i] == 'H':
      H_rimanenti = H_rimanenti + 1
  RETURN H_rimanenti

FUNZIONE f(stato):
  // Stima totale: già ottenuto + ancora ottenibile
  RETURN g(stato) + h(stato)

// AMMISSIBILITÀ di h:
// h(stato) non sovrastima mai i contatti realmente ottenibili,
// perché ogni H rimanente può formare al massimo qualche contatto
// e H_rimanenti è un upper bound ottimistico.
// → h è ammissibile → A* è ottimo.
```

---

## Pseudo-codice

```
FUNZIONE AStar(sequenza):
  stato_iniziale = {posizioni: [(0,0)], indice: 0, sequenza: sequenza}

  frontiera = coda_priorità_minima()
  // Priorità = -f(stato) perché vogliamo MASSIMIZZARE
  // (min-heap con valori negativi → estrae quello con f maggiore)
  frontiera.inserisci(stato_iniziale, priorità = -f(stato_iniziale))

  migliore_soluzione = NULL
  miglior_punteggio  = -1

  MENTRE frontiera non è vuota:
    stato = frontiera.estrai_minimo()   // stato con f(n) più ALTO

    SE è_completo(stato):
      punteggio = conta_contatti_HH(stato)
      SE punteggio > miglior_punteggio:
        miglior_punteggio  = punteggio
        migliore_soluzione = stato
      CONTINUA                          // continua: potrebbero esserci soluzioni migliori

    PER OGNI s IN successori(stato):
      frontiera.inserisci(s, priorità = -f(s))

  RETURN migliore_soluzione

// PERCHÉ È OTTIMO?
// Grazie all'ammissibilità di h, A* non scarta mai un nodo
// che potrebbe portare alla soluzione ottima.
// Espande prima i nodi più promettenti → trova l'ottimo in modo efficiente.
```

---

## Riepilogo finale di tutti gli algoritmi

| # | Algoritmo    | f(n) usata     | Completo | Ottimo | Memoria  |
|---|--------------|----------------|----------|--------|----------|
| 1 | DFS          | —              | No*      | No     | O(b·m)   |
| 2 | BFS          | —              | Sì       | Sì     | O(b^d)   |
| 3 | Min Cost     | g(n)           | Sì       | Sì     | O(b^d)   |
| 4 | IDS          | — (con limite) | Sì       | Sì     | O(b·d)   |
| 5 | Best-First   | h(n)           | No       | No     | O(b^d)   |
| 6 | A*           | g(n) + h(n)    | Sì       | Sì**   | O(b^d)   |

\* DFS completo solo su alberi finiti senza cicli
\** A* ottimo solo se h è ammissibile
