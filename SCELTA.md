# Progetto — raccomandazione aggiornata

> **Decisione successiva:** Andrea ha approvato A e il passaggio a B se troppo lento. Per la preferenza finanziaria il dominio iniziale è ora **MTRAG FiQA**, al posto di Cloud. Stato operativo in `HANDOVER.md`, regole in `AGENTS.md`, architettura in `docs/ARCHITETTURA.md`. La discussione sotto è conservata come storico e non va riproposta come scelta aperta.

Aggiornato il 10 settembre 2026 dopo le precisazioni di Andrea. Nessuna implementazione avviata; la proposta specifica resta da discutere. Questo documento precede IMPOSTAZIONE.md per le scelte operative.

## Vincoli confermati e priorità

- Andrea può dedicare circa 5 ore al giorno fino alla conclusione. La velocità va misurata su lavoro assistito dall'AI, non stimata tramite equivalenze generiche con lavoro umano.
- Portatile con 24 GB di RAM e Intel Iris Xe, riportata dal sistema con 128 MB. Le GPU integrate Intel possono usare memoria condivisa: il numero non va interpretato come tutta la memoria accessibile. Progettare comunque prima per CPU; accelerazione eventuale solo se utile e verificata.
- Massimizzare apprendimento e prove di competenza riducendo costi e attività meccaniche. Preferire corpus, domande e annotazioni già pubblici. Escludere la costruzione manuale di un dataset di 80 casi.
- Budget aggiuntivo non ancora comunicato: default zero euro, nessuna spesa autorizzata.
- Ordine preferito: embedding → ricerca → RAG. Fine-tuning da capire e considerare come estensione, senza far dipendere la conclusione dall'addestramento.
- Una contribuzione esterna verificabile è desiderabile; non promettere ammissione, accettazione o classifica.

## Tre proposte con una domanda concreta

### A. Un assistente tecnico che trova la fonte anche nelle domande successive

**Raccomandazione per il rapporto fra ampiezza dell'apprendimento e lavoro manuale.** Base: dominio Cloud di MTRAG, benchmark IBM con annotazioni umane, compiti di recupero e generazione. Il corpus Cloud pubblicato ha 61.022 passaggi. Usare inizialmente i passaggi già forniti, senza rifare acquisizione e segmentazione.

Domanda principale: a generatore fisso, quanto incidono il metodo di ricerca e l'uso del contesto conversazionale sulla qualità delle risposte supportate dalle fonti?

Primo traguardo autonomo: confrontare ricerca lessicale, embedding piccolo e combinazione dei due, misurando qualità, memoria e latenza. Poi aggiungere la generazione sullo stesso materiale, con fonti già individuate dagli annotatori come riferimento. Le domande non autonome fanno emergere un problema concreto: «E come lo configuro?» richiede di sapere a cosa si riferisce «lo».

Il materiale include casi rispondibili, non rispondibili e parziali. Non utilizzare etichette di rispondibilità, risposte attese o passaggi di riferimento come input al recupero ordinario. I compiti in formato BEIR coprono i casi rispondibili e parziali: non confonderli con l'intera valutazione delle conversazioni.

Limiti: dominio inglese, valutazione generativa più costosa del recupero. Gli script ufficiali includono giudici LLM. A zero euro proporre una valutazione generativa esplorativa su un campione preregistrato, con pochi casi letti insieme; non dichiarare riproduzione completa del punteggio ufficiale usando un giudice diverso o metriche ridotte.

La competizione collegata SemEval 2026 aveva finestre di valutazione concluse tra gennaio e febbraio 2026: non è una submission attualmente disponibile sulla base del calendario pubblicato.

### B. Quanto possiamo alleggerire una ricerca semantica prima che peggiori?

Base: piccolo task pubblico BEIR/MTEB con annotazioni disponibili. Confrontare rappresentazioni normali e quantizzate, cioè memorizzate con meno precisione, misurando spazio occupato, velocità e qualità. Un solo embedding piccolo all'inizio; evitare una rassegna di decine di modelli.

