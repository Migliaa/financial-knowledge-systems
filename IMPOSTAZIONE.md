# RAG — proposta di progetto e percorso

> **Superata come raccomandazione operativa dal successivo confronto con Andrea.** Leggere prima `SCELTA.md`: disponibilità di 5 ore al giorno, 24 GB di RAM, priorità a dati già annotati e minimo lavoro meccanico. La proposta SQLite, le 80 annotazioni manuali e la stima di 25–45 ore qui sotto non sono più il piano raccomandato. Questo file conserva il ragionamento iniziale e riferimenti metodologici riutilizzabili.

Stato al 10 settembre 2026: impostazione proposta, non progetto già approvato né implementato. Il dominio, il tempo disponibile e l'hardware restano da concordare. Budget operativo assunto: zero euro aggiuntivi. Questo documento integra il brief originale, senza modificarlo.

## Obiettivo

Costruire e saper spiegare un sistema che risponde consultando un archivio delimitato, mostra le fonti e riconosce quando le informazioni disponibili non bastano. Dimostrare con esperimenti quale metodo di ricerca conviene e quanto gli errori di recupero incidono sulle risposte.

Domanda proposta: **a modello generativo e budget di contesto fissi, quale metodo di recupero produce risposte meglio supportate dai documenti, e a quale costo in tempo e risorse?**

Il progetto copre RAG ed embedding. Il fine-tuning viene spiegato nel percorso, ma l'esperimento di addestramento è un'estensione condizionata. Non dichiareremo esperienza pratica di fine-tuning senza averlo eseguito e valutato.

## Cosa riprendere da τ²-bench

Letti il portfolio locale, la pagina `/progetti/tassonomia.html`, il report sorgente di tassonomia e report/consegna di metodo. Il riferimento editoriale è: problema concreto, domanda stretta, prima misura, diagnosi dei fallimenti, modifica motivata, risultato e limiti. Figure che spiegano una relazione, tracce come prove, approfondimenti separati. Il report di tassonomia è di circa 1.100 parole: qui puntare indicativamente a 1.200–1.600, senza farne un vincolo rigido.

Riprendere il metodo, adattando le verifiche: ripetere una ricerca deterministica identica non produce nuove evidenze sulla qualità. La variabilità delle domande e quella della generazione sono due fenomeni diversi.

## Le strade e la raccomandazione

| Strada | Cosa dimostra | Limite e posizione proposta |
|---|---|---|
| BEIR su un dataset piccolo | Confronto riproducibile del recupero con giudizi pubblici | Ottima alternativa se si preferisce un benchmark già annotato. Da solo non valuta risposte generate, citazioni o astensione. |
| MTEB / modello di embedding | Valutazione di rappresentazioni; eventuale modello pubblicabile | Estensione, non obiettivo iniziale. Usare MTEB non richiede necessariamente addestrare un modello nuovo; partecipare alla classifica segue una procedura distinta. |
| CRAG | Problemi RAG con benchmark pubblico | Il repository è accessibile. Non è stata verificata l'affermazione del brief sul calendario delle competizioni 2026; il progetto non deve dipendere da una competizione. |
| Open-RAG-Eval | Strumentazione di valutazione | Strumento eventuale, non progetto. Alcune metriche predefinite usano servizi o modelli aggiuntivi; valutarne costo e attendibilità. |
| Assistente su documentazione tecnica delimitata | Intera catena: acquisizione, ricerca, risposta, fonti, errori | Raccomandazione iniziale. Richiede costruire e controllare un piccolo insieme di domande di valutazione. Non ha l'indipendenza di un benchmark esterno. |
| Assistente su un dominio già conosciuto da Andrea | Caso d'uso comprensibile e vicino a possibili clienti | Preferibile alla documentazione tecnica se esistono fonti utilizzabili e Andrea sa verificare le risposte. |

Candidato concreto per la strada tecnica: una selezione della documentazione ufficiale di SQLite (FAQ, utilizzi, limiti e alcune funzionalità), congelata a una data e con un elenco esplicito delle pagine. È una proposta sostituibile: imparare un dominio sconosciuto richiede tempo oltre al RAG. Prima dell'acquisizione verificare le condizioni dei materiali specifici; non estendere automaticamente la licenza del software a ogni contenuto collegato.

