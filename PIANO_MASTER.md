# Progetto — scegliere e valutare sistemi di conoscenza finanziaria

**Revisione del 12 settembre 2026 dopo la correzione di Andrea.** Questo è il piano corrente. Sostituisce il percorso obbligatorio U00–U22: quelle unità restano riferimenti consultabili, non prerequisiti né esami da superare. Versione precedente conservata in docs/archivio/PIANO_MASTER-2026-09-11.md.

## Obiettivo e responsabilità

Dimostrare capacità di progettare, usare e confrontare embedding, retrieval, RAG e strategie agentiche su un caso finanziario realistico. Andrea decide requisiti, alternative, criteri e interpretazione; l'assistente prepara implementazione, dati, esecuzioni e grafici. Andrea deve capire il codice che cambia il comportamento del sistema, non scrivere loader o dimostrare sintassi Python.

Il codice pertinente: testo inviato all'encoder, metadati/filtri, chunk, configurazione della ricerca, fusione, contesto del generatore, chiave di cache, strumenti e condizioni di arresto. Il codice di servizio viene realizzato e verificato dall'IA; se un suo dettaglio compromette il risultato, l'assistente ne esplicita la conseguenza. Delegare implementazione non delega la responsabilità di verificare un sistema.

U00 conclusa; U01 affrontata con esercizio presente ma non formalmente confermata. La conferma non blocca nulla. Dubbio U02 aperto: spiegarlo soltanto quando utile al confronto, non come esame di NumPy.

## Caso applicativo e dati

**Assistente di consultazione per informazioni finanziarie:** cercare evidenze, distinguere prodotti/condizioni, rispondere citando le fonti, riconoscere informazioni mancanti o superate. Uso personale di Andrea, nessun cliente da trovare. Non trading, non raccomandazioni finanziarie attuali.

Due insiemi con ruoli diversi:
- **MTRAG FiQA:** benchmark con annotazioni esistenti per misurare retrieval e conversazione senza etichettatura massiva. Congelare revisione, passaggi, ID, qrels e split per conversazione. Non presumere che contenga versioni di policy, tabelle o casi sufficienti per ogni categoria.
- **Mini-dossier applicativo controllato**, soltanto dove FiQA non copre il fenomeno: circa 12–20 brevi documenti in inglese con condizioni, date, eccezioni e rimandi; fonti finanziarie sintetiche, esplicitamente simulate. L'assistente prepara corpus e casi con riferimenti; Andrea legge pochi casi istruttivi. Non chiamarlo benchmark indipendente, esperienza cliente o valutazione di correttezza finanziaria reale. Se si usano documenti reali, verificare prima licenze/provenienza e non mescolare la valutazione con FiQA.

Il mini-dossier sarà condiviso da RAG, agent file search e wiki; selezione e domande congelate prima dell'ottimizzazione. Serve a isolare effetti di design; non dimostra prestazioni generali. I giudizi creati dall'IA non diventano verità solo perché sono automatici.

## Metodo di lavoro per ogni decisione

1. Mostrare una domanda realistica e il problema che pone.
2. Spiegare due o tre alternative pertinenti, i meccanismi e i compromessi con uno schema quando utile.
3. Andrea formula una scelta e cosa si aspetta di osservare, senza quiz sulla sintassi.
4. L'assistente implementa/esegue il confronto; mostra solo il codice/configurazione che materializza la scelta.
5. Leggere risultati e pochi errori, distinguendo osservazione e spiegazione ipotizzata.
6. Conservare una scheda breve: requisito → alternative → previsione → confronto → decisione → limite.

Non assegnare esercizi Python/JSONL o corsi introduttivi come prerequisiti. KodeKloud solo se Andrea chiede un approfondimento utile a una lacuna incontrata. Non generare un trattato in anticipo: insegnare dentro gli esperimenti. Non costruire tutte le varianti senza coinvolgere Andrea nelle decisioni; infrastruttura e baseline possono essere preparate autonomamente.

## Sequenza effettiva

