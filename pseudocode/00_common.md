# Struttura Comune — HP 2D-Protein Folding

Questi elementi sono condivisi da tutti e 6 gli algoritmi.

---

## Rappresentazione dello Stato

```
STATO:
  sequenza  : stringa di H/P  (es. "HPPHH")  -- costante, non cambia mai
  posizioni : lista di coordinate (x,y) già piazzate
  indice    : indice dell'ultimo aminoacido piazzato
              (parte da 0 = primo aminoacido in (0,0))
```

---

## Direzioni possibili

```
DIREZIONI = [ SU=(0,1), GIÙ=(0,-1), SINISTRA=(-1,0), DESTRA=(1,0) ]
```

---

## Funzione: successori(stato)

```
// Genera tutti i prossimi stati validi dall'attuale
// Ritorna lista vuota se tutti gli aminoacidi sono già piazzati

FUNZIONE successori(stato):
  prossimo_indice = stato.indice + 1

  SE prossimo_indice >= lunghezza(stato.sequenza):
    RETURN []                        // nessun successore: stato terminale

  risultati = []

  PER OGNI direzione IN DIREZIONI:
    nuova_pos = stato.posizioni.ultima() + direzione

    SE nuova_pos NON è già in stato.posizioni:   // vincolo no-crossing
      nuovo_stato = copia_profonda(stato)
      aggiungi nuova_pos a nuovo_stato.posizioni
      nuovo_stato.indice = prossimo_indice
      aggiungi nuovo_stato a risultati

  RETURN risultati
```

---

## Funzione: è_completo(stato)

```
// Vero quando tutti gli aminoacidi della sequenza sono stati piazzati

FUNZIONE è_completo(stato):
  RETURN stato.indice == lunghezza(stato.sequenza) - 1
```

---

## Funzione: conta_contatti_HH(stato)

```
// Conta le coppie H-H con distanza Euclidea = 1
// che NON siano consecutive nella sequenza (indici non adiacenti)

FUNZIONE conta_contatti_HH(stato):
  contatti = 0

  PER i = 0 FINO A lunghezza(stato.sequenza) - 1:
    PER j = i+2 FINO A lunghezza(stato.sequenza) - 1:
      // j > i+1 garantisce che non siano consecutivi nella sequenza

      SE stato.sequenza[i] == 'H' E stato.sequenza[j] == 'H':
        dx = stato.posizioni[i].x - stato.posizioni[j].x
        dy = stato.posizioni[i].y - stato.posizioni[j].y
        distanza = sqrt(dx*dx + dy*dy)

        SE distanza == 1:
          contatti = contatti + 1

  RETURN contatti
```

---

## Stato Iniziale

```
stato_iniziale = {
  sequenza  : input dell'utente  (es. "PHHPHPPHP")
  posizioni : [ (0, 0) ]         -- primo aminoacido sempre in origine
  indice    : 0
}
```
