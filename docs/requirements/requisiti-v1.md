# Palco Centrale – Requisiti e scenari (T02)

Stato: **bozza** · 23 ottobre 2026 · thread T02

Le voci marcate *(da confermare)* sono ipotesi in attesa della risposta di Luca.

## 1. Contesto e ipotesi di base

- Progetto hobbistico, sviluppatore unico: la v1 deve essere piccola, provabile in casa e con un amico.
- Show amatoriali: locale piccolo (pub, sala, piccolo teatro), 4-6 nodi, 15-20 brani per serata *(da confermare)*.
- Rete: un router/switch dedicato allo show, LAN isolata senza Internet. Computer audio e regia via cavo, tablet e telefoni in Wi-Fi.
- Hardware disponibile: Mac Intel (macOS ≤ 10.15) con Focusrite FireWire, telefono Android, tablet Android e iOS saltuari, PC Windows dell'amico tastierista, convertitore USB/DMX autocostruito, alcune luci DMX.

## 2. Attori

| Attore | Dove lavora | Cosa fa |
| --- | --- | --- |
| Regista / fonico | Mac, in cavo | Prepara lo show, lancia i GO, controlla lo stato dei nodi |
| Musicista | Tablet o telefono, in Wi-Fi | Legge testi e accordi che scorrono a tempo, sente il click in cuffia |
| Tastierista | PC Windows, in cavo | Riceve cambi di scena o programma via MIDI, opzionalmente lancia basi |
| Tecnico luci | Lo stesso regista nella v1 | Le luci seguono i cue, con override manuale |
| Telecomando | Telefono Android | GO / stop / avanti / indietro dal palco |

## 3. Scenari

### S1 – Concerto della band (prioritario)

Il Mac fa da regia e da motore audio. Per ogni brano un cue avvia la base e il click sulle uscite Focusrite (click solo in cuffia), richiama la scena luci, apre la pagina di testi e accordi sui tablet e mette un marker nella registrazione. Durante il brano le luci cambiano a battuta, i testi scorrono a tempo. Tra un brano e l'altro il regista o un musicista dà GO.

### S2 – Prova

Come S1, ma si salta a un brano qualsiasi, si riparte da una battuta o da un marker, si ripete un passaggio in loop e si cambia il BPM del click. Si registra per riascoltarsi.

### S3 – Spettacolo teatrale (dopo la band)

Cue list sequenziale con GO manuale: effetti sonori, cambi luce con dissolvenza, sopratitoli o copione su tablet. Nessun tempo musicale, solo show time.

### S4 – Guasto durante lo show (trasversale)

Un tablet perde il Wi-Fi, il telefono-telecomando si spegne, il cavo del nodo luci si stacca, il Mac regia si blocca. Lo show non si ferma: vedi §6.

## 4. Casi d'uso

| ID | Caso d'uso | Scenario | Priorità v1 |
| --- | --- | --- | --- |
| UC01 | Creare e salvare uno show (brani, cue, scene) in un file di progetto | tutti | Deve |
| UC02 | Aprire lo show e far associare i nodi con un codice di accoppiamento | tutti | Deve |
| UC03 | Vedere dalla regia lo stato di ogni nodo (OK, degradato, offline) | tutti | Deve |
| UC04 | GO / stop / cue successivo e precedente dalla regia | tutti | Deve |
| UC05 | GO / stop da telecomando Android | S1, S3 | Deve |
| UC06 | Riprodurre base e click su uscite separate, con BPM e metrica per brano | S1, S2 | Deve |
| UC07 | Richiamare scene luci DMX e cambi a battuta o a show time | S1, S3 | Deve |
| UC08 | Mostrare testi e accordi sincronizzati sul tablet | S1, S2 | Deve |
| UC09 | Saltare a brano, battuta o marker e ripartire | S2 | Deve |
| UC10 | Registrare in multitraccia sul Mac con marker di brano e cue | S1, S2 | Dovrebbe |
| UC11 | Inviare cambi di programma o scena MIDI alle tastiere | S1 | Dovrebbe |
| UC12 | Loop di un passaggio in prova | S2 | Dovrebbe |
| UC13 | Effetti sonori a GO e dissolvenze luci | S3 | Dovrebbe |
| UC14 | Sopratitoli e copione su tablet | S3 | Dopo |
| UC15 | Controllo di un mixer fisico via OSC/MIDI | S1 | Dopo *(da confermare)* |
| UC16 | MIDI Clock, MTC, LTC verso apparati esterni | S1 | Dopo |
| UC17 | Master di riserva che subentra alla regia | S4 | Dopo |

