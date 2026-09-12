# Imparare e documentare attraverso le figure

Piano del 12 settembre 2026. Specifica operativa per chi insegna D1–D6: Andrea preferisce spiegazioni visive, confronti affiancati, colori e poche scritte pertinenti. Le figure si realizzano durante gli esperimenti; questo catalogo non è una consegna già prodotta e non richiede un secondo progetto di dashboard.

## Ciclo della lezione

1. Aprire con la domanda applicativa e uno schema del punto della pipeline che stiamo cambiando. Riutilizzare lo schema precedente evidenziando il componente attivo.
2. Spiegare come leggere la vista: significato di punti, colori, frecce, assi e unità. Definire i termini nuovi prima di usarli nella legenda.
3. Mostrare alternative sullo stesso esempio; chiedere una previsione progettuale, senza quiz di programmazione.
4. Dopo la prova, mostrare il comportamento misurato e il testo delle fonti coinvolte. Una figura non sostituisce la lettura di una condizione o di un'eccezione.
5. Chiudere con tre frasi: cosa osserviamo, quale scelta suggerisce, cosa non dimostra. Salvare la vista utile mentre lavoriamo.

Default: una vista di meccanismo e una di evidenza per decisione, spesso pannelli dello stesso artefatto. Le viste aggiuntive sotto sono attivabili quando risolvono una domanda; non un elenco di grafici obbligatori. Non rimandare tutte le figure alla fine né interrompere il corso per perfezionare il design.

## Catalogo delle viste

| ID / fase | Domanda e costruzione della figura | Materiale necessario e lettura corretta |
|---|---|---|
| V01 / D1, poi riusata | **Da testo a fonte recuperata.** Due corsie: documenti → segmenti → encoder → vettori/indice; domanda → stesso encoder → ricerca → classifica. BM25 in corsia parallela. Evidenziare online e preparazione una tantum; aggiungere contesto/risposta solo in D3. | Schema didattico, nessun numero sperimentale. Far vedere dove intervengono modello, nostri parametri e annotazioni di valutazione, queste ultime fuori dagli input. |
| V02 / D1 | **Che cosa finisce vicino?** Due mappe 2D affiancate, stessi passaggi/ID, MiniLM e BGE. Domanda a stella; selezione di un punto mostra testo, metadati e vicini reali. Colori per categorie note, contorno per pertinenza alla domanda. | Embedding reali, coordinate salvate, metadati con provenienza. Affiancare sempre V03. In assenza di categorie affidabili usare colori neutri/documento d'origine: non inventare temi per dare forma al grafico. |
| V03 / D1–D2 | **Quale fonte viene trovata o persa?** Una domanda, colonne BM25/MiniLM/BGE, righe di classifica con stessi ID e breve estratto; evidenza pertinente evidenziata. Dopo D2, mostrare le posizioni prima/dopo fusione e reranking. | Top-k, annotazioni, testi, configurazioni. Mostrare posizioni anziché confrontare direttamente score BM25 e cosine. Passaggio non annotato = non giudicato, se le annotazioni non sono esaustive; non automaticamente errato. |
| V04 / D2 | **Dove tagliare il documento?** Testo con due segmentazioni affiancate, confini e sovrapposizioni visibili; collegare regola ed eccezione. Un pannello mostra il filtro prodotto/versione e cosa esclude. | Dossier con offset e versioni; esempio simulato segnalato. Prima spiegare con una coppia, poi misurare le configurazioni; non cambiare contemporaneamente chunking, encoder e filtro attribuendo il risultato al solo chunking. |
| V05 / D1–D4 | **Quale compromesso conviene?** Punti qualità–latenza con etichette di configurazione; pannello separato per memoria/indicizzazione. Eventuale matrice metodi × tipi di domanda con valori e numerosità, per scoprire dove cambia la scelta. | Metriche per query, tempi e stesso split. Recall/nDCG per ricerca, valutazione delle risposte su altro pannello. Categorie congelate sullo sviluppo, denominatori visibili; niente ranking affidabile per categorie con pochissimi casi. |
| V06 / D3 | **Dove nasce l'errore RAG?** Domanda → fonti recuperate → contesto inviato → affermazioni. Collegamenti da ciascuna affermazione al passaggio che la sostiene, la contraddice o non basta; confronto recuperato/gold sullo stesso caso. | Traccia reale, testo effettivo del contesto e verifica semantica dei riferimenti. Un ID di citazione non basta. Distinguere fonte mai trovata, trovata ma esclusa dal contesto, risposta non supportata. |
| V07 / D4 | **Che cosa cambia con un agente?** Tre corsie RAG fisso / agente file / agente wiki per la stessa domanda; blocchi ricerca, lettura, generazione, arresto. Se misurata, larghezza proporzionale al tempo; altrimenti schema senza scala temporale. | Eventi e tool call reali, limiti applicati, risultato finale. Mostrare una traccia istruttiva e rimandare ai risultati aggregati: meno passaggi non significa migliore risposta. |
| V08 / D4 | **Quanto costa preparare e aggiornare la wiki?** Piccolo grafo fonte/versione → pagina wiki → risposta; evidenziare cosa deve cambiare dopo l'aggiornamento di una fonte. Grafico cumulativo a parte: costruzione + interrogazioni + aggiornamenti. | Provenienza e tempi/token di compilazione e replay. Grafo limitato alle dipendenze del caso, non una rete illeggibile. Costo temporale e token separati; eventuale punto di pareggio vale solo per quel carico, anche quando non viene raggiunto. |
| V09 / D5 | **Quando il riuso è corretto?** Sequenza domanda → ripetizione → cambio di storia → cambio di fonte. Stato cache e risposta per ogni evento; a fianco costi freddo/caldo. Per cache semantica, curva soglia vs riusi corretti/errati. | Replay con chiavi/versioni, hit/miss, invalidazioni e controlli. Mostrare risparmio insieme ai falsi riusi; soglia scelta sullo sviluppo. Cache embedding e risposta distinte, nessun risparmio token dedotto da un semplice hit. |
| V10 / D6, condizionale | **Serve adattare i pesi?** Albero decisionale conoscenza / rappresentazione / comportamento, con alternative e dati richiesti. Solo se avviene training: confronto base/adattato su test separato, qualità e regressioni per compito. | Albero = schema di ragionamento, non prescrizione universale. Curve di loss eventuali per studio; una loss in discesa non dimostra miglioramento applicativo. Nessun grafico prima/dopo se il training non è stato eseguito. |
| V11 / chiusura progressiva | **Cosa sceglierei e perché?** Tabella visuale di decisione: esigenza, sistema/configurazione scelto, evidenza, compromesso. Piccoli grafici di V05/V08/V09 riusati dove aiutano. | Solo risultati disponibili; alternative mai provate marcate tali. Nessun radar di competenze, voto complessivo arbitrario o architettura vincente per definizione. |