Se si sceglie BEIR, SciFact è un candidato piccolo (circa 5.000 documenti e 300 query di test secondo il repository). Il suo dominio scientifico rende meno semplice la verifica personale. Usare il corpus completo del dataset scelto; un sottoinsieme di corpus modifica la difficoltà e va dichiarato come valutazione personalizzata. Non inserire obbligatoriamente sia un benchmark sia un corpus proprio nella prima consegna.

## Tre concetti, tre ruoli

- **RAG:** al momento della domanda si cercano informazioni nei documenti e le si passa al modello perché formuli la risposta. È un'architettura, non un addestramento.
- **Embedding:** un modello trasforma un testo in un vettore, cioè una sequenza di numeri, per confrontarlo con altri testi. La vicinanza può aiutare a trovare contenuti pertinenti, ma non certifica verità, accordo o presenza della risposta. Un RAG può anche usare sola ricerca lessicale.
- **Fine-tuning:** si modificano i parametri di un modello attraverso esempi. Si può adattare il modello che cerca oppure quello che genera: sono interventi diversi. Non è il modo predefinito di aggiornare un archivio documentale.

Correzioni al brief: la ricerca lessicale non si limita necessariamente alla corrispondenza letterale; embedding e fine-tuning non sono obbligatori in ogni RAG. Le percentuali sul mercato del lavoro provengono dal brief e non sono state rivalidate in questa sessione; non dimostrano, da sole, che il fine-tuning degli embedding sia la forma prevalentemente richiesta.

## Percorso di apprendimento e lavoro

Ogni tappa segue: spiegazione breve → esempio concreto → previsione di Andrea → piccolo esperimento → lettura degli errori → nota di ciò che abbiamo imparato. Non anticipare un'intera implementazione mentre Andrea sta ancora comprendendo il primo passaggio.

| Tappa | Cosa imparare e fare | Evidenza per chiuderla |
|---|---|---|
| 0. Il problema | Domanda, fonte, contesto, parametri del modello; esempio manuale su 5 documenti | Andrea sa distinguere cercare, generare e addestrare; ricostruisce una risposta con le sue fonti. |
| 1. Dati e misura | Scegliere corpus, fonti, domande e risposte attese; distinguere sviluppo e test | Piccolo campione verificato a mano e protocollo scritto prima dei confronti. |
| 2. Prima ricerca | BM25 e ricerca semantica sullo stesso materiale; vettori, similarità e limiti | Prime classifiche confrontate, casi in cui ciascuna ricerca sbaglia e misura di RAM/tempo. |
| 3. Migliorare la ricerca | Chunking, ricerca ibrida; eventualmente riordinamento dei risultati | Una modifica per volta, previsione registrata, confronto sullo sviluppo e configurazione finale congelata. |
| 4. Rispondere con prove | Budget di contesto, risposta, citazioni, astensione; generatore fisso | Valutazione distinta di recupero, correttezza, supporto delle citazioni e mancata risposta. |
| 5. Risultati e comunicazione | Confronto finale, analisi degli errori, limiti, riproduzione | Risultati sul test, report, esempi consultabili, istruzioni per ripetere il lavoro. |
| 6. Estensione facoltativa | Quando adattare un embedding e come evitare di memorizzare il test | Decisione motivata di farlo o rinviarlo; se fatto, confronto prima/dopo su dati separati. |

Stima di pianificazione, non promessa: 25–45 ore di lavoro attivo per un primo caso ristretto, compresi apprendimento, annotazione e report. Rivedere dopo le prime due sessioni. Quantità di documenti, lingua e conoscenza del dominio possono modificare molto la stima; tempo di calcolo escluso.

## Architettura iniziale

Preparazione: elenco fonti → copia congelata → estrazione di testo e titoli → passaggi con ID e riferimenti → indice lessicale + embedding salvati.

Domanda: ricerca → eventuale fusione delle classifiche → selezione entro un limite di testo → generatore → risposta e riferimenti ai passaggi.

Valutazione separata: domande attese + evidenze → risultati di ogni configurazione → metriche, tempi, errori → tabelle del report.

Scelte proposte, da verificare con un campione prima di bloccare le dipendenze:

