# Architettura di base e confini degli esperimenti

> Aggiornamento 12 settembre: vincoli CPU e integrità sperimentale restano utili; ordine didattico e varianti sono ora definiti dal PIANO_MASTER D1–D6. Due encoder piccoli nel confronto iniziale; API/database/cache avanzate non bloccano il confronto RAG/agente/wiki. Non usare i moduli tecnici di questo documento per imporre lezioni di implementazione di base.

> Questo documento dettaglia la base tecnica e i controlli CPU del laboratorio. Il programma corrente è [PIANO_MASTER](../PIANO_MASTER.md): M1 ricerca, M2 RAG personale/cache, M3 confronto agentico/wiki. Il piano definisce componenti successivi, didattica e criteri di chiusura. Non aggiungerli tutti ora. Fine-tuning pratico facoltativo.

Disegno concordato, implementazione da iniziare con Sol. Non è un elenco di dipendenze già installate. Prima tappa: embedding e ricerca; il generatore viene aggiunto successivamente.

## Dati e dominio

Base **MTRAG FiQA**, dati finanziari testuali. Gli autori riportano 7.661 documenti e 49.607 passaggi e raccomandano la versione a passaggi per allinearsi ai riferimenti. Conservare ID e segmentazione ufficiali. MTRAG FiQA non coincide con l'intero dataset BEIR FiQA: non mescolare corpora, query o punteggi.

Prima del download fissare revisione, URL e condizioni dei file specifici; poi salvare hash, conteggi e provenienza in `data/manifest.json`. Verificare formato, ID unici e copertura dei riferimenti. I README possono contenere percorsi vecchi: usare i file realmente presenti nella revisione scelta.

Il recupero ordinario riceve domanda, eventuale storia e corpus. Etichette, risposte attese e qrels (associazioni tra domanda e fonti pertinenti) sono accessibili solo alla valutazione. Il controllo con fonti corrette è una condizione sperimentale separata, esplicitamente etichettata.

## Componenti minimi

| Componente futuro | Responsabilità | Output persistente |
|---|---|---|
| `src/data.py` | Leggere e validare i file; separare input e riferimenti | Manifest e dati normalizzati |
| `src/encode.py` | Produrre embedding in piccoli batch | Vettori e mappa riga → ID passaggio |
| `src/retrieve.py` | BM25, ricerca vettoriale esatta, fusione RRF | Classifica con ID e punteggi per domanda |
| `src/evaluate.py` | Metriche di recupero e controlli dei riferimenti | Risultati per domanda e aggregati |
| `src/generate.py`, più avanti | Preparare contesto entro un limite, invocare modello, registrare risposta | Prompt, passaggi e risposta |
| `scripts/figures/`, quando utile | Trasformare risultati salvati in figure | SVG/PNG con provenienza |

File e moduli sono proposti; crearli al bisogno, senza una libreria astratta prima del primo esperimento. Un adattatore dati, un embedding piccolo, un generatore locale eventuale. JSONL per record e output, NumPy per vettori, CSV per metriche, configurazioni versionate. Nessun server vettoriale, orchestratore, servizio cloud o interfaccia web obbligatorio.

Candidato iniziale: `BAAI/bge-small-en-v1.5`, da validare prima dell'uso. Verificare scheda, revisione, licenza, istruzione per query, tokenizer, lunghezza supportata e costo CPU. I passaggi ufficiali non garantiscono automaticamente compatibilità con il suo limite di token. Misurare e dichiarare troncamenti; non scegliere un modello a contesto corto ignorando il testo perso. Reranker candidato: `cross-encoder/ms-marco-MiniLM-L6-v2`, solo sui primi 20 risultati, prova E04 nel piano.

Cache identificata da hash del corpus/preprocessing e revisione del modello, con pooling, normalizzazione, prompt di codifica e troncamento. Una modifica incompatibile invalida la cache. Conservare ripresa dei batch, per non ripetere lavoro completato.

## Fattibilità e passaggio all'alternativa B

Le soglie seguenti sono guardrail di pianificazione scelti per questo portatile, non prestazioni promesse:

