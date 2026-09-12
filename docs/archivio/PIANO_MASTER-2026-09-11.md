# Corso-progetto — sistemi di conoscenza finanziaria

**Piano operativo del 11 settembre 2026.** Sostituisce le priorità precedenti in STRATEGIA_PROFILO, SCELTA e IMPOSTAZIONE. Tre macroprogetti sequenziali, una base software condivisa, Andrea come utilizzatore. Nessuna ricerca di clienti, candidatura o raccolta di feedback esterno è necessaria per completarli. Non è un piano di produzione aziendale né una promessa di prestazioni. Nessuna implementazione ancora avviata.

## Obiettivo, vincoli e metodo

Andrea impara a leggere, modificare, misurare e spiegare un sistema che cerca e usa documenti. Finanza come dominio motivante; zero euro aggiuntivi; 24 GB RAM, CPU come riferimento. Il progetto deve insegnare sia i concetti AI sia il codice che li collega. I risultati sono quelli osservati, anche quando una variante peggiora.

**Scelta tecnica iniziale:** MTRAG FiQA con passaggi e giudizi ufficiali; ricerca esatta per evitare un server; un embedding piccolo; generatore locale scelto dopo un pilota. Il passaggio già autorizzato alla ricerca più leggera resta disponibile se il carico è eccessivo. Non si sacrifica la comprensione per finire prima.

Ogni unità segue: problema concreto → spiegazione in italiano → esempio manuale → lettura di una piccola funzione → previsione di Andrea → sua modifica o piccolo esercizio → esecuzione e test → nota breve. Le attività meccaniche sono automatizzate. SOL deve spiegare dati in ingresso, trasformazioni, uscita e possibili errori del codice; non limitarsi a commentarlo riga per riga. Andrea sceglie se scrivere da sé o guidare la modifica, ma deve poter ricostruire il ragionamento.

**Ritmo di partenza, adattabile:** unità di circa 60–90 minuti di lavoro attivo, non scadenze. Su 5 ore disponibili alternare concetti, codice, esperimenti e una breve chiusura documentale. Non stimare il numero totale di giorni prima della prima unità. Un blocco non compreso si riprende; la compilazione del codice non equivale ad apprendimento.

## Mappa dei tre macroprogetti

| ID | Domanda | Consegna autonoma | Dipendenza |
|---|---|---|---|
| M1 — Ricerca | Quali informazioni troviamo, e con quale costo? | Motore di ricerca locale + confronto riproducibile + report breve | Basi Python necessarie, insegnate durante il lavoro |
| M2 — RAG personale | Le fonti diventano risposte supportate, aggiornabili e veloci? | Assistente locale usato da Andrea + prove di risposte/cache/errori + report | M1 funzionante e compreso |
| M3 — Ricerca agentica e wiki | Conviene esplorare durante la domanda o preparare conoscenza prima? | Confronto controllato di tre strategie + prova di aggiornamento + report | M2 e generatore con capacità sufficienti |

Chiudere M1 prima di ampliare M2, e M2 prima di M3. Possibile affiancare solo brevi laboratori di basi; niente tre implementazioni in parallelo. Se M3 è troppo oneroso, una dimostrazione documentata su pochi casi resta didattica e non viene chiamata benchmark conclusivo. M1 e M2 restano consegne valide.

## Unità e prove di apprendimento

La [matrice competenze](../../percorso/COMPETENZE.md) definisce verifiche di comprensione del codice, strumenti scelti e formulazioni pubbliche ammissibili. Aggiunge U10b/L01 (Chroma locale) e L02 facoltativo (Redis), senza moltiplicare gli esperimenti principali. La struttura pubblica è definita in [PORTFOLIO](../../docs/PORTFOLIO.md): un progetto visibile con tre moduli, non nove sezioni da visitare.

