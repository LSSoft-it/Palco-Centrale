# 0005. Licenza del progetto e delle dipendenze

- Stato: Proposto
- Data: 2026-10-23
- Thread: T01

## Contesto

Il progetto è un hobby di una sola persona e per ora non si distribuisce. JUCE è disponibile con licenza AGPLv3 oppure commerciale; il livello commerciale Starter è gratuito fino a 20.000 USD di ricavi annui totali (termini annunciati per JUCE 8; la versione attuale usa l'EULA di JUCE 9, da ricontrollare prima di distribuire).

## Decisione

- Repository **privato** per ora.
- JUCE usato con la **licenza Starter** gratuita.
- Solo dipendenze con licenze **permissive** (MIT, BSD, Apache-2.0, ISC, zlib) o compatibili con AGPLv3. Ogni nuova dipendenza riporta la licenza nel file di inventario delle dipendenze (da creare in T03).
- La scelta tra open source e uso commerciale si rimanda: le due strade restano aperte.

Non è un parere legale; i termini esatti stanno nella licenza e nell'EULA di JUCE.

## Alternative scartate

- **Open source subito (AGPLv3)**: possibile, ma non porta vantaggi finché il progetto è privato e acerbo.
- **Qt**: la LGPL su iOS è difficile da rispettare (vedi [ADR 0002](0002-nucleo-cpp-juce.md)).

## Conseguenze

- Se un giorno il progetto si pubblica come open source, tutto il codice che usa JUCE diventa AGPLv3.
- Se si vende, si resta su Starter fino alla soglia di ricavi, poi serve la licenza Indie o Pro.
- Dipendenze con licenze copyleft diverse da AGPLv3 (per esempio GPLv2 only) sono escluse.
