# Competenze da costruire e rendere verificabili

> **Revisione del 12 settembre:** prevale il nuovo PIANO_MASTER. Le prove consistono nel scegliere, configurare, usare e valutare sistemi, anche con implementazione affidata all'IA. Gli esercizi C01/C02 di programmazione di base non sono più requisiti; leggere/modificare il codice rilevante al design resta utile. Chroma/API/Redis/Docker sono estensioni e non ritardano il confronto RAG/agente/wiki. La matrice precedente sotto è un catalogo, non un contratto di esami da superare.

11 settembre 2026. Estensione didattica del [PIANO_MASTER](../PIANO_MASTER.md). Nessuna nuova competenza del percorso RAG è già acquisita per il solo fatto di essere elencata qui. Il mockup può mostrarla come prevista. La verifica della comprensione vive in HANDOVER; questo documento definisce i criteri, non duplica gli stati personali.

## Che cosa significa «so usarlo»

Tre formulazioni pubbliche, senza stelline o percentuali di padronanza:

- **Studiato:** Andrea sa spiegare problema, meccanismo e limite con un esempio. Non equivale a esperienza pratica.
- **Usato nel progetto:** esiste una parte eseguita del sistema con quello strumento; Andrea ne comprende input/output e sa indicare una scelta. Specificare l'ambito, per esempio «inferenza locale».
- **Competenza dimostrata nel progetto:** oltre all'uso, Andrea modifica un comportamento, diagnostica almeno un errore e verifica la correzione in un caso nuovo. La prova è collegata a codice/run/report.

Non pretendere memoria delle firme delle API. Consentire documentazione e suggerimenti graduali; per la verifica finale Andrea deve formulare il piano e interpretare l'esito prima che SOL consegni la soluzione. Se SOL scrive il codice, registrare il contributo assistito; non chiamare autonomia il copia/incolla riuscito.

Una futura dichiarazione «ho esperienza con X» resta circoscritta al lavoro svolto. Inferenza con Sentence Transformers non è addestramento in PyTorch; endpoint locale non è gestione di un servizio cloud; tool calling non è automaticamente MCP.

## Matrice delle competenze

| ID / unità | Competenza finale prevista | Verifica di trasferimento, breve e concreta | Evidenza da conservare | Formula pubblica dopo la prova |
|---|---|---|---|---|
| C01 / U01–U04 | Python per pipeline dati | Aggiungere un campo al loader; gestire record invalido senza perdere gli altri | Modifica + caso valido/invalido + spiegazione | Pipeline Python per acquisizione e validazione di dati testuali |
| C02 / U02–U03 | Vettori e inferenza embedding | Spiegare la forma di una matrice; prevedere l'effetto della normalizzazione; trovare un testo troncato | Esempio NumPy e tokenizer con esito | Embedding testuali con Sentence Transformers e ricerca vettoriale con NumPy |
| C03 / U05–U08 | Recupero e ranking | Scegliere un caso lessicale e uno semantico; spiegare RRF; riordinare solo i candidati | E01–E04, ranking e costi | Confronto BM25, ricerca semantica, fusione RRF e reranking |
| C04 / U04–U09 | Valutazione retrieval | Calcolare Recall su tre risultati; individuare leakage fra turni della stessa conversazione | Split congelato, metriche, esempio manuale | Valutazione del recupero con Recall/nDCG e analisi degli errori |
| C05 / U09–U10 | Contesto e segmentazione | Risolvere un riferimento nella conversazione; spiegare perché cambiare chunk rompe qrels esistenti | E05–E06 separati | Gestione del contesto conversazionale e studio della segmentazione |
| C06 / U10b | Database vettoriale locale | Salvare/riaprire la collezione, filtrare, aggiornare ed eliminare un documento | L01 con prima/dopo, stesso encoder e ID | Uso locale di Chroma per persistenza, filtri e ciclo di vita dei documenti |
| C07 / U11–U13 | RAG e diagnosi generativa | Spiegare una risposta sbagliata con fonti giuste e una con recupero sbagliato | E07, tracce e casi verificati | RAG con fonti, controllo delle citazioni e diagnosi recupero/generazione |
| C08 / U14–U15 | Cache e invalidazione | Stessa domanda, storia diversa; cambio fonte; parafrasi ingannevole | E08–E09 e risposta obsoleta impedita | Cache versionate e valutazione del riuso delle risposte |
| C09 / U16 | API e contratti | Aggiungere un campo alla risposta; rifiutare input invalido; gestire timeout del modello | Contratti, test integrazione e runbook | Servizio RAG locale con FastAPI, Pydantic e test di integrazione |
| C10 / trasversale | Riproducibilità e debugging | Ripartire da una run fallita senza mescolare versioni; ricostruire un numero | Manifest, log, config e figura derivata | Esperimenti riproducibili, versionamento e analisi delle tracce |
| C11 / U17–U18 | Ciclo agente e strumenti | Gestire argomento invalido e file non consentito; fermare ciclo ripetitivo | E10, tool call e arresto verificato | Progettazione di strumenti limitati e agenti documentali con budget di esecuzione |
| C12 / U19–U21 | Conoscenza derivata e aggiornamenti | Risalire dalla sintesi alla fonte; aggiornare una pagina dipendente da una fonte cambiata | E11–E12, provenienza e snapshot | Confronto tra ricerca agentica e wiki con provenienza e costi di manutenzione |
| C13 / U22 | Scelta fra retrieval e fine-tuning | Distinguere problema di conoscenza, comportamento e rappresentazione con tre esempi | Nota ragionata; eventuale training separato | Conoscenza dei criteri di scelta del fine-tuning; nessuna esperienza pratica se non eseguito |