1. Dopo la prima esercitazione, codificare un campione deterministico di 128 passaggi; se economico estenderlo a 512 con lunghezze rappresentative. Registrare anche avvio/download separatamente. Interrompere il pilota dopo circa 5 minuti di calcolo se non progredisce abbastanza da stimare il costo.
2. Stimare il corpus completo mostrando formula e campione. Obiettivi iniziali: entro circa 90 minuti per l'indicizzazione una tantum, entro circa 12 GB di memoria del processo e ricerca interattiva nell'ordine di pochi secondi. Una stima è etichettata come tale fino alla misura completa.
3. Se una soglia è superata, provare al massimo due correzioni semplici (batch, cache, embedding più piccolo ma compatibile) e leggere la causa. Non fare giri infiniti di ottimizzazione. Informare Andrea dell'evidenza e passare a B se A resta sproporzionato, come già autorizzato; nuovi vincoli di Andrea prevalgono sulle soglie.
4. B riusa loader, metriche e tracciamento: studio della ricerca con embedding più leggeri su un task piccolo già annotato. Preferire finanza se resta sostenibile; altrimenti un piccolo dataset come SciFact, dichiarando il cambio. Comprimere i vettori non riduce automaticamente il tempo necessario a produrli: se è quello il collo di bottiglia, ridurre modello o scegliere un corpus davvero più piccolo.
5. Alla generazione fare prima 3–5 casi e misurare la latenza. Se il generatore o il giudice sono troppo lenti, chiudere il traguardo di recupero e dimensionare una dimostrazione RAG separata. Non comprare API; non presentare una piccola dimostrazione come valutazione completa.

## Protocollo del primo confronto

- Usare split ufficiali se adatti allo scopo. In alternativa separare per ID di conversazione, congelare gli ID prima di scegliere configurazioni e dichiarare lo split interno. Controllare che abbastanza conversazioni restino in ogni gruppo prima di fissare percentuali.
- Prima confronto BM25 / embedding / ibrido con **ultima domanda** uguale per tutti. Poi confronto ultima domanda / storia delle domande su una configurazione fissata. Evitare un prodotto di decine di combinazioni.
- Riutilizzare i passaggi ufficiali e gli stessi ID. RRF combina posizioni, non punteggi grezzi non confrontabili; fissare i parametri o sceglierli soltanto sullo sviluppo. Restituire 10 risultati senza duplicati.
- Recupero: nDCG@10 e Recall@5/@10 rispetto ai giudizi disponibili. Salvare risultati per domanda. Annotazioni incomplete possono omettere fonti utili: analizzare casi senza rietichettare il test per migliorare il punteggio. Astensione valutata separatamente dai soli task di recupero rispondibili/parziali.
- Non utilizzare le riscritture ufficiali delle query come se il sistema le producesse gratis: sono un'eventuale condizione separata, con provenienza e costo di produzione spiegati.
- Generazione, più avanti: modello, prompt e budget di contesto fissi. Condizioni fonti corrette / fonti recuperate; storia di riferimento esplicitata. Le risposte del benchmark non sono automaticamente un riferimento per un dialogo libero autoregressivo.
- Controlli automatici per tutti gli output; lettura con Andrea di pochi casi rappresentativi. Eventuale giudice LLM calibrato e dichiarato. Nessun numero aggregato di qualità generativa senza una valutazione adeguata al claim. Citazioni, correttezza e astensione sono dimensioni distinte.
- Dopo il test non ritoccare la configurazione e ripresentare lo stesso test come indipendente. Per eventuali intervalli, campionare per conversazione e rispettare la dipendenza tra turni.

## Fonti iniziali

- Corpus FiQA, conteggi e passaggi: https://github.com/IBM/mt-rag-benchmark/tree/main/corpora
- Query, qrels e impostazioni della ricerca: https://github.com/IBM/mt-rag-benchmark/blob/main/mtrag-human/retrieval_tasks/README.md
- Compiti generativi: https://github.com/IBM/mt-rag-benchmark/blob/main/mtrag-human/generation_tasks/README.md
- Valutazione: https://github.com/IBM/mt-rag-benchmark/blob/main/scripts/evaluation/README.md

Consultati il 10 settembre 2026. Durante l'acquisizione conservare il permalink alla revisione effettivamente usata. Fine-tuning e submission restano estensioni; non sono necessari per completare il primo progetto.