| Unità | Concetti e lavoro concreto | Codice da capire/modificare | Evidenza e documentazione |
|---|---|---|---|
| U00 | Perché cercare fonti, perché un modello può rispondere senza averle | Nessun prerequisito; esempio dei tre documenti in percorso/00 | Andrea distingue ricerca e risposta; schema della catena |
| U01 | File, liste, dizionari, funzioni, JSONL, eccezioni; solo lacune effettive | Leggere tre record, cercare un ID, gestire un record invalido | Piccolo esercizio e test; KodeKloud Python se utile |
| U02 | Vettore, dimensioni, norma, prodotto scalare e coseno | Calcolo su vettori didattici, poi NumPy; forma della matrice | Andrea predice una vicinanza e spiega limiti del disegno 2D |
| U03 | Embedding appreso, bi-encoder, tokenizer, normalizzazione, troncamento | Codificare testi e query, vedere forme e classifiche | Primo embedding reale; distinguere output del modello e numeri inventati |
| U04 | Dataset, fonti pertinenti, split, confronto equo | Loader FiQA, ID, manifest, controlli qrels | Dati e split congelati; prova di fattibilità CPU |
| U05 | Ricerca per parole, frequenze, BM25 | Una ricerca manuale piccola, poi libreria; tokenizzazione | E01: prima misura, spiegazione di Recall e nDCG su pochi risultati |
| U06 | Ricerca semantica esatta | Matrice-vettore, ordinamento, top-k, cache embedding | E02: confronto con E01; errori su nomi/numeri/parafrasi |
| U07 | Fusione delle classifiche, RRF | Implementare una piccola funzione di fusione con test | E03: ibrido vs componenti; non presumere vittoria |
| U08 | Reranking e cross-encoder; costo query-passaggio | Riordinare solo i primi 20 candidati, restituire 10 | E04: prova CPU e confronto; facoltativo adottarlo |
| U09 | Perché la storia cambia una domanda | Costruire query da ultima domanda / storia delle domande | E05: confronto isolato; chiusura M1 e report |
| U10 | Chunking, overlap, titoli, riferimenti al documento | Due segmentazioni di testi didattici o derivati con mappa ID | E06: prova didattica; distinta dal punteggio ufficiale |
| U10b | Persistenza, metadati e ciclo di vita in Chroma locale | Riutilizzare embedding, filtrare, riaprire, aggiornare/eliminare record | L01: confronto d'integrazione con NumPy; non nuovo benchmark; rinviabile per incompatibilità |
| U11 | Modello generativo, token, contesto, prompt e citazioni | Costruzione del contesto con limiti, invocazione locale | Pilota 3–5 domande; configurazione generativa congelata |
| U12 | RAG semplice vs selezione migliore; fonti corrette come controllo | Collegare ricerca e generazione; formati di risposta | E07: nessuna fonte / fonti corrette / recuperate sullo stesso campione |
| U13 | Errori di ricerca vs risposta, astensione, citazioni | Controlli formali e valutazione separata | Pochi casi letti insieme; limiti espliciti delle misure automatiche |
| U14 | Cache persistente ed esatta, invalidazione | Chiavi, versioni, TTL, hit/miss; test cambio corpus/prompt | E08: carico ripetuto freddo/caldo, risparmi e risultati obsoleti |
| U15 | Cache semantica e falsi riusi | Ricerca nella cache, soglia sullo sviluppo, filtri di entità/data | E09: laboratorio circoscritto, funzione disattivata di default |
| U16 | API locale FastAPI/Pydantic, input, timeout, errori, test e uso personale | Collegare componenti senza duplicare logica; piccolo client e test pytest | Demo locale, runbook e report M2; Docker solo dopo prova compatibilità |
| U17 | Agente: strumenti, stato, limiti e condizione di arresto | Ciclo esplicito search/read, validazione degli argomenti | Pilota tool calling, senza shell arbitraria o annotazioni attese |
| U18 | Ricerca agentica nei file | Elenco, ricerca testuale, lettura di file/range, cronologia | E10: agente vs ricerca fissa su identica collezione controllata |
| U19 | Wiki: fonti immutabili, pagine derivate, collegamenti e provenienza | Ingestione, indice, verifica riferimenti, versione delle pagine | Costruzione da sole fonti, senza domande/risposte del test |
| U20 | Confronto wiki / file originali / RAG fisso | Stessi strumenti per agente raw/wiki; limiti e misure condivisi | E11: qualità, costo iniziale e per domanda, numero di passi |
| U21 | Fonte modificata, sintesi vecchia, cache vecchia | Invalidazione e ricostruzione delle dipendenze | E12: modifica controllata, regressioni e report M3 |
| U22 | Fine-tuning: che cosa cambia nei pesi e quando conviene | Lettura di un esempio minimo; eventuale training separato | Decisione motivata; pratica opzionale con risorse e dati separati |

