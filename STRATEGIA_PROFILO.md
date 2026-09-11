# Quale prossimo progetto serve al profilo di Andrea?

> Documento storico. Dall'11 settembre prevale [PIANO_MASTER](PIANO_MASTER.md): Andrea è l'utilizzatore, feedback esterno e ricerca clienti non sono requisiti. La sequenza corrente è ricerca → RAG personale/cache → confronto agentico/wiki. Le indicazioni operative precedenti qui sotto non vanno riapplicate in contrasto con il piano.

Valutazione strategica del 10 settembre 2026, richiesta da Andrea dopo la scelta MTRAG FiQA. Questo documento riconsidera il traguardo professionale; non contiene risultati sperimentali e non autorizza spese, pubblicazioni o contatti esterni. Le basi e il metodo didattico restano approvati; le estensioni qui raccomandate non sono implementate.

## Raccomandazione

Posizionamento di lavoro: **Applied AI Engineer capace di integrare modelli, dati e strumenti in un'applicazione verificabile, con attenzione all'affidabilità.** È coerente con i lavori già pubblicati e con l'obiettivo di rendersi utile alle aziende. Non assumere seniority, capacità software o esperienza produttiva che il portfolio non dimostra ancora.

Il prossimo progetto consigliato è un **assistente documentale finanziario per preparare risposte con fonti**, sviluppato in due traguardi separati:

1. MTRAG FiQA come laboratorio per imparare embedding, ricerca, valutazione e RAG. Un risultato di recupero riproducibile, con poche configurazioni, chiude questo traguardo.
2. Una piccola applicazione del metodo: un servizio utilizzabile, provato da una persona esterna e accompagnato da test, tracce, comportamento sui guasti e un resoconto del feedback. MTRAG può alimentare la demo iniziale; un vero caso aziendale richiede esigenze e fonti proprie.

Il secondo traguardo copre meglio i vuoti del portfolio rispetto a una lunga ricerca di piccoli miglioramenti sul benchmark. Se nel frattempo emerge un processo aziendale accessibile con un utilizzatore reale, quello può diventare il caso applicativo prioritario. Finanza resta una preferenza, non un vincolo che impedisce esperienza concreta altrove.

## Evidenze di partenza e loro limiti

Letti PROGETTI.md e la ricerca AI engineer di SitoPersonale, oltre ai report già esaminati. Il portfolio dimostra soprattutto valutazione, tracce e analisi degli errori. Il registro della centralina segnala come scoperte produzione, recupero sui dati, valutazione online e permessi. Non è un audit completo del codice o di tutte le esperienze di Andrea.

La ricerca precedente contiene percentuali aggregate e conclusioni forti sul mercato. Non sono state replicate in questa sessione: campioni orientati al senior e fonti editoriali non bastano a stabilire quali competenze garantiscano assunzioni junior. In particolare, una percentuale di annunci non dimostra né irrilevanza generale del fine-tuning né una separazione assoluta tra AI engineer e ML engineer.

Verifica mirata su pagine aziendali, non indagine rappresentativa, al 10 settembre 2026:

| Fonte | Segnale osservato | Limite |
|---|---|---|
| AssistenteRUP, Junior AI Engineer | Ricerca ibrida, RAG, valutazione; Python engineering, test e backend | Un solo datore, dominio giuridico; non prova prevalenza di mercato |
| ION, Graduate AI Engineer, Trento/Milano | Fintech; basi software, consegna di prodotti, LLM, RAG, embedding, valutazione e fine-tuning | Requisito accademico specifico: laurea magistrale STEM recente/in corso; idoneità di Andrea non verificata |
| Algor, Junior AI Engineer | Scelta di modelli, agenti, RAG, fine-tuning; deployment cloud tra le preferenze | Un ruolo specifico, non tutto obbligatorio né garanzia di selezione |
| EY Milano, AI Engineer Senior, annuncio 1 settembre 2026 | RAG e agenti insieme a integrazione, test, CI/CD e monitoraggio | Segnale di evoluzione del mestiere, non soglia d'ingresso |
| Capco Italia, Lead AI Engineer | Sistemi agentici, RAG, memoria, tool calling e affidabilità in produzione | Posizione lead, da non usare come checklist per un principiante |

Inferenza: conviene saper collegare retrieval, applicazione e verifica. Le pagine non supportano l'idea che RAG sia già irrilevante. Gli annunci non misurano le probabilità di assunzione. Non sono state inviate candidature o comunicazioni.

## RAG, ricerca agentica e LLM Wiki: le differenze utili

Il problema comune è dare al modello informazioni esterne appropriate. Le tecniche differiscono nel lavoro fatto prima della domanda, nelle decisioni prese durante la risposta e nella manutenzione successiva.