M1/M2/M3 restano capitoli della stessa repository e di un solo progetto pubblico. Le fasi D identificano decisioni, non nuove lezioni da completare meccanicamente. Gli ID E01–E12 precedenti si conservano se già usati; per nuove prove collegare la scheda al D corrispondente senza riusare un ID per un altro esperimento.

| Fase | Decisione da imparare | Prova concreta | Chiusura |
|---|---|---|---|
| D1 / M1 | Quale embedding e quale tipo di ricerca per il compito? | Due encoder piccoli, BM25, confronto query/classifiche e mappa 2D | Scelta motivata sullo sviluppo, costi e casi di errore |
| D2 / M1 | Quale unità indicizzare e come combinare le evidenze? | Chunking nel dossier, metadati, ibrida e reranking mirati | Configurazione retrieval scelta, report M1 breve |
| D3 / M2 essenziale | Come trasformare le fonti in risposte? | RAG base vs retrieval selezionato; una variante per conversazione/decomposizione se pertinente | Baseline generativa, citazioni e diagnosi degli errori |
| D4 / M3 | Quando convengono agente sui file e wiki? | Stesso dossier e generatore: RAG fisso / agente raw / agente wiki | Confronto end-to-end, costruzione e aggiornamenti inclusi |
| D5 / M2–M3 | Che cosa conviene conservare e quando invalidarlo? | Cache esatta/versionata, semantica circoscritta, fonte cambiata | Politica cache motivata, risparmio e falsi riusi |
| D6 / estensione | Serve modificare i pesi? | Diagnosi residuale, dati training/test separati, pilot se sostenibile | Decisione fine-tuning vs alternative, pratica soltanto se eseguita |

**API, Docker, Chroma e Redis non sono cancelli prima di D4.** Si aggiungono dopo come integrazioni utili, oppure si omettono senza impedirci di documentare il confronto. La consegna M2 può essere inizialmente una pipeline locale; il servizio API è un'estensione successiva. Nessuna attesa di report editoriale perfetto per iniziare il confronto seguente: bastano esecuzioni conservate e una decisione comprensibile.

## D1 — due encoder, non due nomi messi a caso

Confronto iniziale proposto:
- `sentence-transformers/all-MiniLM-L6-v2`: encoder per frasi/paragrafi brevi, utile come baseline di similarità.
- `BAAI/bge-small-en-v1.5`: candidato orientato anche al recupero query/passaggio, con configurazione query documentata.

Entrambi producono vettori densi di 384 dimensioni. Sono due modelli preaddestrati con scelte di training/pooling e limiti diversi, **non due famiglie architetturali radicalmente diverse** e non modelli addestrati da noi sul dominio finanziario. Le differenze osservate non possono essere attribuite al solo pooling. Non sostituire il pooling a piacere ignorando il training.

Decisione applicativa: basta una rappresentazione di similarità generale per le nostre domande, oppure l'altro encoder recupera evidenze migliori? Confrontare anche BM25: per codici o termini distintivi la similarità semantica non è necessariamente sufficiente. Nessuna vittoria presunta.

Prima dei download completi: campione CPU, spazio, lunghezza e costo di ciascun modello. Usare gli stessi passaggi ufficiali, documentare i token persi per ciascuno; eventuale analisi su testi che rientrano nei limiti di entrambi è separata, non sostituisce silenziosamente il test. Cache embedding versionata. Se due encoder sul corpus sono sproporzionati, usare un corpus delimitato dichiarato, non nascondere la riduzione.

Tipi di query da esplorare: parafrasi; termini/identificatori precisi; numeri, negazioni ed eccezioni; riferimenti conversazionali. Gruppi descritti con regole fissate prima dei risultati e denominatori espliciti; categorie piccole restano analisi di casi, non percentuali affidabili per settore.

### Figura 2D

Prevista come strumento di esplorazione, preferibilmente interattiva; export statico nel report. Prima mostrare domanda, top-k e testo delle fonti: la figura deve spiegare un comportamento concreto, non essere la prima e unica evidenza.

