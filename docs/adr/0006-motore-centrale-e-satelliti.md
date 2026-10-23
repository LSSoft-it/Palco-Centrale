# 0006. App unica con motore centrale e satelliti su un bus comune

- Stato: Proposto
- Data: 2026-10-23
- Thread: Architettura generale (requisiti da T02)

## Contesto

Il documento di riferimento mette il master nella regia, su un computer, e vuole i dispositivi mobili solo come controllo. T02 ha cambiato le premesse: si progetta per ruoli, i ruoli si accorpano o si dividono, e il motore centrale deve poter girare anche su un tablet da solo (C1, primo test). Lo stesso software deve servire dalla one man band al teatro (C1-C5). Restano i principi: show time comune, fail-safe, nessun modulo che parla direttamente con un altro, tutto provabile senza hardware.

## Decisione

1. **Una sola app** per tutte le piattaforme: nucleo comune + moduli esecutori + interfacce di ruolo. Cosa è attivo su un nodo dipende dallo show e dall'hardware collegato.
2. **Motore centrale come servizio separato dalla regia**: clock master, show attivo, cue e trasporto, registro dei nodi. Uno solo per show, scelto a mano all'apertura.
3. **Bus di messaggi unico** con trasporto locale (in memoria) o di rete (WebSocket): gli stessi messaggi versionati in entrambi i casi. Un modulo non sa dove si trova il motore.
4. **Satelliti autonomi**: ricevono in anticipo comandi con show time di esecuzione e la mappa del tempo del brano, e da lì eseguono da soli.

## Alternative scartate

- **Regia = master su computer** (documento di riferimento): non copre C1, tablet da solo.
- **Un processo per modulo** (microservizi locali): isolamento migliore, ma iOS non permette processi separati e la one man band diventerebbe pesante.
- **Motore che fa tutto, satelliti come schermi passivi**: semplice, ma se la rete cade si ferma tutto (viola il fail-safe) e la latenza di rete entra nel tempo.
- **Elezione automatica del motore**: utile per la riserva, ma complessa; rimandata a T17.

## Conseguenze

- Il nucleo e il motore devono compilare su Android e iOS fin dall'inizio.
- I test multi-nodo possono girare in un solo processo con il bus locale e i simulatori.
- Il protocollo (T04) deve prevedere: busta con versione, nodo, show, sequenza, show time; comandi con istante di esecuzione; pubblicazione della mappa del tempo.
- La struttura del repository aggiunge `modules/engine/`, `modules/input/`, `modules/sync/`.
- Il documento di riferimento va aggiornato (§ Architettura di riferimento) quando l'ADR è accettato.
