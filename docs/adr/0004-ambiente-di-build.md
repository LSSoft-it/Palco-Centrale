# 0004. Ambiente di sviluppo e di build

- Stato: Proposto
- Data: 2026-10-23
- Thread: T01

## Contesto

Macchine disponibili:

- **PC Windows aziendale**: non si installa nulla, ma si apre github.com e un terminale web verso il server di casa.
- **Server Linux di casa**: x86_64, Debian 12, 12 core, 15 GB di RAM, circa 370 GB liberi, Docker 29. Già usato per compilare progetti ESP32 con git.
- **Mac Intel** con macOS vecchio e Focusrite Saffire FireWire: unica macchina con l'interfaccia audio.
- Telefono Android, tablet Android e iOS saltuari, PC Windows di un amico per le prove.

Per compilare per macOS e iOS servono l'SDK e la firma di Apple, disponibili solo su Mac; la licenza di macOS ne permette la virtualizzazione solo su hardware Apple.

## Decisione

- **Scrittura del codice**: dal PC aziendale con github.dev (editor nel browser) o Notepad++; tutto passa da git e dal repository GitHub privato.
- **Build Linux, Android, web e Windows**: sul server di casa con un **runner GitHub Actions self-hosted** in Docker. Il runner si collega a GitHub in uscita, quindi non servono porte aperte verso casa. Toolchain dentro immagini Docker versionate nel repository (`tools/docker/`).
- **Build Windows**: cross-compilazione dal server; la toolchain (MinGW-w64 o clang-cl con SDK Microsoft) si sceglie in T03 dopo una prova con JUCE. Il runner `windows-latest` di GitHub resta come verifica con MSVC.
- **Build macOS e iOS**: runner macOS di GitHub (Apple Silicon), con pochi minuti gratuiti sul repository privato, quindi solo su richiesta o prima di una prova. Il Mac vecchio compila i moduli audio per sé stesso.
- **Test audio con hardware reale**: sul Mac con la Focusrite e sul PC dell'amico.
- Gli eseguibili si scaricano come artefatti delle build.

## Alternative scartate

- **Mac aggiornato all'ultima versione**: perderebbe la Focusrite FireWire.
- **macOS virtualizzato su hardware non Apple**: fuori licenza e fragile.
- **Mac a noleggio (AWS EC2 Mac, MacinCloud, Scaleway)**: AWS ha un minimo di 24 ore per sessione; costi da valutare solo se la CI di GitHub non basta.
- **Solo runner GitHub ospitati**: con il repository privato i minuti gratuiti finiscono presto, soprattutto su macOS.

## Conseguenze

- Il server di casa diventa parte dell'infrastruttura: se è spento le build Linux, Android e Windows si fermano (si può ripiegare sui runner di GitHub).
- Il runner self-hosted va usato solo con il repository privato, per non eseguire codice di terzi sul server.
- T03 implementa immagini Docker, workflow e runner.