Ogni verifica può richiedere pochi casi ben scelti. Non è un nuovo benchmark da annotare a mano. Gli esiti servono a verificare comprensione, non a certificare un livello professionale universale.

## Stack deciso e ruolo dei nomi

| Strumento o modello | Decisione | Uso esplicito nel percorso | Cosa evitare nel CV |
|---|---|---|---|
| Python | Centrale | File/JSONL, funzioni, typing, errori, moduli, API | «Esperto Python» da una sola pipeline |
| NumPy | Centrale M1 | Matrici, normalizzazione, ricerca esatta e top-k | Confondere un indice esatto con un database vettoriale |
| Sentence Transformers (`sentence_transformers`) | Centrale M1 | `SentenceTransformer`, codifica query/documenti, batch e `CrossEncoder` | Dichiarare training solo perché la libreria lo supporta |
| `BAAI/bge-small-en-v1.5` | Candidato iniziale | Encoder inglese; fissare revisione, istruzione query e troncamento | Trattarlo come linguaggio/libreria o come modello già adottato |
| `sentence-transformers/all-MiniLM-L6-v2` | Alternativa, non secondo modello obbligatorio | Eventuale prova didattica/fallback dopo verifica token e CPU | Due badge per simulare due competenze distinte; presumere che sia più veloce senza misurarlo |
| `cross-encoder/ms-marco-MiniLM-L6-v2` | Candidato reranker | Valutare coppie query/passaggio sui top 20 | Confonderlo con all-MiniLM-L6-v2: compito diverso |
| Implementazione BM25 | Una sola da fissare a U05 | Prima esempio piccolo, poi libreria adatta e verificata | Confondere algoritmo e pacchetto o scegliere tre librerie equivalenti |
| Chroma | Laboratorio programmato U10b | Persistenza, metadati, filtri, query con vettori già prodotti, upsert/delete | Esperienza distribuita/enterprise; confronti che cambiano anche encoder |
| FastAPI + Pydantic | Scelta per U16 | Endpoint, input/output, errori, schema e documentazione API | Far coincidere demo e produzione |
| SQLite (`sqlite3`) | Base M2 | Cache esatta, chiavi versionate, query parametrizzate, transazioni semplici | «Database engineering» o SQL avanzato senza altro lavoro |
| pytest | Verifiche mirate | Integrità dati, ranking didattico, invalidazione, contratto API e limiti strumenti | Test che ripetono il codice senza catturare un errore significativo |
| Git + uv | Processo di lavoro | Diff/commit ragionati, ambiente e versioni riproducibili | Considerare presenza nel computer prova d'uso |
| Matplotlib | Figure dai risultati | Classifiche, qualità/tempo, annotazioni e denominatori | Pubblicare proiezioni embedding come misura di retrieval |
| llama.cpp | Runtime candidato U11 | Inferenza locale, modello/revisione, contesto e tempi | Training o deployment GPU |
| Redis + redis-py | Estensione facoltativa L02 | Sostituire la sola cache esatta, TTL, invalidazione, indisponibilità | Redis dichiarato perché abbiamo implementato caching in SQLite |
| Docker | Estensione U16, dopo prova locale | Immagine, avvio e persistenza con risorse misurate | Kubernetes, CI/CD o esperienza cloud impliciti |
| Transformers / PyTorch | Dipendenze possibili, non badge automatici | Aggiungere solo se si interagisce direttamente con tokenizer/tensori e si completa un esercizio | Presumere padronanza perché installati transitivamente |
| LangChain / LangGraph / LlamaIndex | Nessuna dipendenza richiesta ora | Eventuale refactoring circoscritto futuro se risolve un bisogno osservato | Collezionare framework per parole chiave |

Non installare questo elenco in anticipo. Si acquisisce uno strumento quando serve e dopo controllo di compatibilità (in particolare Python presente, librerie ML e Chroma). Ogni sostituzione va motivata nella scheda relativa. Non alterare un confronto già congelato per allinearlo a un marchio.

## L01 / U10b — Chroma senza ricominciare il progetto

Si inserisce dopo la consegna retrieval M1, all'ingresso di M2. È un laboratorio d'integrazione, non il nuovo benchmark principale. Parti da un campione già indicizzato; non generare nuovi embedding.