- Python; file JSONL per documenti, domande e tracce; CSV per i risultati. Una pipeline piccola con componenti sostituibili.
- BM25 come riferimento; Sentence Transformers con un embedding piccolo per la ricerca semantica. Se il corpus è inglese, primo candidato `all-MiniLM-L6-v2`; controllare limite di token, licenza e scheda del modello prima di usarlo. Se le domande sono italiane e le fonti inglesi, il confronto è multilingue e richiede una scelta coerente: non cambiare lingua a metà esperimento.
- Similarità esatta su vettori salvati per il corpus piccolo. Un database vettoriale server non è un prerequisito; lo si aggiunge solo per esigenze reali di scala, aggiornamenti o accesso concorrente.
- Fusione lessicale/semantica tramite Reciprocal Rank Fusion, da spiegare e misurare. Nessuna assunzione che la combinazione vinca.
- Generatore locale piccolo come ipotesi a zero spese aggiuntive; modello preciso dopo verifica RAM e prova di latenza. La sostenibilità del RAG completo a zero euro non è ancora verificata. Se il locale è impraticabile, ridiscutere dimensione della prova o budget prima di introdurre API.
- Registrare fonti, passaggi trovati, posizioni, prompt, risposta, modello/versione, tempi e consumi disponibili. Separare tempo di indicizzazione e tempo per domanda. Cache per evitare di ricalcolare embedding o risposte identiche.

Reranker, database server, orchestratori complessi, agenti, PDF scansionati/OCR, produzione con endpoint e fine-tuning generativo sono fuori dalla prima versione. Il riordinamento dei risultati può entrare come singola estensione se la diagnosi lo giustifica.

## Protocollo minimo proposto per il corpus proprio

1. Partire con 10–15 domande pilota di sviluppo. Poi mirare a 80 domande complessive, indicativamente 40 sviluppo e 40 test; dimensione da correggere dopo aver misurato il tempo di annotazione. Nessun punteggio è già disponibile.
2. Per ogni domanda salvare risposta attesa, passaggi che la supportano, tipo e rispondibilità rispetto al corpus congelato. Gli esempi proposti dall'AI devono essere verificati da Andrea; evitare domande che ricopiano tutte le parole della fonte.
3. Includere identificatori precisi, parafrasi, risposte che richiedono due passaggi, ambiguità e domande senza risposta nel corpus. Distribuire anche i casi senza risposta tra sviluppo e test; dichiarare i denominatori.
4. Separare sviluppo e test per gruppi di domande/argomenti e parafrasi correlate. Tutto il corpus pertinente resta ricercabile. Preparare e congelare il test prima della selezione del metodo; non usarlo per scegliere parametri o soglie. Il test interno resta meno indipendente di una valutazione esterna, anche se congelato.
5. Stabilire gli ID delle evidenze prima di variare il chunking, riferendoli a testo sorgente/sezioni. Gestire i duplicati e misurare la copertura dei passaggi necessari, evitando che cinque pezzi dello stesso documento contino come cinque risposte utili.
6. Sullo sviluppo confrontare BM25, embedding e ibrido a chunking e k fissi. Poi al massimo due strategie di chunking sulla configurazione scelta. Registrare anche limite di token, troncamenti e quantità di contesto: cambiare chunk cambia il testo disponibile.
7. Recupero: Recall@5 sui casi rispondibili, con definizione esplicita delle evidenze rilevanti; nDCG@10 come misura complementare dell'ordine, se le annotazioni lo consentono. Riportare anche numeri per domanda e famiglia, non solo una media.
8. Generazione: sul campione fissato confrontare senza documenti, con ricerca di riferimento e con ricerca selezionata, a stesso modello/prompt e limite di contesto dove applicabile. Un piccolo controllo con evidenze corrette fornite a mano serve a distinguere errori di ricerca da errori del generatore.
9. Valutare correttezza rispetto alla risposta attesa, supporto delle affermazioni e delle citazioni, astensione corretta sui casi senza risposta e astensione errata sui casi rispondibili. Esistenza di un link e similarità elevata non provano che una citazione supporti un'affermazione. Controlli manuali con griglia esplicita; presentare le risposte in ordine casuale senza etichetta del metodo, quando praticabile. Un giudice LLM eventuale è un ausilio da calibrare, non la verità di riferimento.
10. Trattare i documenti come contenuti, non istruzioni: alcuni casi di sviluppo con istruzioni inserite nei documenti verificano questo confine. Sono prove mirate, non una certificazione di sicurezza.
11. Ripetizioni soltanto dove c'è variabilità utile da stimare: latenza e generazione. Per qualità del recupero deterministico usare confronti appaiati sulle domande e, se adeguato al campione, intervalli bootstrap per domanda/gruppo. Con 40 casi di test le conclusioni saranno limitate; non promettere significatività né miglioramenti.
12. Congelare la configurazione prima del test finale. Un peggioramento o un vantaggio non conclusivo è un risultato pubblicabile. Se si cambia dopo il test, quel test è diventato sviluppo: dichiararlo e predisporre una nuova verifica.

