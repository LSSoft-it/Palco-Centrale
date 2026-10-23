# 0002. Nucleo e moduli desktop in C++ con JUCE

- Stato: Proposto
- Data: 2026-10-23
- Thread: T01

## Contesto

Il nucleo comune (clock dello show, protocollo, scoperta, modello dello show) e i moduli desktop (mixer, playback, registrazione, luci) devono girare su Windows, macOS e Linux, e in parte su Android e iOS. Il thread audio non può allocare memoria, prendere lock, fare I/O o log (principio 2), quindi serve un linguaggio senza garbage collector.

Vincoli reali:

- Il Mac di sviluppo è Intel e resta su una versione vecchia di macOS, presumibilmente 10.15 Catalina o precedente: i driver Focusrite Saffire FireWire sono supportati solo fino a 10.15. Su Catalina l'ultimo Xcode installabile è il 12.4 (Apple clang 12).
- JUCE compila con Xcode 12.4 (macOS 10.15.4 su Intel), e ha come target macOS 10.11+, Windows 10, Linux, iOS 12+, Android 7+ (API 24).
- Luca ha già usato JUCE in passato.

## Decisione

- Nucleo e moduli desktop in **C++ con JUCE**, build con **CMake** (supporto CMake ufficiale di JUCE).
- Standard **C++20**, limitato alle funzionalità supportate sia da Apple clang 12 sia dai compilatori recenti (GCC, Clang, MSVC). In particolare niente `std::format`, `std::ranges` e `std::jthread` finché il Mac resta su Xcode 12.4. La CI verifica la compatibilità con i compilatori recenti; il Mac verifica quella con il compilatore vecchio.
- La logica di clock, protocollo e cue non dipende da JUCE dove non serve, così si testa senza hardware e con qualsiasi compilatore.

Da confermare: la versione esatta di macOS e di Xcode sul Mac (attesa per il 2026-10-26).

## Alternative scartate

- **Rust (con cpal per l'audio)**: toolchain moderna anche su Mac vecchi e sicurezza di memoria, ma l'ecosistema audio è meno completo (ASIO da compilare a parte, nessun equivalente di JUCE per mixer, file audio e plugin) e Luca non lo conosce.
- **Ibrido Rust per la rete e C++/JUCE per l'audio**: due linguaggi e un'interfaccia FFI da mantenere da soli, troppo per un progetto hobby.
- **Qt**: licenza complessa su mobile, Qt 6 probabilmente non supporta Catalina come host, e non porta un motore audio.

## Conseguenze

- Più attenzione alla gestione della memoria: test automatici e sanitizer (ASan, UBSan, TSan) in CI su Linux.
- Le funzionalità C++20 escluse vanno ricordate nelle review finché il Mac non viene sostituito.
- La licenza di JUCE incide sulla licenza del progetto: vedi [ADR 0005](0005-licenza.md).
