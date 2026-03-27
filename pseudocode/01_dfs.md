# 1. Depth First Search (DFS)

## Idea

Usa una **pila LIFO**: espande sempre il nodo più recente,
cioè va in profondità prima di tornare indietro (backtrack).

- **Completo:** No — su alberi infiniti o con cicli si perde
- **Ottimo:** No — trova la prima soluzione completa, non necessariamente la migliore
- **Memoria:** O(b·m) — salva solo il cammino corrente + fratelli non esplorati
- **Tempo:** O(b^m)

---

## Pseudo-codice

```
FUNZIONE DFS(sequenza):
  stato_iniziale = {posizioni: [(0,0)], indice: 0, sequenza: sequenza}

  pila = [ stato_iniziale ]        // struttura LIFO
  migliore_soluzione = NULL
  miglior_punteggio  = -1

  MENTRE pila non è vuota:
    stato = pila.pop()             // prendi il nodo in CIMA alla pila

    SE è_completo(stato):
      // Tutti gli aminoacidi piazzati: valuta la soluzione
      punteggio = conta_contatti_HH(stato)
      SE punteggio > miglior_punteggio:
        miglior_punteggio  = punteggio
        migliore_soluzione = stato
      CONTINUA                     // non espandere oltre questo nodo

    // Espandi: aggiungi i successori in cima alla pila
    PER OGNI s IN successori(stato):
      pila.push(s)                 // LIFO → il prossimo nodo sarà s

  RETURN migliore_soluzione

// NOTA: DFS esplora tutto l'albero cercando la soluzione con più contatti H-H.
// Non si ferma alla prima soluzione completa: continua per trovare la migliore.
```

---

## Esempio di esecuzione (sequenza "HPH")

```
Pila iniziale: [ A=(0,0) ]

Step 1: pop A → non completo → successori: B=(0,1), C=(1,0), D=(0,-1), E=(-1,0)
        pila: [ B, C, D, E ]

Step 2: pop E=(-1,0) → non completo → successori: ...
        → continua in profondità lungo il ramo E

... e così via fino a piazzare tutti gli aminoacidi
```