| Approccio | Cosa fa | Quando valutarlo | Cosa misurare |
|---|---|---|---|
| Ricerca lessicale / nessun generatore | Restituisce direttamente i testi | Lookup, bisogno di fonti più che di sintesi | Successo dell'utente, pertinenza, latenza |
| Contesto completo, se entra | Passa tutto il piccolo archivio al modello | Pochi documenti e budget compatibile | Qualità, token, latenza, distrazioni |
| RAG con ricerca predefinita | Recupera passaggi e genera una risposta | Molte domande su archivi indicizzati | Recupero, risposta, citazioni, aggiornamento |
| Ricerca agentica | Il modello decide ricerche e letture successive, e quando fermarsi | Domande ambigue, esplorative o che richiedono più fonti | Miglioramento rispetto alla ricerca fissa, tool call, fallimenti, costo |
| Wiki mantenuta da LLM | Produce e aggiorna sintesi collegate derivate dalle fonti | Conoscenza accumulata e riutilizzata, sintesi ricorrenti | Costo di costruzione/manutenzione, provenienza, omissioni e aggiornamenti |

RAG non richiede sempre embedding. La ricerca agentica può usare grep, ricerca lessicale, vettori o una combinazione. Un risultato recuperato da un agente e usato per generare rimane una forma di generazione aumentata da informazioni recuperate. La wiki può essere cercata a sua volta con un motore o un agente. Queste categorie non formano una successione in cui ogni nuova voce rende inutile la precedente.

Il gist LLM Wiki di Karpathy è un pattern di organizzazione della conoscenza: fonti originali, pagine derivate, convenzioni e manutenzione. Prevede anche l'aggiunta di ricerca quando serve. Non è una dimostrazione generale di superiorità né la promessa che una sintesi resti corretta senza controlli. Persistenza di indici/cache esiste anche nel RAG: evitare la falsa spiegazione che tutto venga sempre ricalcolato da zero.

Esistono anche sistemi di ricerca che usano il nome LLM-Wiki: distinguerli dal pattern personale di Karpathy. Il preprint di Ming e colleghi propone compilazione e navigazione agentica e riporta miglioramenti nei propri benchmark. Un diverso preprint, su appena 24 documenti e 13 domande, trova compromessi dipendenti da baseline e giudici. Qui sono stati esaminati gli abstract, non riprodotti gli esperimenti: nessuno dei due autorizza una conclusione universale sul progetto di Andrea.

La guida Anthropic sul contesto descrive navigazione progressiva e strategie ibride, con un costo di esplorazione durante la query. Inferenza progettuale: un agente è utile se le sue decisioni migliorano il risultato abbastanza da compensare più chiamate e maggior variabilità. Il confronto va misurato sul compito, non deciso dal nome.

## Cosa cambierei nel piano operativo

**Conservare:** percorso da zero, FiQA come materiale già annotato, prima misura CPU, tre metodi di recupero al massimo, fonti e risultati tracciati, report breve. La sequenza embedding → ricerca → RAG è didattica, non una legge che obbliga a usare vettori in ogni progetto.

**Ridurre:** confronto esteso di modelli, inseguimento di leaderboard, fine-tuning privo di necessità, produzione anticipata di documentazione molto lunga. Non serve diventare specialisti IR prima di provare un'applicazione.

**Aggiungere come estensione delimitata dopo la baseline:** un agente con soli strumenti di ricerca e lettura, limite esplicito di passi e token, accesso alle stesse fonti e nessuna annotazione attesa. Il confronto principale usa lo stesso generatore e riporta separatamente contesto totale e chiamate; stesso modello non significa stesso budget. Congelare domande prima della selezione e dichiarare eventuali sottoinsiemi. Se il modello locale non sa usare bene gli strumenti, registrarlo come limite del sistema provato, non come sconfitta generale dell'approccio agentico.

FiQA è adatto al confronto dei metodi di ricerca, meno a provare i vantaggi di navigare gerarchie ricche di file: non creare cartelle o indici usando risposte del test. Per una prova di vera navigazione documentale usare una piccola collezione con struttura naturale e dichiarare una valutazione separata.

**Dare precedenza dopo la baseline a un servizio piccolo:** backend con input validati, test pertinenti, errori e timeout gestiti, log, cache, limite di costo, configurazione ripetibile. Se si espongono dati diversi a utenti diversi, autorizzare prima del recupero. Un solo percorso d'uso: domanda → fonti → risposta proposta → feedback. Aggiornamento o rimozione di una fonte deve avere un comportamento verificabile. Deployment e gestione utenti vanno dimensionati al caso, non a una lista di tecnologie.