## 5. Obiettivi misurabili

Rivisti rispetto al documento di riferimento per show amatoriali. Ogni valore va misurato in T19 (prova sul campo) e, prima, con i simulatori di T09.

| Requisito | Documento | Proposta T02 | Motivo |
| --- | --- | --- | --- |
| Allineamento clock, LAN cablata | < 1 ms | **< 1 ms** (p99) | Necessario per audio e luci a tempo; realistico su switch locale |
| Allineamento clock, Wi-Fi | < 5 ms | **< 10 ms** (p95) | I nodi Wi-Fi mostrano solo testi e telecomando; il risparmio energetico dei telefoni rende instabile il Wi-Fi sotto i 5 ms. L'occhio non nota 10 ms su uno scorrimento |
| Latenza GO → esecuzione, cablato | < 20 ms | **< 20 ms** | I cue a tempo arrivano in anticipo; conta solo per il GO manuale |
| Latenza GO → esecuzione, da telecomando Wi-Fi | – | **< 50 ms** (p95) | Per un umano che preme un tasto è immediato |
| Latenza audio di rete | < 10 ms | **fuori dalla v1** | L'audio resta locale al Mac (vedi §7) |
| Latenza audio locale (ingresso → uscita) | – | **< 10 ms** | Buffer 128-256 campioni a 48 kHz sulla Focusrite |
| Riconnessione di un nodo | < 3 s | **< 3 s**, senza interrompere lo show | Invariato |
| Nodi gestiti | ≥ 16 | **≥ 16 con simulatori**, 6 reali | Lo show tipico ne ha 4-6; i 16 si provano con nodi finti |
| Durata di uno show senza riavvii | – | **≥ 4 ore** | Serata più prove |
| Avvio dello show dal file al primo GO | – | **< 2 minuti** | Montaggio rapido |

## 6. Comportamento in caso di guasto

| Guasto | Comportamento richiesto |
| --- | --- |
| Tablet perde il Wi-Fi | Continua a scorrere con l'ultimo tempo noto, mostra un avviso, si riallinea al ritorno |
| Telecomando spento | Nessun effetto; i GO restano disponibili dalla regia |
| Nodo luci perde la regia | Mantiene l'ultima scena accesa (nessun blackout) e lo segnala |
| Nodo audio perde la rete | La base in corso continua fino alla fine del brano |
| Regia bloccata | v1: i nodi tengono l'ultimo stato; riavvio della regia e riaggancio < 30 s. Master di riserva: dopo (UC17) |
| Comando da nodo non associato | Ignorato e registrato nel log |

## 7. Decisioni aperte chiuse in T02 (proposte)

| Decisione | Proposta | Stato |
| --- | --- | --- |
| Mixer interno, controllo di mixer fisici, o entrambi? | v1: nessun mixer software con DSP; solo livelli di base e click sulle uscite. Controllo di mixer fisici dopo, se ne esiste uno digitale | *(da confermare)* |
| Audio in rete tra i nodi? | No nella v1: audio locale al Mac con la Focusrite | *(da confermare)* |
| Hardware da supportare per primo | Focusrite FireWire su Mac, convertitore USB/DMX autocostruito, tastiere via MIDI | *(modelli da raccogliere)* |
| Formati di spartito prioritari | ChordPro e PDF; MusicXML dopo | *(da confermare)* |
| Lingua dell'interfaccia | Italiano, testi separati dal codice per una traduzione futura | *(da confermare)* |

## 8. Fuori ambito nella v1

Streaming su Internet, controllo via cloud, DSP da console professionale, audio in rete, video (oltre al controllo OSC di software esterni), master di riserva automatico, pubblicazione su App Store e Play Store.

## 9. Criteri di accettazione della v1

1. Con il Mac, un tablet e il telefono si esegue uno show S1 di 3 brani con base, click, luci DMX e testi a tempo.
2. Staccando il Wi-Fi a un tablet durante un brano, lo show continua e il tablet si riallinea in < 3 s.
3. Staccando il cavo del nodo luci, la scena resta accesa.
4. Gli stessi 3 brani girano con soli simulatori, senza hardware, su CI.
