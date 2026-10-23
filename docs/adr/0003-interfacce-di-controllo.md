# 0003. Interfacce di controllo

- Stato: Proposto (scelta aperta)
- Data: 2026-10-23
- Thread: T01

## Contesto

Le interfacce di controllo (regia su tablet, telecomando, testi e spartiti, sopratitoli) girano su tablet e telefoni Android e iOS e sui computer. Il documento di progetto proponeva Flutter.

Vincoli reali:

- Flutter richiede almeno macOS 11 per sviluppare, quindi non gira sul Mac di Luca (vedi [ADR 0002](0002-nucleo-cpp-juce.md)). Gira invece su Linux, quindi sul server di casa (vedi [ADR 0004](0004-ambiente-di-build.md)), che compila per Android, Linux e web; iOS solo in CI.
- I tablet Android e iOS sono disponibili solo ogni tanto, e sul PC aziendale non si installa nulla.
- Per installare un'app su un iPad senza un Mac recente serve un account Apple Developer (99 USD/anno) e TestFlight.

## Decisione proposta

**Web app servita dai nodi** (TypeScript), aperta dal browser del dispositivo di controllo. Parla con i nodi con lo stesso protocollo del piano di controllo (WebSocket, vedi T04). Le app native iOS/Android si rimandano a T18.

Questa proposta si discosta dal documento di progetto (che indicava Flutter) e va approvata da Luca.

## Alternative

| Opzione | Pro | Contro |
| --- | --- | --- |
| Web app (proposta) | Nessuna installazione su tablet, telefono e PC aziendale; nessun App Store né account Apple; si sviluppa ovunque | Niente mDNS dal browser (si entra con QR code o indirizzo); su iOS lo schermo che si spegne interrompe la pagina |
| Flutter | App native con mDNS e schermo sempre acceso; una base di codice per 4 piattaforme | Sviluppo e build su Linux o in CI, mai sul Mac; per iOS servono CI e TestFlight |
| Tutto JUCE | Un solo linguaggio | UI mobile poco moderna, scomoda per testi e spartiti; iOS comunque in CI |
| Qt/QML | Valido su desktop | Licenza LGPL difficile su iOS |

## Conseguenze

- Il protocollo del piano di controllo deve essere usabile da un browser: WebSocket con JSON (MessagePack resta possibile come codifica alternativa).
- Lo show time nel browser si ottiene con la stessa sincronizzazione del clock via WebSocket; la precisione attesa su Wi-Fi è compatibile con l'obiettivo di 5 ms per la visualizzazione, da misurare in T06.
- In T18 si valuta se incapsulare la web app o scrivere app native.
