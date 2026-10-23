# 0003. Interfacce e app sui dispositivi

- Stato: Proposto (scelta aperta)
- Data: 2026-10-23
- Thread: T01 (requisito da T02)

## Contesto

Le interfacce (regia, fonico, tecnico luci, musicista/attore con testi e spartiti) girano su computer, tablet e telefoni Android e iOS. Il documento di progetto proponeva Flutter.

Requisito emerso in T02: il motore centrale (scaletta, clock, master) deve poter girare **anche su un tablet, senza nessun computer**. Il primo scenario da provare è la one man band: un tablet sul leggio con scaletta, testi e accordi che avanzano con un pedale (page-turner Bluetooth o MIDI). Il telecomando non è un ruolo ma un ingresso. Dettagli in "Palco Centrale – Ruoli e modalità (T02)".

Quindi su Android e iOS serve un'app nativa che contenga il nucleo, e non si può rimandare tutto a T18.

Vincoli reali:

- Flutter richiede almeno macOS 11 per sviluppare, quindi non gira sul Mac di Luca (vedi [ADR 0002](0002-nucleo-cpp-juce.md)). Gira su Linux, quindi sul server di casa (vedi [ADR 0004](0004-ambiente-di-build.md)).
- JUCE compila per Android anche da Linux; iOS solo in CI.
- Per installare un'app su un iPad senza un Mac recente servono un account Apple Developer (99 USD/anno) e TestFlight.

## Decisione proposta

**Un'app JUCE per ogni piattaforma, con il nucleo C++ e l'interfaccia scritta in web (TypeScript) dentro una WebView** (il `WebBrowserComponent` di JUCE con integrazione nativa).

- Sul tablet della one man band l'app è master: nucleo, clock e scaletta girano lì, e la stessa app mostra testi e accordi.
- In una produzione più grande ogni nodo usa la stessa app; i satelliti possono anche aprire l'interfaccia da un semplice browser, servita dal nodo master.
- Un solo nucleo (C++) e una sola base di codice per le interfacce (web), su tutte le piattaforme.
- Il pedale arriva come tasto (page-turner Bluetooth) o come messaggio MIDI, gestito dal nucleo come ingresso generico.

Da verificare con un prototipo in T03: WebView di JUCE su Android e iOS, ricezione dei tasti del pedale con l'app in primo piano, schermo sempre acceso.

## Alternative

| Opzione | Pro | Contro |
| --- | --- | --- |
| App JUCE + interfaccia web in WebView (proposta) | Un nucleo e una UI per tutto; la UI si apre anche da browser senza installare nulla; mDNS e schermo acceso nativi; Android si compila dal server di casa | WebView da validare su mobile; due linguaggi (C++ e TypeScript), ma separati da un confine chiaro |
| Flutter + nucleo C++ via FFI | UI mobile molto curata | Tre pezzi (Dart, C++, FFI); nessuno sviluppo sul Mac; iOS solo in CI |
| Tutto JUCE, UI comprese | Un solo linguaggio | UI scomoda per testi, accordi e spartiti su tablet |
| Solo web app servita dai nodi | Nessuna installazione | Non copre il tablet senza computer: scartata dopo il requisito T02 |
| Qt/QML | Valido su desktop | Licenza LGPL difficile su iOS |

## Conseguenze

- Il nucleo deve compilare e girare su Android e iOS fin dall'inizio, non solo sui desktop.
- Il protocollo del piano di controllo deve essere usabile da una WebView e da un browser: WebSocket con JSON (MessagePack resta possibile come codifica alternativa).
- iOS richiede l'account Apple Developer prima delle prove sull'iPad.