Unità e numeri identificano argomenti, non giornate. Si possono accorpare unità già comprese, mai marcarle completate senza evidenza. Ogni report include l'apporto di librerie e dataset oltre alle modifiche di Andrea.

## M1 — decisioni tecniche e matrice esperimenti

**Primo embedding:** `BAAI/bge-small-en-v1.5`, candidato scelto per iniziare, non migliore modello proclamato. Verificata scheda ufficiale; revisione esatta da bloccare all'acquisizione. Inglese, CPU, Sentence Transformers, normalizzazione e istruzione query coerenti con la scheda. Controllare i token reali: MTRAG e BGE non condividono necessariamente tokenizer/limiti.

**Reranker candidato:** `cross-encoder/ms-marco-MiniLM-L6-v2`, solo sul piccolo insieme di candidati. Nessuna scansione del corpus con cross-encoder. Il confronto base resta utilizzabile se il reranker è troppo lento.

| Run | Variante | Condizioni mantenute uguali |
|---|---|---|
| E01 | BM25 | Stesso corpus, query, split, cutoff |
| E02 | Embedding | Come E01; misurare indicizzazione separata |
| E03 | BM25 + embedding tramite RRF | Stesse classifiche; parametri fissati sullo sviluppo |
| E04 | Migliore retrieval di sviluppo + reranker | Stessi candidati confrontati prima/dopo |
| E05 | Ultima domanda vs storia delle domande | Retrieval fissato; nessuna riscrittura gold gratuita |

Non fare tutte le combinazioni. Metriche primarie nDCG@10, Recall@5/@10; risultati per query e aggregati, tempi e memoria. Metodi deterministici non richiedono ripetizioni identiche per la qualità; ripetere quando serve misurare tempo o variabilità generativa. Split per conversazione; benchmark intero e sottoinsieme dichiarati distintamente. Conservare l'originale con passaggi ufficiali per comparabilità; E06 non altera retroattivamente E01–E05.

**Comprendere senza implementare tutto:** TF-IDF come confronto concettuale a BM25; sparse appresi/SPLADE, multi-vector/ColBERT, Matryoshka, ANN/HNSW e multilingua solo panoramica al momento opportuno. Embedding quantizzati int8/binari come estensione/fallback B, distinta da riduzione dimensionale e fine-tuning. Non tagliare coordinate arbitrariamente e chiamarlo Matryoshka.

## M2 — applicazione personale e RAG

**Utente e compito:** Andrea interroga e confronta risposte su una raccolta finanziaria, vede fonti e tracce e controlla se il sistema ha risposto usando informazioni sufficienti. Nessun cliente da trovare. Il flusso è una demo didattica personale; non assumere correttezza finanziaria attuale dalle vecchie fonti del benchmark.

**Pipeline:** domanda/storia → identificazione versione corpus → cache esatta valida → se miss, ricerca → eventuale reranking → selezione entro budget → generatore → risposta e riferimenti → controlli → traccia e cache. Gli embedding dei documenti sono già persistenti e non si ricalcolano a ogni domanda. La cache semantica, se sperimentata, è una diramazione esplicita, disattivabile.

**Generatore:** backend locale con interfaccia sostituibile; scegliere il modello quantizzato compatibile al momento U11 con al massimo due candidati piccoli, leggendo scheda/licenza e chat template. Non fissare oggi velocità o qualità su hardware non provato. `llama.cpp` è candidato runtime; l'eseguibile/ambiente Windows va verificato. Modello, quantizzazione, template, contesto e impostazioni diventano immutabili per il confronto. Tool calling verrà verificato separatamente prima di M3.