## Condizione per il fine-tuning

Aprire l'estensione solo dopo aver verificato che errori persistenti dipendano dalla rappresentazione semantica, e non da fonti mancanti, estrazione, chunking o generatore. Servono esempi di addestramento separati, confronti con alternative semplici, risorse misurate e un limite di lavoro dedicato. Valutare il modello adattato sul dominio e su un controllo esterno per individuare regressioni. Un miglioramento sui dati usati per insegnargli non dimostra generalizzazione. Nessun obiettivo di classifica MTEB nella prima consegna.

## Budget e uso dei modelli di assistenza

Non sostenere spese aggiuntive senza accordo sul limite. Abbonamento Codex/ChatGPT e uso API sono canali di fatturazione distinti: non assumere che l'applicazione possa chiamare gratuitamente le API grazie all'abbonamento. Nessuna installazione, download di modelli, chiamata API o addestramento è stato eseguito in questa sessione.

Seguendo la preferenza di Andrea: Astra per impostazione e revisione del protocollo; Sol per esercizi, implementazione per tappe e aggiornamento dei documenti; ritorno ad Astra se emergono dubbi sulla validità della valutazione, alla decisione sul fine-tuning e per la revisione delle conclusioni. È una divisione operativa proposta, non una stima verificata di risparmio. Il cambio di modello resta ad Andrea.

## Documentazione e criterio di completamento

Tre livelli, costruiti durante il lavoro:

- **Apprendimento interno:** `percorso/`, brevi note per tappa, esempi e domande di comprensione; `DIARIO.md` per decisioni, tentativi e problemi. Scrivere cosa Andrea sa già spiegare e cosa resta incerto.
- **Verificabilità tecnica:** README con riproduzione, configurazioni, protocollo e risultati/tracce. Versioni di dati, modelli e codice sufficienti a ricostruire ogni numero. Pubblicabilità dei dati da controllare.
- **Portfolio:** `report/report.md`, figure, pochi casi consultabili domanda → evidenza → risposta → verdetto; `report/CONSEGNA.md` per la sessione del sito. Sintesi home breve e collegamento al report, dettagli tecnici accessibili separatamente. Diario e note di studio non vengono consegnati come testo editoriale.

La prima consegna è completa quando la pipeline è ripetibile, c'è un confronto finale coerente, sono documentati errori e limiti, e Andrea sa spiegare senza leggere: perché usare RAG; cosa rappresenta un embedding; perché la similarità può sbagliare; come abbiamo scelto i passaggi; come distinguiamo un errore di ricerca da uno di risposta; perché il test è separato; quando non rispondere; quando il fine-tuning avrebbe senso. Un singolo progetto non equivale a esperienza su tutti i contesti produttivi.

La pubblicazione effettiva e l'HTML sono gestiti dalla sessione del sito secondo `CLAUDE.md`. Qui prepariamo la consegna, senza inventare risultati o pubblicare una demo vuota.

## Fonti consultate il 10 settembre 2026

- BEIR, obiettivo, metriche e dimensioni dei dataset: https://github.com/beir-cellar/beir
- MTEB e procedura per aggiungere un modello: https://github.com/embeddings-benchmark/mteb ; https://docs.mteb.org/contributing/adding_a_model/
- CRAG, repository pubblico: https://github.com/facebookresearch/CRAG
- Open-RAG-Eval, requisiti e componenti di valutazione: https://github.com/vectara/open-rag-eval
- Sentence Transformers, ricerca semantica: https://www.sbert.net/examples/sentence_transformer/applications/semantic-search/README.html
- Sentence Transformers, modelli e addestramento: https://www.sbert.net/docs/sentence_transformer/pretrained_models.html ; https://www.sbert.net/docs/sentence_transformer/training_overview.html
- SQLite, indice documentazione e pagina copyright: https://www.sqlite.org/docs.html ; https://www.sqlite.org/copyright.html
- OpenAI, distinzione accesso da piano e uso API: https://learn.chatgpt.com/docs/auth

Le dimensioni del campione, le ore, l'architettura e la raccomandazione di progetto sono scelte di pianificazione dell'assistente, non risultati riportati da queste fonti.
