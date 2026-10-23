# Palco Centrale – Requisiti e scenari (T02)

Stato: **bozza** · 23 ottobre 2026 · thread T02

Le voci *(da confermare)* sono ipotesi in attesa di Luca.

## 1. Principi

1. **Ruoli, non dispositivi.** Ogni funzione appartiene a un ruolo; un dispositivo non ha un ruolo fisso.
2. **Ruoli accorpabili o divisi.** Una persona può avere tutti i ruoli (one man band) o uno solo; un ruolo può essere diviso tra più persone (es. fonico di sala e di palco).
3. **Qualsiasi ruolo su qualsiasi dispositivo**, nei limiti dell'hardware collegato: audio, DMX e MIDI girano dove sono collegate le interfacce.
4. **Un motore centrale e dei satelliti.** Ogni show ha un solo motore centrale (scaletta e clock dello show); gli altri nodi si agganciano a lui. Il motore può girare su computer **o su tablet**.
5. **Gli ingressi non sono ruoli.** Pedale, tasti, controller MIDI, telefono: sono modi per dare comandi a un ruolo.
6. **Dal "miserello" a Hollywood.** Lo stesso software deve servire un solo telefono e una produzione grande; la v1 punta alle configurazioni piccole.

## 2. Ruoli

| Ruolo | Responsabilità | Preparazione | Esecuzione |
| --- | --- | --- | --- |
| Regista | Flusso dello spettacolo, coordinamento, controllo che tutto vada bene | Scrive la scaletta (brani, cue, scene), assegna i ruoli ai dispositivi | GO, salti, stop; stato di tutti i nodi |
| Fonico | Audio: mixer (sala, palco/monitor, mix personali), basi, sequenze, effetti sonori, registrazione | Disegna il mixer, importa basi, prepara scene per brano | Livelli, cambi scena automatici, registrazione |
| Musicista / Attore | Esegue con testi e spartiti sincronizzati, click e mix personale in cuffia | Importa o scrive testi e spartiti, sceglie il proprio mix | Scorrimento a tempo; riceve cambi di scena e programmi MIDI (tastiere, ma anche effetti delle chitarre) |
| Tecnico luci | Luci ed effetti: mixer luci, sequenze di effetti | Definisce luci, scene e sequenze, le lega ai brani | Cambi automatici, interventi a mano |

## 3. Modalità

