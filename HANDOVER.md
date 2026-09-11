# Passaggio al corso M1

Ultimo aggiornamento: 12 settembre 2026.

## Stato corrente e prossima azione

**Ripartire da U01 (U1): esercizio pratico Python/JSONL. Non ripetere U00.**
Andrea ha studiato con Claude durante il limite di utilizzo di questa sessione e dichiara compresi Appunti1 e Appunti2, **eccetto un punto della sezione U02 di Appunti2**. Il punto preciso non è ancora identificato: chiederglielo quando arriveremo a U02, senza anticipare ora la lezione.

U00 è stata risolta con motivazione corretta secondo il diario della sessione Claude. U01 e U03 hanno preparazione teorica; nessuna esecuzione pratica documentata. U02 non è consolidata. Non confondere studio anticipato con completamento delle unità.

Leggere [Appunti1](percorso/Appunti1.md), [Appunti2](percorso/Appunti2.md) e le nuove regole di ritmo in AGENTS. Piccole rettifiche tecniche sono state inserite il 12 settembre: sono correzioni degli appunti, non nuova comprensione verificata di Andrea.

## Primo incontro nella nuova sessione

1. Leggi AGENTS, questo file, Appunti2/U01 e la sezione U01 di [GUIDA_DOCENTE_M1](percorso/GUIDA_DOCENTE_M1.md). Appunti1 è il riferimento per i concetti già studiati, non materiale da rispiegare interamente.
2. Recap di poche frasi, poi un blocco utile con spiegazione, codice ed esercizio insieme. Punto di partenza: loader JSONL, accesso per ID, record invalido e campo source.
3. Chiarisci in quel contesto le rettifiche Python: il dict conserva l'ordine di inserimento ma si accede per chiave; leggere riga per riga e accumulare tutti i record in una lista non mantiene costante l'uso di memoria; il vecchio esempio salta errori senza registrarli.
4. Fai prevedere e modificare il codice ad Andrea; aspetta il ragionamento quando serve a verificare la comprensione. Niente micro-domande con un termine per messaggio e niente intera pipeline già costruita.
5. Chiudi U01 con una verifica pratica. Prima di U02 identifica il passaggio non capito e spiegane i prerequisiti; non passare automaticamente a embedding reali.
6. Nessuna installazione ML necessaria per iniziare U01. Quando serve, verificare ambiente/compatibilità e rispettare le approvazioni degli strumenti; nessuna spesa.

## Comprensione e prove

| Tema | Stato | Evidenza |
|---|---|---|
| U00: fonte pertinente e distinzione ricerca/risposta | Risolto con motivazione | Diario Claude: D1 per apertura Aurora |
| Embedding, tokenizzazione, pesi/vettori, pipeline M1 | Studiato e dichiarato compreso | Appunti1 e messaggio Andrea del 12 settembre |
| U01: Python, JSONL, loader e lookup | Teoria studiata; pratica da verificare | Appunti2 e dichiarazione Andrea |
| U02: NumPy, forme e operazioni vettoriali | **Non consolidato; dubbio aperto** | Andrea segnala un punto non capito; non inferire quale |
| U03: uso di Sentence Transformers | Teoria studiata; codice non eseguito | Appunti2; nessuna run documentata |
| Caching, agenti, wiki e fine-tuning pratico | Nessuna prova pratica | Pianificazione futura |

Non promuovere le correzioni appena aggiunte a «capite». Non inferire competenza dal silenzio o dal titolo di studio.

## Ritmo richiesto

Più contenuto per messaggio: spiegazione, codice, esercizio in un blocco coerente. Definire ogni termine nuovo con un esempio, senza tono da principiante assoluto. Ordinare autonomamente i prerequisiti; non seguire ciecamente l'ordine delle domande. Schemi espliciti per percorsi paralleli che convergono. Validare le sintesi di Andrea affermazione per affermazione. Produrre note riusabili, non far ripetere ciò che è già negli appunti. Le regole sono in AGENTS, senza dipendere dalla memoria privata di Claude.

## Perimetro e conservazione

Piano corrente: [PIANO_MASTER](PIANO_MASTER.md). M1 ricerca, poi M2 RAG/cache e M3 confronto agentico/wiki, stessa base software. Andrea è l'utilizzatore, nessuna ricerca clienti. CPU, 24 GB RAM, Iris Xe, zero spese aggiuntive. Alternative leggere già autorizzate; misurare prima di indicizzare tutto. Nessun download dati/modelli, installazione ML o risultato sperimentale documentato.

Repository corso: https://github.com/Migliaa/financial-knowledge-systems, main. Mockup indipendente: https://github.com/Migliaa/portfolio-redesign, main. Il lavoro precedente di collegamento/caricamento e redesign è concluso; non riprenderlo. Il sito sorgente non è stato modificato. Le capacità future nel mockup non sono competenze già dimostrate.

[COMPETENZE](percorso/COMPETENZE.md) definisce prove e criteri; Chroma U10b/L01, Redis facoltativo, FastAPI/Pydantic U16. Non anticiparli ora.

## Chiusura

Aggiornare questo file con unità, comprensione effettiva, esercizio e prossimo passo; una riga nel DIARIO e la nota/run coinvolta. Nessun nuovo handover parallelo. Per Andrea: [INIZIA_CON_SOL](INIZIA_CON_SOL.md). Nessuna compattazione manuale richiesta per la ripresa.