Punti forti: costo monetario nullo per calcolo locale, valutazione del recupero automatica, apprendimento più profondo su vettori, memoria e compromessi. I limiti del portatile diventano un vincolo sperimentale misurabile.

Limiti: è soprattutto un progetto di ricerca semantica; il RAG deve essere aggiunto dopo e valutato separatamente. Non spacciare la compressione dei vettori per fine-tuning. La calibrazione della quantizzazione non usa etichette del test; indicare se usa il corpus indicizzato. Misurare la memoria effettiva, incluse eventuali copie in alta precisione, e non dedurre la velocità dalla sola riduzione dei byte.

È la scelta più economica e lineare per un primo risultato sugli embedding. MTEB offre una via reale per proporre risultati, subordinata a un contributo utile e alla revisione dei manutentori.

### C. Il controllore che riconosce risposte prive di supporto

Base: RAGBench, che contiene domande, documenti, risposte già prodotte e annotazioni sul supporto delle affermazioni. Confrontare la similarità tramite embedding con un piccolo modello di verifica, per capire perché una risposta semanticamente vicina ai documenti possa comunque essere sbagliata.

Punti forti: poche generazioni nuove, dati pronti, collegamento al lavoro già fatto sulla valutazione. Non trasferire le annotazioni di una risposta del dataset a una risposta nuova: riguardano quel testo specifico. Controllare l'origine delle annotazioni, che include modelli, anziché presentarle tutte come verità umana indipendente.

Limiti: copre prevalentemente la valutazione di un RAG e lascerebbe meno esperienza pratica sulla pipeline completa. Per questo è terza scelta per l'obiettivo attuale. Non è stata identificata una submission ufficiale aperta per questo progetto.

## Percorso consigliato se scegliamo A

1. **Embedding:** pochi esempi per capire vettori, similarità, ricerca domanda-passaggio e casi in cui vicinanza non significa risposta corretta. Una sola breve esercitazione, non un corso propedeutico separato.
2. **Prova di fattibilità:** su un campione di passaggi misurare tempo di codifica, memoria e lunghezze; stimare il corpus intero e poi verificare. La RAM da sola non predice la velocità. Modello di embedding scelto anche in base alla lunghezza dei passaggi, evitando troncamenti silenziosi.
3. **Ricerca:** corpus Cloud completo per le misure confrontabili sul dominio, tre configurazioni al massimo: lessicale, embedding, ibrida. Nessun generatore necessario per questo traguardo. Distinguere tempo una tantum di indicizzazione e latenza per domanda.
4. **Contesto conversazionale:** scegliere una strategia semplice di rappresentazione della conversazione e confrontarla con l'ultima domanda isolata; evitare da subito riscritture con molte chiamate LLM. La cronologia di riferimento fornita dal benchmark è un'impostazione diversa dal dialogo libero con risposte precedenti generate da noi, e va dichiarata.
5. **RAG:** usare un generatore locale piccolo solo dopo una prova breve. Confrontare evidenze corrette fornite dal dataset con evidenze recuperate, così da distinguere i limiti del generatore da quelli della ricerca. Risposte con riferimenti, gestione delle informazioni mancanti, tracce.
6. **Report:** domanda, riferimento iniziale, confronto, pochi errori rappresentativi, risorse usate, limiti. Ogni numero deve ricondurre a un risultato salvato; non serve leggere manualmente tutte le esecuzioni.

Se A si rivela troppo lento o complesso nel primo ciclo, B è la riduzione di ambito proposta. Non tagliare il corpus intorno alle risposte corrette e poi confrontare il risultato con quello ottenuto sul corpus completo.

## Rigore proporzionato all'esercizio