| Modalità | Uso | Permessi |
| --- | --- | --- |
| Preparazione | Costruire lo show, anche senza rete | Tutto |
| Esecuzione · Prove | Provare | Come Live, più salti a brano/battuta/marker, loop, cambio BPM, piccole modifiche salvate nello show |
| Esecuzione · Live | Lo spettacolo | Comandi in tempo reale; modifiche valide solo per la serata |
| Emergenza (dentro l'Esecuzione) | Tenere in piedi lo show | Comandi per ruolo con precedenza su tutto: GO manuale e salto cue, muto generale e stop basi, luce fissa di servizio e congelamento scena, scorrimento a mano |

*(da confermare: Prove come variante dell'Esecuzione, non terza modalità)*

## 4. Configurazioni di riferimento

| ID | Configurazione | Dispositivi | Motore centrale | v1 |
| --- | --- | --- | --- | --- |
| C1 | One man band minima | 1 tablet sul leggio + pedale (simulato) | Tablet | **Sì, primo test** |
| C2 | One man band completa | Tablet o PC + interfaccia audio + DMX | PC o tablet | Dovrebbe |
| C3 | Band amatoriale | Mac + Focusrite, tablet e telefoni, PC dell'amico, luci DMX | Mac | **Sì** |
| C4 | Musical / teatro medio | Computer regia, audio, luci; tablet attori e sopratitoli | Computer regia | Verifica del modello |
| C5 | Produzione grande | Come C4 con ruoli divisi tra più persone | Computer regia con riserva | Dopo |

## 5. Scenari

| Scenario | Contenuto |
| --- | --- |
| S1 · Concerto | Scaletta di brani con tempo musicale: testi e accordi a tempo, click, basi, luci a battuta, MIDI agli strumenti, registrazione |
| S3 · Teatro | Cue list a GO manuale: effetti sonori, luci con dissolvenze, copione e sopratitoli. **Contiene S1**: un musical mescola scene e brani nella stessa scaletta |
| S4 · Guasto | Rete che cade, nodo spento, motore bloccato: lo show non si ferma (§8) |

Le Prove (ex S2) sono una modalità, non uno scenario.

## 6. Casi d'uso

| ID | Caso d'uso | Ruolo | Priorità v1 |
| --- | --- | --- | --- |
| UC01 | Creare e salvare uno show: scaletta con brani e cue teatrali mescolati | Regista | Deve |
| UC02 | Assegnare ruoli ai dispositivi, anche tutti a uno solo | Regista | Deve |
| UC03 | Associare i nodi allo show con codice di accoppiamento | Regista | Deve |
| UC04 | Vedere lo stato di ogni nodo (OK, degradato, offline) | Regista | Deve |
| UC05 | GO / avanti / indietro / stop, anche da ingresso esterno (pedale, tasto, MIDI) | Regista | Deve |
| UC06 | Testi e accordi (ChordPro) a schermo, avanzamento a pedale o a tempo | Musicista | Deve |
| UC07 | Basi e click con BPM e metrica per brano | Fonico | Deve |
| UC08 | Mixer software modulare: uscita principale + N mandate (sala, palco, mix personali) | Fonico | Deve |
| UC09 | Scene luci DMX richiamate dai cue e cambi a battuta | Tecnico luci | Deve |
| UC10 | Salti a brano, battuta o marker; loop; cambio BPM | tutti (Prove) | Deve |
| UC11 | Comandi di emergenza per ruolo | tutti | Deve |
| UC12 | Cambi di programma MIDI verso tastiere ed effetti | Musicista | Dovrebbe |
| UC13 | Mix personale scelto dal musicista dal proprio dispositivo | Musicista | Dovrebbe |
| UC14 | Registrazione multitraccia con marker di brano e cue | Fonico | Dovrebbe |
| UC15 | Controllo di un mixer esterno via OSC/MIDI | Fonico | Dovrebbe |
| UC16 | Effetti sonori a GO e dissolvenze luci | Fonico, Tecnico luci | Dovrebbe |
| UC17 | Altri formati di spartito: tablature Guitar Pro (export di Songsterr), PDF, MusicXML | Musicista | Dopo |
| UC18 | Sopratitoli e copione | Attore | Dopo |
| UC19 | MIDI Clock, MTC, LTC verso apparati esterni | Fonico | Dopo |
| UC20 | Motore centrale di riserva | Regista | Dopo |
| UC21 | Pedale Bluetooth con ESP32 | ingresso | Dopo |

## 7. Obiettivi misurabili

Da misurare con i simulatori (T09) e nella prova sul campo (T19).

| Requisito | Documento | Proposta T02 | Motivo |
| --- | --- | --- | --- |
| Allineamento clock, LAN cablata | < 1 ms | **< 1 ms** (p99) | Audio e luci a tempo |
| Allineamento clock, Wi-Fi | < 5 ms | **< 10 ms** (p95) *(da confermare)* | Il risparmio energetico dei telefoni rende instabile il Wi-Fi sotto i 5 ms; 10 ms non si notano su testi e luci |
| Latenza GO → esecuzione, cablato | < 20 ms | **< 20 ms** | Conta solo per il GO manuale |
| Latenza GO → esecuzione, da Wi-Fi o pedale | – | **< 50 ms** (p95) | Percepito come immediato |
| Latenza audio locale (ingresso → uscita) | – | **< 10 ms** | Buffer 128-256 campioni a 48 kHz |
| Latenza audio di rete | < 10 ms | **fuori dalla v1** *(da confermare)* | Audio locale al nodo con l'interfaccia |
| Riconnessione di un nodo | < 3 s | **< 3 s** senza fermare lo show | Invariato |
| Nodi gestiti | ≥ 16 | **≥ 16 con simulatori**, 6 reali | Lo show tipico ne ha 4-6 |
| Durata senza riavvii | – | **≥ 4 ore** | Serata più prove |
| C1 dall'accensione al primo brano | – | **< 1 minuto** | Il caso minimo deve essere immediato |

## 8. Comportamento in caso di guasto

| Guasto | Comportamento richiesto |
| --- | --- |
| Satellite perde la rete | Continua con l'ultimo stato e la timeline ricevuta, lo segnala, si riallinea al ritorno |
| Nodo luci perde il motore | Mantiene l'ultima scena accesa (nessun blackout) |
| Nodo audio perde la rete | La base in corso continua fino a fine brano |
| Ingresso (pedale, telefono) spento | Nessun effetto; i comandi restano disponibili dal Regista |
| Motore centrale bloccato | v1: i nodi tengono l'ultimo stato; riavvio e riaggancio < 30 s. Riserva: dopo (UC20) |
| Comando da nodo non associato | Ignorato e registrato nel log |

## 9. Decisioni prese in T02

| Tema | Decisione |
| --- | --- |
| Mix | Sala, palco/monitor, mix personale per musicista |
| Mixer | Sia mixer software modulare (uscita principale + N mandate) sia controllo di mixer esterni. Escluso nella v1 il DSP da console professionale |
| Motore centrale | Su qualsiasi dispositivo, anche tablet |
| Formati di spartito | Importatori a moduli; primo ChordPro, poi tablature Guitar Pro (da Songsterr), PDF, MusicXML |
| Lingua | Italiano, interfaccia pronta per la localizzazione |
| Pedale | Simulato (tastiera/tocco); pedale ESP32 in futuro |

## 10. Impatti sul documento di riferimento e su altri thread

- **Documento di riferimento**: i dispositivi mobili non sono più solo nodi di controllo; il motore centrale può girare su tablet. Il "telecomando" non è un modulo ma un ingresso. Il mixer software rientra nella v1.
- **T01**: il nucleo deve girare come motore su Android e iOS; servono app mobili già per C1, non solo in T18.
- **T07**: la scaletta mescola brani (tempo musicale) e cue teatrali (solo show time); ruoli e assegnazione ai dispositivi fanno parte dello show.
- **T12**: mixer con N mandate e mix personali.
- **T14**: interfaccia comune per i moduli di import degli spartiti.

## 11. Domande aperte

- Modelli dell'hardware: mixer, Focusrite, luci DMX, strumenti e interfacce dell'amico.

## 12. Criteri di accettazione della v1

1. **C1**: un tablet da solo esegue una scaletta di 3 brani con testi e accordi, avanzando con un pedale simulato.
2. **C3**: Mac, un tablet e un telefono eseguono 3 brani con base, click, mix personale, luci DMX e testi a tempo.
3. Staccando il Wi-Fi a un tablet durante un brano, lo show continua e il tablet si riallinea in < 3 s.
4. Staccando il cavo del nodo luci, la scena resta accesa.
5. Gli stessi scenari girano con soli simulatori, senza hardware, su CI.
