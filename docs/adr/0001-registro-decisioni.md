# 0001. Registro delle decisioni architetturali

- Stato: Proposto
- Data: 2026-10-23
- Thread: T01

## Contesto

Palco Centrale è sviluppato da una sola persona, a tempo perso e a intervalli. Tra una sessione e l'altra si perde memoria del perché di una scelta, e il documento di progetto ("Palco Centrale – Istruzioni e ossatura") chiede che ogni scelta non ovvia sia registrata.

## Decisione

- Ogni decisione architetturale si registra come ADR in `docs/adr/NNNN-titolo.md`, numerato in sequenza e mai rinumerato.
- Struttura fissa: contesto, decisione, alternative scartate, conseguenze (modello in [README](README.md)).
- Stati: Proposto, Accettato, Sostituito da NNNN, Ritirato. Un ADR accettato non si riscrive: si sostituisce con uno nuovo.
- Gli ADR sono in italiano, come il documento di progetto; codice, commenti e commit restano in inglese.
- Ogni ADR passa da una pull request; il merge equivale all'accettazione.

## Alternative scartate

- **Decisioni solo nel documento di progetto**: si perde la storia delle scelte superate.
- **Wiki di GitHub**: non è versionato insieme al codice e non passa dalle review.

## Conseguenze

- Ogni thread Txx che prende una decisione non ovvia aggiunge un ADR e aggiorna l'indice in `README.md`.
- Il costo è basso (un file breve per decisione) e permette di riprendere il progetto dopo mesi.