## Proiezioni: t-SNE, UMAP e PCA

t-SNE è un algoritmo di riduzione dimensionale; `TSNE` è una sua implementazione in scikit-learn, non una libreria di plotting. Produce coordinate da disegnare. UMAP è un'altra opzione; PCA fornisce una proiezione lineare utile come riferimento. Non occorre eseguire tutte e tre per collezionare strumenti: partire dalla mappa prevista in D1, scegliere il metodo in base a domanda e pilot CPU; usare un secondo metodo quando chiarisce un possibile artefatto.

Per V02:

- Ricerca e metriche usano i vettori originali: la riduzione a 2D è solo una vista in questo esperimento. Ridurre i vettori per il retrieval sarebbe un'altra decisione da valutare separatamente.
- Stessi ID e stessa selezione deterministica nei due modelli. Non mescolare i vettori di encoder diversi in un unico spazio assumendoli compatibili. Non interpretare spostamenti fra coordinate di mappe indipendenti, né chiamare gli assi «rischio» o «finanza».
- Conservare modello/revisione, input, normalizzazione, metrica, metodo di proiezione/versione, parametri, seed, campione e coordinate. Le query visualizzate non devono cambiare silenziosamente una mappa già confrontata: congelare il set oppure indicare che la proiezione è stata ricalcolata.
- Se una conclusione visiva guida la discussione, controllarne la sensibilità con un'altra inizializzazione/parametro ragionevole sul campione. Non scegliere la mappa più bella dopo molti tentativi.
- Distanze e dimensioni dei gruppi nella mappa possono ingannare. Controllare i vicini nello spazio originale e la pertinenza nella classifica. La disposizione non dimostra che un modello capisca meglio il dominio.
- 2D per la documentazione. 3D solo se l'esplorazione aggiunge informazione verificabile: occlusione, rotazione e prospettiva rendono uno screenshot meno immediato.