**E07:** sullo stesso piccolo campione preregistrato confrontare modello senza fonti, modello con fonti gold e modello con fonti recuperate; se il costo consente, confronto recupero semplice/variante selezionata. Partire da 12–20 task distribuiti per conversazione e rispondibilità, senza selezionarli dopo aver visto gli esiti; dichiarare il numero effettivo. Non è una misura dell'intero MTRAG. Risposte attese già disponibili non rendono automatica la valutazione di ogni parafrasi. Controlli formali automatizzati su tutto; leggere insieme 6–10 casi istruttivi; ampliare giudizio manuale/automatico solo per claim che lo richiedono. Non pubblicare un tasso complessivo di correttezza su risposte non valutate.

Le nuove domande spontanee di Andrea alimentano la demo e lo studio, non il test congelato. Nessun corpus annotato da costruire manualmente.

**Architetture:** provare RAG semplice, retrieval migliorato con budget e citazioni, e gestione della conversazione. Studiare parent-child, query rewriting/decomposizione, HyDE e GraphRAG come risposte a tipi di errore, senza renderle tutte implementazioni obbligatorie. Query rewriting può diventare un singolo esperimento opzionale se i dati mostrano un bisogno. La wiki di M3 non equivale automaticamente a GraphRAG.

**Applicazione:** moduli Python piccoli, CLI prima e poi API locale con client minimo; validazione, test utili, timeout, errori, log, ripresa e invalidazione. Nessuna dipendenza obbligatoria da Redis, cloud, Kubernetes o orchestratori. La forma del client si decide a U16 in base al flusso, senza una fase di design autonoma. Docker è un esercizio di distribuzione dopo il funzionamento locale, non condizione per imparare RAG.

## Caching — implementazione, limiti e verifiche

| Livello | Che cosa riusa | Risparmio effettivo | Politica |
|---|---|---|---|
| Embedding documenti | Vettori di testi già codificati | Codifica e indicizzazione | Persistente già in M1; hash testo + modello/revisione/preprocessing |
| Embedding query e risultati ricerca | Vettore query/classifica | Ricodifica o ricerca ripetuta | Chiave include input effettivo/storia, versione indice e configurazione |
| Risposta esatta | Risposta per lo stesso input operativo | Può saltare ricerca e generazione | M2; corpus/prompt/modello/storia/politica nel fingerprint, TTL e invalidazione |
| Risposta semantica | Risposta a una domanda considerata equivalente | Può evitare il resto della pipeline, ma richiede verifica della cache | Solo laboratorio E09; default off e nessuna soglia universale |
| Prefisso/KV nel runtime | Computazioni per token già elaborati | Tempo di elaborazione input, a seconda del runtime | Studio e misura se supportato; non riusa automaticamente una risposta e non elimina il contesto logico |

Usare inizialmente file/SQLite, non un nuovo servizio. Due domande semanticamente vicine possono chiedere condizioni diverse per prodotto, valuta, data o negazione. Non usare la sola similarità come prova di equivalenza. Le entità e i filtri rilevanti devono coincidere; richieste temporali/dinamiche escluse dalla cache semantica iniziale.

Per tutte le cache conservare provenance, versione e istante di creazione. Invalidazione conservativa globale per versione corpus all'inizio; invalidazione per dipendenze soltanto più avanti. Un TTL da solo non garantisce aggiornamento. Nessuna cache di errori/transitori inizialmente. Se in futuro ci fossero più utenti, includere identità/ambito autorizzato e controllo accessi; ora utente singolo.

**E08:** replay prefissato con domande uniche, ripetizioni identiche, stessa frase in storie diverse, corpus e prompt cambiati. Misurare hit rate, tempo freddo/caldo, chiamate evitate, token effettivamente elaborati se disponibili, risposte obsolete servite. Cache risposta off per il benchmark qualità; cache embedding ammessa perché non evita decisioni valutate. Le repliche generative non devono riciclare la stessa risposta.