Stessi ID di documenti e query, colori/temi assegnati indipendentemente dal risultato, proiezione UMAP riproducibile per ogni modello (parametri/seed registrati). Due mappe indipendenti non hanno assi/direzioni/distanze confrontabili: non dedurre miglioramento da uno spostamento visivo. Non unire direttamente vettori di modelli diversi come se condividessero le coordinate. Per query nuove usare la trasformazione del riduttore già adattato al corpus, se supportata.

Mostrare i vicini calcolati nello **spazio originale**, selezione query/documento e relativo testo; accanto Recall/nDCG disponibili, latenza e memoria. UMAP può distorcere vicinanze e separazioni: cluster più belli non dimostrano retrieval migliore. Un documento può essere dello stesso tema e non rispondere alla domanda. 3D solo se risolve un limite reale della lettura 2D, non come requisito.

## D2 — design della rappresentazione e del recupero

Tre piani da distinguere:
1. **Rappresentazione:** densa, lessicale/sparsa, multi-vettore; lingua, compito di training, dimensioni e lunghezza supportata.
2. **Che cosa rappresentiamo:** passaggio corto, sezione, testo con titolo/metadati; conservazione di eccezioni e contesto.
3. **Come cerchiamo:** ricerca esatta/approssimata, filtri, ibrida e reranking. Un database è implementazione di persistenza/ricerca, non una nuova capacità semantica.

Implementazione principale limitata:
- Sul benchmark: BM25, i due encoder; poi RRF con l'encoder selezionato e confronto con/senza cross-encoder sui top 20. Non fare tutte le combinazioni.
- Nel dossier: due segmentazioni (fissa vs confini di sezione, con budget documentato); test di domanda che richiede un'eccezione nello stesso contesto. Metadati prodotto/versione con filtri prima del ranking; il test deve specificare la versione richiesta.
- Multi-vettore/ColBERT, sparse learned/SPLADE, multilingual, quantizzazione/Matryoshka: spiegare quale problema risolvono. Pilot aggiuntivo solo se un errore o vincolo misurato lo giustifica, non benchmark obbligatorio di ogni tecnologia.

Codice utile ad Andrea: encoder/config, costruzione del testo del chunk, filtro e fusione. Loader, serializzazione e batch plumbing li prepara l'assistente. Recall/nDCG misurano recupero, non correttezza generativa; confrontare qualità e risorse, non un unico punteggio globale.

## D3 — confrontare RAG con uno scopo

Pipeline base: domanda → retrieval → contesto → risposta con fonti.
Prima distinguere fallimento della ricerca, contesto insufficiente e interpretazione/generazione. Pilot locale 3–5 richieste prima di campagne; massimo due generatori candidati, nessuna API a pagamento.

Minimo:
- RAG con retrieval semplice vs retrieval selezionato in D2, stesso generatore/prompt/budget di contesto.
- Controllo con fonti gold su casi annotati: diagnostico, mai presentato come sistema autonomo.
- Una variante adattiva se il compito la richiede: riscrittura della query usando la storia **oppure** decomposizione di domanda multi-documento. Misurare anche il suo costo; nessuna dipendenza da risposte gold nel prompt.
- Sul mini-dossier, se entra nel contesto a costo sostenibile, tutto il dossier come baseline: può mostrare che il retrieval non conviene a quella scala.

Non dire «ranking migliore del RAG» senza distinguere classifica dei passaggi e qualità della risposta. Registrare supporto delle affermazioni, citazioni, astensione, tempi/chiamate e casi valutati. Dataset piccoli e controlli manuali limitati comportano conclusioni limitate. HyDE, parent-child e GraphRAG vengono spiegati rispetto ai problemi; non installati per collezionare nomi.

## D4 — agente sui file e wiki subito dopo la baseline

Stesso mini-dossier, stesso generatore, stesse domande e limiti:
- F: RAG selezionato, ricerca prestabilita.
- A: agente con list/search_text/read, può iterare e deve fermarsi entro limiti applicati dal codice.
- W: stesso tipo di agente e strumenti su wiki compilata dalle sole fonti con provenienza; nessun accesso alle domande/risposte test in costruzione.

Wiki: indice, pagine sintetiche, link alle fonti originali/versioni. Compilazione non significa fine-tuning. È una strategia di preparazione del contesto, non un algoritmo unico universalmente definito. Eventuale accesso alle fonti originali esplicito ed equivalente fra agenti.