Automatizzare download, controlli dei formati, verifica degli ID, calcolo delle metriche e tabelle. Riutilizzare le annotazioni esistenti. Leggere insieme indicativamente 6–10 casi selezionati per insegnare qualcosa; quel campione serve a spiegare gli errori, non a stimare con precisione la qualità dell'intero sistema.

Separare sviluppo e verifica finale per conversazione, non per singoli turni che condividono la stessa storia. Usare split ufficiali quando appropriati; altrimenti congelare uno split interno e dichiararlo. Una prova sul solo Cloud non è il punteggio aggregato su tutto MTRAG. La valutazione generativa completa resta da dimensionare, includendo il costo del giudice.

Il metodo professionale si mostra formulando ipotesi, controllando i confronti, misurando le risorse e distinguendo i risultati dai limiti. Capacità non esercitate su grande scala non possono essere certificate semplicemente attribuendo ogni limite all'hardware. Possiamo dimostrare ciò che abbiamo eseguito e spiegare come progetteremmo l'estensione.

## Contribuzione pubblica: cosa è davvero possibile

Correzione al brief: **per contribuire risultati a MTEB non è obbligatorio fare fine-tuning o creare un embedding nuovo.** La guida ufficiale mostra valutazione e proposta di risultati anche per un modello esistente (`all-MiniLM-L6-v2`). Le proposte passano per pull request e revisione. Prima di dedicare calcolo a una submission, verificare risultati già presenti e un contributo mancante/correzione utile. Non è stato ancora individuato un abbinamento modello-task da proporre.

La classifica identifica il modello; la PR documenta chi ha contribuito alla valutazione. Non presenteremo un modello altrui come modello di Andrea. MTRAG e MTEB hanno protocolli distinti: risultati del nostro esperimento non sono automaticamente sottomettibili a MTEB.

Nel precedente τ²-bench il report documenta un'issue agli autori. È una contribuzione pubblica, distinta da una submission di punteggi e da una PR accettata. Qui potremo documentare risultati, proporre una PR utile o segnalare un problema reale, senza inventare un difetto per ottenere un link.

## Tempi, modelli e prossimo passo

Ritirata la stima precedente di 25–45 ore. Dopo il primo ciclo misurare separatamente lavoro attivo, attesa di calcolo e tempo di apprendimento; stimare solo il prossimo traguardo. Il codice può accelerare molto con l'AI, ma la comprensione va verificata nell'interazione, non inferita dalla presenza dei file.

Astra per chiudere questa scelta; Sol per esercitazioni e implementazione progressiva; revisione Astra se emergono dubbi metodologici e prima delle conclusioni. Nessun nuovo modello o dipendenza installato in questa sessione.

Prossima decisione consigliata: scegliere A con traguardo iniziale limitato agli embedding e alla ricerca, mantenendo B come alternativa se prevale la rapidità del primo risultato. Il budget resta zero finché Andrea non indica diversamente; non serve decidere subito un budget API per iniziare dagli embedding.

## Fonti primarie verificate il 10 settembre 2026

- MTRAG, corpus e compiti: https://github.com/IBM/mt-rag-benchmark
- Annotazioni e impostazioni reference/full RAG: https://github.com/IBM/mt-rag-benchmark/blob/main/mtrag-human/README.md
- Valutazione e giudici: https://github.com/IBM/mt-rag-benchmark/blob/main/scripts/evaluation/README.md
- Finestre SemEval: https://ibm.github.io/mt-rag-benchmark/MTRAGEval/
- Submission MTEB: https://docs.mteb.org/contributing/submitting_results/
- Quantizzazione dei vettori: https://www.sbert.net/examples/sentence_transformer/applications/embedding-quantization/README.html
- RAGBench: https://huggingface.co/datasets/galileo-ai/ragbench
- Memoria grafica Intel: https://www.intel.com/content/www/us/en/support/articles/000020962/graphics.html

Titoli di progetto, raccomandazioni, campioni e piano sono proposte nostre, non prescrizioni degli autori dei benchmark.