**E09:** piccolo set didattico di parafrasi equivalenti e coppie ingannevoli (stesso prodotto/data diversa, negazione, numeri diversi), con pochi giudizi verificati insieme. Soglia scelta su sviluppo, prova separata; cache popolata cronologicamente dalle risposte precedenti, mai da risposte gold del test. Riportare hit corretti, falsi hit, falsi miss e campione. Nessuna percentuale generale di risparmio da un replay composto solo da duplicati.

## M3 — ricerca agentica e LLM Wiki nello stesso esperimento

MTRAG serve al recupero su passaggi; non attribuirgli una struttura documentale che non ha. **Collezione per M3:** selezione deterministica di circa 30 documenti sorgente FiQA, fissata indipendentemente dalle domande di test, conservando provenienza. Titoli/ID possono guidare ricerca e lettura; non inventare tassonomie usando le risposte. Se servono fonti con struttura naturale diversa, è una valutazione separata e va dichiarata.

Preparare 12–20 domande esplorative con evidenze disponibili, create dall'assistente o derivate da quelle ufficiali quando valide nel corpus ridotto. Andrea ne verifica solo un piccolo campione istruttivo. Nessuna conclusione quantitativa di correttezza complessiva senza giudizi sufficienti. Congelare sviluppo/test prima di ottimizzare le strategie; la selezione del corpus non dipende dai risultati. Dichiarare che non è il punteggio ufficiale MTRAG e che l'indipendenza del test interno è limitata.

Tre condizioni minime, **stesso corpus originario e stesso generatore**:

- **F — RAG fisso:** ricerca selezionata in M1, contesto preparato in una passata, risposta.
- **A — agente sui file:** `list_documents`, `search_text`, `read_document` con intervalli e limiti. Il modello sceglie cosa leggere e se cercare ancora. Niente shell generale, scrittura, web o annotazioni attese. Un'esperienza successiva può aggiungere `semantic_search`, ma non contaminare il primo confronto cambiando strumenti a metà.
- **W — agente sulla wiki:** stessi tipi di strumenti e modello di A; cambia la rappresentazione navigata, costruita dalle sole fonti. Eventuale accesso agli originali uguale ed esplicito nella configurazione. Pagine di sintesi/concetti, indice e link a ID/versioni delle fonti. Costruzione senza domande, risposte attese o errori del test. Wiki congelata durante il test, niente apprendimento dalle domande precedenti.

A vs F confronta strategie end-to-end, includendo le differenze dei retriever. W vs A isola meglio l'effetto della rappresentazione. Non attribuire a una singola causa la differenza di due pipeline che cambiano più componenti. Le pagine wiki sono derivate e non possono essere trattate come nuove fonti indipendenti.

Limiti pilota proposti: massimo 4 cicli modello/strumenti, 8 chiamate a strumenti, 5 minuti per task; stesso limite di output, tetto di contesto coerente con il modello, budget totale registrato. Controllare argomenti, file accessibili e limiti fuori dal prompt. Se il modello non produce tool call valide, provare al massimo una correzione/modello alternativo; poi ridurre a dimostrazione didattica, senza inventare una valutazione riuscita.

**E10/E11:** confronto sulle stesse domande, cache risposte off; misurare riferimenti recuperati, supporto delle risposte valutate, passi, token/call e tempo. Per W aggiungere compilazione e manutenzione; non fingere che tutto il costo sia quello della query. Un eventuale pareggio economico dipende dal numero di domande e dai costi osservati, e può non esistere.

**E12:** introdurre una modifica controllata a una fonte didattica/versione di test, etichettata come tale senza alterare il dataset ufficiale. Verificare aggiornamento indice, cache e pagine wiki dipendenti; interrogare la nuova versione e controllare che i riferimenti non puntino a una sintesi superata. Conservare i due snapshot. La prova riguarda manutenzione su scala piccola, non un sistema di conoscenza enterprise.