F vs A confronta sistemi completi, non isola l'effetto puro dell'agency. A vs W mantiene più componenti costanti e valuta la rappresentazione. Includere costo di compilazione/manutenzione, limiti di tool call, token e latenza; valutare una fonte cambiata. Non aspettarsi un vincitore universale: l'esito può favorire strategie diverse per compiti diversi.

Se il generatore locale non usa correttamente strumenti, separare limite del modello da valore dell'architettura; ridurre la dimostrazione e dichiararlo, senza fingere un confronto conclusivo.

## D5 — cache come decisione applicativa

Cache di embedding già necessaria a D1; più avanti risultati di ricerca e risposte. Chiavi con domanda/storia/config/corpus; invalidazione quando cambiano fonti o prompt. Scegliere cosa riusare in funzione di ripetizioni e aggiornamenti.

Replay con richieste uniche, ripetizioni, stessa frase in contesti diversi e fonte modificata. Misurare risparmio reale e risposte obsolete. Cache semantica separata e spenta per default: parafrasi vs domande simili ma non equivalenti per data/prodotto/negazione. Soglia su sviluppo; niente risposte gold precaricate. Cache risposta disattivata nel confronto di qualità fra architetture.

File/SQLite bastano al primo confronto; Redis è un adattatore eventuale, non la lezione centrale. Distinguere riuso di risposta, riuso di embedding e riuso del prefisso nel runtime. Non promettere risparmio di token per ogni cache.

## D6 — fine-tuning con un'ipotesi

Discutere già in D1 quando può servire; non rimandare il concetto a fine corso. Pratica dopo diagnosi: il problema è conoscenza aggiornata, rappresentazione del dominio o comportamento/formato del generatore? Retrieval, preprocessing, reranker, prompting e training risolvono problemi differenti.

Opzione pertinente: adattamento dell'encoder con coppie/triplette e negativi difficili, se esistono dati indipendenti e il pilot CPU lo consente. Nessuna promessa di training su questo portatile, niente cloud pagato. Separare training/dev/test per fonte/conversazione e registrare regressioni. Se resta solo studio, dichiarare conoscenza dei criteri, non esperienza pratica di training.

## Evidenza professionale e documentazione

La verifica non è scrivere una funzione a memoria. Andrea sa: formulare requisito; scegliere alternative; definire prova e costo accettabile; interpretare un fallimento; indicare la configurazione/codice che cambia il comportamento; spiegare limiti e quando sceglierebbe diversamente.

Output per confronto: config e run generate automaticamente; scheda decisione breve; uno o due casi commentati; figura se aiuta. Report principale: problema applicativo → matrice delle scelte → confronti → errori → decisione per contesto → limiti. M1/M2/M3 sezioni dello stesso progetto; niente nuova documentazione duplicata per ogni tool.

Un esito negativo è utile se la decisione è sostenuta dalle prove. Non dichiarare produzione, autonomia di implementazione, training o risultati mai verificati. I testi già pubblicati del sito non sono modificati in questa revisione.

## Prossima sessione

**D1, non U01/U02.** Aprire con quattro esempi di richieste finanziarie (parafrasi, identificatore, condizione/numero, follow-up); spiegare cosa dovrebbe recuperare un sistema e perché non basta la vicinanza di tema. Presentare confronto MiniLM/BGE/BM25, decidere ipotesi con Andrea e preparare il primo pilot. L'assistente gestisce acquisizione e infrastruttura; mostra configurazioni e risultati, non un esercizio di file reading.

## Fonti verificate il 12 settembre

[MiniLM](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2), [BGE](https://huggingface.co/BAAI/bge-small-en-v1.5), [retrieve/rerank](https://www.sbert.net/examples/sentence_transformer/applications/retrieve_rerank/README.html), [limiti UMAP](https://umap-learn.readthedocs.io/en/latest/faq.html). Modelli e tool da validare nell'ambiente prima dell'uso; nessun nuovo esperimento eseguito in questa revisione.