Per zero euro iniziare con servizio locale e prova supervisionata; pubblicare eventualmente una demo registrata e risultati consultabili. Questo dimostra un prototipo utilizzabile, non un servizio cloud già in esercizio. Hosting gratuito, endpoint pubblici e nuove spese non sono assunti disponibili o autorizzati.

**LLM Wiki:** estensione successiva breve su fonti contenute, se imparare manutenzione della conoscenza aggiunge valore. Esempio: aggiungere una nuova versione di una fonte e verificare quali sintesi cambiano, quali affermazioni obsolete restano e se i riferimenti riportano all'originale. Non costruirla sull'intero MTRAG per completare una terna di tecnologie. Costi di compilazione, query, aggiornamento e verifica tutti espliciti; nessuna garanzia di ammortamento.

## Quale progetto successivo o parallelo

| Priorità | Attività | Nuova prova per il portfolio | Perimetro |
|---|---|---|---|
| Parallelo leggero, subito | Individuare un utilizzatore e un compito frequente; avviare candidature mirate quando i materiali sono presentabili | Capacità di capire requisiti e ottenere feedback | Poche conversazioni e una scheda problema; Andrea gestisce i contatti, nessun invio automatico |
| Subito dopo il laboratorio | Portare l'assistente a un piccolo servizio e farlo provare | Backend, integrazione, rilascio, gestione errori e feedback | Una funzione, un flusso, pochi test significativi; non un prodotto SaaS completo |
| Progetto successivo distinto | Assistente operativo per riconciliare movimenti e documenti contabili di esempio | SQL, dati strutturati, validazione, workflow con conferma | Dati sintetici/pubblici o autorizzati; codice per importi e corrispondenze, LLM per testo ambiguo; niente operazioni bancarie reali |
| Esperimento facoltativo | Wiki finanziaria aggiornabile e controllata | Provenienza, versioni, aggiornamento delle sintesi | Poche fonti, vecchia/nuova versione, casi noti; distinta da evidenza di produzione |

Il progetto di riconciliazione deve confrontarsi con una soluzione deterministica di base. Il modello propone soltanto dove serve; i totali, le valute, gli identificatori e i duplicati si verificano con codice. Un risultato utile può essere scoprire che l'LLM non migliora quel passaggio.

Non eseguire contemporaneamente tre grandi implementazioni. Il parallelismo più utile ora è tecnico + scoperta del bisogno/candidature. Se un utilizzatore reale è già disponibile per una diversa automazione, il valore marginale di lavorare con lui può superare la preferenza finanziaria e la novità architetturale.

## Quali prove deve poter mostrare Andrea

- Sa spiegare come dati, codice, modello e valutazione producano il risultato, con un errore concreto e una modifica motivata.
- Sa leggere e modificare il codice essenziale, verificare un bug e giustificare un test. L'assistenza dell'AI è trasparente; non sostituisce questa comprensione.
- Consegna un'applicazione piccola e ripetibile, distinguendo demo, prova con utente e servizio in produzione.
- Dimostra trade-off con numeri, senza attribuire validità universale a un campione.
- Ha almeno un feedback esterno sul flusso; un singolo test utente è evidenza di usabilità iniziale, non di valore economico generalizzato.

Proposta di narrazione portfolio: τ²-bench dimostra diagnosi e valutazione; l'assistente documentale aggiunge dati propri e integrazione; l'esercizio operativo successivo aggiunge dati strutturati e un processo con effetti controllati. Pochi lavori complementari sono più utili di più chatbot simili.

## Fonti e verifica

Pagine consultate il 10 settembre 2026; possono cambiare. Annunci come esempi, non campione statistico. Nessuna raccomandazione di candidatura basata sull'idoneità personale, ancora da verificare.

- AssistenteRUP: https://assistenterup.it/career/junior-ai-engineer
- ION: https://jobs.lever.co/ion/f93f9d1a-9006-405e-ba38-9e79b98f4301
- Algor: https://www.algoreducation.com/it/team-and-careers
- EY: https://careers.ey.com/ey/job/Milano-AI-Engineer_Senior-20123/1432378533/
- Capco: https://job-boards.greenhouse.io/capco/jobs/8173426
- Anthropic, contesto e ricerca agentica: https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents
- Karpathy, LLM Wiki: https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f
- Ming et al., abstract LLM-Wiki: https://arxiv.org/abs/2605.25480
- Cochran, abstract confronto piccolo: https://arxiv.org/abs/2605.18490

Le priorità e la proposta di percorso sono un giudizio ragionato dell'assistente sulla situazione di Andrea, non un risultato dimostrato dalle fonti né una garanzia di lavoro.