## Fine-tuning e fallback

Lo studio concettuale U22 è incluso. L'addestramento reale è una mini-estensione separata se resta un problema di rappresentazione e ci sono esempi di training indipendenti. Prima: motivazione, corpus/split di training, criterio di successo, prova CPU di pochi passi, limite dedicato. Dopo: confronto col modello base su dati non usati nel training e controllo di regressione. Se non sostenibile, segnare «studiato, non eseguito», senza attribuire esperienza pratica.

Il fallback B non diventa un quarto grande progetto: riusa M1 per misurare quantizzazione o un modello più leggero, eventualmente su un dataset piccolo diverso. La compressione dell'indice non risolve da sola la lentezza del generatore o dell'encoder. Conservare i risultati già utili e rivedere solo il modulo bloccato.

## Documentazione e file: una sola fonte per ogni cosa

Piano corrente qui; stato e comprensione in HANDOVER; lezioni in percorso; esperimenti in esperimenti; misure in runs. Non duplicare la tabella del piano in tre file.

Per ogni unità creare una nota al momento di affrontarla: problema, concetti, funzione esaminata, esercizio di Andrea, risultato, link alla run e dubbio residuo. Ogni esperimento usa `esperimenti/TEMPLATE.md`. I file delle run seguono `docs/DOCUMENTAZIONE.md`; includere stato cache, storia, versione corpus, modello, budget e tempi.

Per ogni macroprogetto, alla chiusura: `report/m1-ricerca/report.md`, `report/m2-rag/report.md`, `report/m3-agent-wiki/report.md` con figure proprie e breve consegna. `report/report.md` sarà l'indice/sintesi dei risultati disponibili e `report/CONSEGNA.md` il punto unico per il sito. Creare questi file solo quando esistono risultati; niente scaffolding di report pieni di placeholder.

Figure previste: U02 geometria didattica; M1 una domanda/tre classifiche e qualità-tempo; M2 due punti d'errore e cache con invalidazione; M3 lavoro prima/durante/dopo la domanda e risultato dopo aggiornamento. SVG o grafici generati dai dati, Mermaid per schemi, sempre fonte e didascalia. Due o tre figure informative per report possono bastare. Conservare sorgenti, controllare rendering e leggibilità prima della consegna. Nessun grafico numerico inventato.

**Completamento:** ogni macroprogetto funziona nel perimetro dichiarato, ha risultati ricostruibili e Andrea sa spiegare e modificare i componenti principali. Feedback esterno, hosting pubblico, submission e fine-tuning non sono prerequisiti. Nessun contatto esterno da organizzare.

## KodeKloud: supporto mirato

Vedere `percorso/KODEKLOUD.md`. I laboratori servono per le basi e per esercitarsi senza copiare la soluzione di SOL. Nessun corso intero obbligatorio prima di cominciare; nessun abbonamento. Le note pubbliche AI sono riferimenti, non prova di accesso gratuito ai laboratori del corso completo.

## Decisioni lasciate alla misura, non all'improvvisazione

Prima U04: conteggi e split effettivi, licenze/revisioni; prima U11: modello generativo e contesto; prima U16: client e compatibilità Docker; prima U18: pilota tool calling e selezione corpus. Ogni scelta ha un punto preciso nel percorso; non servono ora date o prestazioni inventate. Restano i guardrail CPU di `docs/ARCHITETTURA.md`.

Fonti tecniche consultate il 11 settembre 2026: [BGE small](https://huggingface.co/BAAI/bge-small-en-v1.5), [MiniLM reranker](https://huggingface.co/cross-encoder/ms-marco-MiniLM-L6-v2), [retrieve/rerank](https://www.sbert.net/examples/sentence_transformer/applications/retrieve_rerank/README.html), [semantic cache](https://redis.io/docs/latest/develop/use-cases/semantic-cache/), [llama.cpp](https://github.com/ggml-org/llama.cpp). Le matrici, soglie e campioni sono scelte progettuali nostre; nessuna dipendenza è stata installata.