1. Definire l'operazione del retriever (query, filtri, k → ID/punteggi) usando il codice esistente; astrazione minima solo ora che esistono due implementazioni.
2. Creare collezione persistente locale, ID e metadati, distanza coerente. Passare gli embedding esplicitamente, evitando un modello di default diverso.
3. Chiudere e riaprire; verificare il recupero e il filtro su metadati realmente presenti, con stesso sottoinsieme filtrato nel riferimento NumPy.
4. Aggiornare e cancellare un record; verificare che non rimanga una versione vecchia. Mantenere cache/indice coerenti.
5. Confrontare alcuni top-k con il riferimento esatto, senza pretendere identità se l'indice usa ricerca approssimata; dichiarare metrica e pareggi. Questo è un controllo d'integrazione, non la prova che Chroma migliori la pertinenza.
6. Salvare una scheda L01 (config, esiti, limiti) e un esercizio svolto da Andrea. Se l'ambiente richiede lavoro sproporzionato, rinviare e non promuovere la competenza.

L01 non impedisce di chiudere M1. L'adattatore può restare una variante; non obbliga a sostituire il percorso più semplice in M2.

## L02 — Redis, solo dopo aver capito la cache

Dopo E08 e, se disponibile, ambiente locale compatibile. Nessun servizio a pagamento. Ricontrollare installazione/licenza/versione per Windows e non improvvisare dipendenze globali. Docker/WSL solo se già disponibili o introdotti con uno scopo sostenibile.

Una sola sostituzione: cache esatta SQLite → Redis, stesso contratto, stesso replay, stesso modello. Capire connessione, serializzazione, chiavi, TTL, cancellazione/versione e cosa succede se la cache non risponde. Il sistema deve poter proseguire con cache indisponibile; non conservare credenziali nelle run. Registrare tempi e costi del servizio e non dedurre miglioramenti da un singolo replay.

La cache semantica resta E09: non è necessario implementarla anche in Redis. Non imporre un secondo database vettoriale. Se L02 non viene svolto, Redis compare soltanto negli argomenti studiati, non nella lista di strumenti usati.

## Come SOL conduce la verifica

Alla fine di un blocco: una spiegazione di Andrea, una piccola variazione di requisito e un errore da diagnosticare. Massimo pochi casi. Registrare: ID competenza, file/run, modifica proposta da Andrea, aiuto ricevuto, cosa sa spiegare, prossimo dubbio. HANDOVER rimane l'unico stato corrente; le note contengono gli esempi.

Domande da saper discutere a fine percorso, senza imparare risposte a memoria:

1. Perché la similarità non prova che un documento risponda alla domanda?
2. Quando BM25 può battere gli embedding?
3. Perché RRF usa posizioni? Cosa compra il reranker con il calcolo aggiuntivo?
4. Come scegli un modello rispetto a lunghezza dei testi, qualità e CPU?
5. Come separi un errore di retrieval da un errore del generatore?
6. Che differenza c'è fra indice, database vettoriale e cache?
7. Quando una cache serve una risposta sbagliata nonostante un hit corretto sulla stringa?
8. Quali controlli dell'agente devono vivere nel codice, fuori dal prompt?
9. Quando la wiki può costare più del RAG e come risali alla fonte?
10. Che cosa manca per passare dal nostro servizio locale a un sistema aziendale?

L'ultima risposta deve distinguere lavoro svolto e futuro: autenticazione/autorizzazione, dati reali, carico, gestione segreti, monitoraggio in esercizio, procedure di rilascio e recupero non diventano competenze pratiche automaticamente.

## Sintesi pubblica futura, da adattare ai risultati

«Ho costruito e valutato un assistente documentale locale: ricerca lessicale e semantica, risposte con fonti, cache versionate e API. Ho confrontato le strategie conservando configurazioni, tracce e limiti.» Usare questa frase solo dopo M1 e M2 effettivamente completati; aggiungere agenti/wiki dopo M3. Inserire numeri reali con perimetro, non promesse. I nomi dei checkpoint stanno nel report tecnico, quelli degli strumenti principali nella scheda.

## Fonti ufficiali consultate

11 settembre 2026. Le scelte didattiche sono nostre, non raccomandazioni universali delle fonti.

- [Sentence Transformers](https://www.sbert.net/docs/sentence_transformer/usage/usage.html) e [CrossEncoder](https://www.sbert.net/docs/package_reference/cross_encoder/model.html).
- [BGE-small-en-v1.5](https://huggingface.co/BAAI/bge-small-en-v1.5) e [all-MiniLM-L6-v2](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2): modelli distinti, non librerie; verificare schede al momento dell'uso.
- [Chroma: query ed embedding espliciti](https://docs.trychroma.com/docs/querying-collections/query-and-get).
- [Redis: semantic cache](https://redis.io/docs/latest/develop/use-cases/semantic-cache/).
- [FastAPI](https://fastapi.tiangolo.com/) e [Pydantic](https://docs.pydantic.dev/latest/).