Fonti tecniche consultate: [scikit-learn, effetto della perplexity in t-SNE](https://sklearn.org/stable/auto_examples/manifold/plot_t_sne_perplexity.html), [UMAP, parametri](https://umap-learn.readthedocs.io/en/latest/parameters.html), [UMAP, limiti per clustering](https://umap-learn.readthedocs.io/en/latest/clustering.html). Verificare l'API della versione effettiva prima di implementare.

## Grammatica visiva comune

- Una domanda per figura; titolo breve. Pannelli A/B/C con ordine stabile: input → trasformazione → esito, oppure baseline → variante. Legenda vicina ai dati e testo essenziale direttamente accanto all'elemento.
- Colori dei metodi stabili nella stessa famiglia di confronti: baseline grigio, variante principale blu, alternativa arancio, ulteriore variante viola. Etichette e forme accompagnano sempre il colore. Lo stato supportato/non supportato usa icone e contorni distinti, non la stessa codifica dei metodi.
- Mappe: colori categoriali per temi/metadati; matrici: scala sequenziale per quantità, divergente centrata su zero per differenze. Non usare arcobaleni, rosso/verde come unica distinzione o intensità normalizzata separatamente per far sembrare uguali pannelli diversi.
- Assi e unità espliciti, scale comparabili tra pannelli dello stesso confronto. Barre partono da zero; metriche mancanti indicate N/D. Tempi una tantum e per domanda separati. Intervalli soltanto se calcolati con un metodo dichiarato; eventuale ricampionamento per conversazione, preservando la dipendenza dei turni.
- Nel footer: «schema didattico», «dossier simulato» o «misura su …», run/split e n quando pertinente. Colore/tema assegnato dall'IA segnalato come annotazione esplorativa, non ground truth.
- Font leggibili anche nella pagina del report e nella miniatura del portfolio; evitare didascalie microscopiche. Testo alternativo con conclusione e limite. La figura deve restare interpretabile senza hover e senza percepire tutti i colori.

## Produzione, salvataggio e riuso

Le convenzioni generali restano in [DOCUMENTAZIONE](DOCUMENTAZIONE.md). L'assistente automatizza generazione ed export; Andrea non deve fare screenshot e impaginazione manualmente.

Per esplorare: HTML interattivo locale quando selezione di query, hover con testo, filtri o confronto parametri aiutano davvero. Plotly è una possibile implementazione; nessuna dashboard/server obbligatoria. Per esportare: grafici riproducibili con strumenti standard come Matplotlib; SVG e PNG da stessi dati. Diagrammi in Mermaid/SVG. Scegliere la soluzione più leggera compatibile con l'ambiente, senza installazioni preventive.

Non usare un servizio di generazione immagini per grafici quantitativi. Lo screenshot è una comodità, l'export con sorgente e dati è la prova riutilizzabile. Un grafico didattico sintetico resta tale anche se molto rifinito.

- Schemi di studio e sorgenti in `percorso/figure/`; visualizzazioni di run in `runs/<run_id>/figures/`, script comuni in `scripts/figures/` quando necessari.
- Ogni vista misurata conserva un piccolo manifest con ID V, domanda, run di confronto, file dati/hash, script/config, seed, filtri/query selezionata, parametri, export e limite. Non copiare interi dataset nel manifest.
- Per pubblicare selezionare le figure nel solo `report/figure/INDEX.md`, come già previsto, con sorgente, didascalia e testo alternativo. Se si esporta una nuova revisione, conservare il riferimento alla versione usata nel report; mai sovrascrivere una run.
- Renderizzare e guardare gli export a dimensione reale e ridotta: niente tagli, sovrapposizioni, legenda mancante o controlli indispensabili persi. Controllare che valori e campioni corrispondano alla run.
- Per i casi narrativi dichiarare la regola di scelta: un caso rappresentativo e un limite rilevante, collegati agli aggregati. Nessuna selezione dei soli successi. I dati non disponibili non diventano grafici segnaposto pubblicabili.

## Percorso del recruiter

Una sintesi visuale unica per M1/M2/M3; nessun obbligo di visitare tutte le viste del laboratorio. Alla chiusura selezionare indicativamente quattro pannelli:

1. **Problema e sistema:** V01 ridotta, con la scelta progettuale centrale.
2. **Confronto e decisione:** V05/V11, qualità e costo nello stesso compito.
3. **Una prova leggibile:** V03 oppure V06; mappa V02 solo se aggiunge una spiegazione, sempre con ranking.
4. **Quando cambierei soluzione:** V07–V09 e compromesso osservato, inclusi aggiornamenti.

Ogni pannello: figura + conclusione breve + limite + collegamento diretto alla prova. Approfondimenti interattivi e note restano facoltativi. Il numero è un budget editoriale, non un requisito di quattro figure prima di poter chiudere M1. Nessuna modifica al sito richiesta da questo piano: la selezione verrà consegnata quando esistono risultati.
