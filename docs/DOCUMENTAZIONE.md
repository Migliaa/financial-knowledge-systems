# Documentare durante il lavoro

Obiettivo: ogni risultato ricostruibile, ogni concetto recuperabile, un report finale leggibile in pochi minuti. Non registrare tutto in prosa e non produrre documenti paralleli con le stesse informazioni.

## Una casa per ogni informazione

| Informazione | Dove vive | Quando aggiornare |
|---|---|---|
| Stato, comprensione e prossima azione | `HANDOVER.md` | Fine sessione o cambio di tappa |
| Storia delle decisioni | `DIARIO.md` | Una riga densa per sessione |
| Concetto studiato ed esempio | `percorso/NN-tema.md` | Durante la tappa |
| Ipotesi e interpretazione | `esperimenti/E001-nome.md` | Prima della prova, poi dopo |
| Configurazione e dati misurati | `runs/<run_id>/` | Automaticamente durante ogni run |
| Programma e ordine delle unità | `PIANO_MASTER.md` | Solo quando cambia il piano |
| Numeri e spiegazioni pubblicabili | Report autonomo M1, M2 o M3; indice `report/report.md` | Quando esistono risultati verificati |

Documenti storici iniziali conservati, ma non fonti dello stato operativo. Non aggiornare ogni scheda iniziale a ogni decisione.

## Contratto minimo di una run

Creare le cartelle di run quando parte un esperimento, con ID univoci, per esempio `E001-20260911T093000Z-bm25`. Non creare file vuoti che sembrino risultati.

- `manifest.json`: stato running/completed/failed, timestamp UTC, experiment_id, codice (commit se disponibile o hash sorgenti), dipendenze, hardware, seed, ID/hash dei dati e split, modello/revisione, configurazione completa, riferimenti a input/output. Se manca una misura, null con motivo, mai zero inventato.
- `results.jsonl`: risultati per query con ID passaggi, posizioni, punteggi e tempi. Nelle run generative includere passaggi effettivamente inviati, prompt o suo riferimento immutabile, risposta, modello, impostazioni, token/costi solo se disponibili. Mai credenziali.
- `metrics.csv`: aggregati prodotti dal codice, con denominatori, split e definizione delle metriche; dettagli per query in un file dedicato se necessari.
- `events.jsonl`: eventi essenziali, errori, riprese e tempi di fase. Scrittura incrementale per conservare lavoro in caso di interruzione. Registrare stato failed senza cancellare output parziali.

La scheda dell'esperimento rimanda alla run. Il report rimanda alla scheda o agli artefatti pubblicabili. Questo percorso permette di risalire da un numero al metodo che lo ha prodotto. Niente framework di tracking remoto necessario all'inizio.

Per le prove cache registrare stato freddo/caldo, chiave/versioni, hit/miss, invalidazione e costo delle fasi effettivamente eseguite. Per agenti: chiamate strumenti, esiti, limiti e arresti. Per wiki: snapshot fonti, modello/prompt di compilazione, riferimenti alle fonti originali, tempo/token di costruzione e aggiornamento separati dalle interrogazioni. Non attribuire risparmi a operazioni mai misurate.

## Figure che aggiungono informazione

La prima figura schematica è in `percorso/00-il-problema.md`. Le altre nascono quando servono a spiegare una domanda precisa. Una figura deve mostrare una relazione, un confronto o un errore che il testo da solo rende più faticoso capire.

| Figura possibile | Informazione aggiunta | Quando crearla |
|---|---|---|
| Ricerca e risposta, due punti di errore | Dove controllare fonte trovata e affermazione generata | Primo incontro: schema Mermaid già pronto |
| Parole simili, fonti diverse | Perché un identificatore o una negazione cambia la pertinenza | Dopo gli esempi di embedding |
| Qualità rispetto a memoria/tempo | Compromesso tra configurazioni, con unità e stesso compito | Dopo il confronto misurato |
| Una domanda, tre classifiche | Quale evidenza trova o perde ogni metodo | Analisi di un caso reale |
| Fonti corrette contro fonti recuperate | Separare l'errore della ricerca da quello del generatore | Tappa RAG |

Preferire SVG per diagrammi e grafici statici, PNG per screenshot quando utile. Diagrammi semplici in Mermaid; figure basate su dati generate da uno script in `scripts/figures/`, leggendo le run. Non usare immagini generate dall'AI per inventare grafici o tracce. Una proiezione 2D di embedding reali altera distanze: dichiararlo e non usarla come prova della qualità di ricerca.

Per ogni figura conservare in `report/figure/INDEX.md`, quando nasce la prima figura pubblicabile: domanda a cui risponde, tipo (schema didattico / dato misurato), fonte/run, script o sorgente, breve didascalia e testo alternativo. Coordinate e numeri inventati sono sempre etichettati.

Prima della consegna, renderizzare e guardare ogni figura a dimensione reale e ridotta: testo leggibile, niente tagli, unità esplicite, colori distinguibili, eventuale incertezza e denominatori. Conservare sorgente modificabile oltre all'esportazione. Le figure non ancora verificate non sono pronte per il sito.

## Pubblicazione

Una consegna per parte: `report/m1-ricerca/report.md`, `report/m2-rag/report.md`, `report/m3-agent-wiki/report.md`. `report/report.md` ne sarà l'ingresso comune, senza ricopiare i risultati. Un'unica `report/CONSEGNA.md` chiarirà cosa è pronto per il sito. Nessun report vuoto o con miglioramenti promessi. Studio e appunti tecnici restano separati e collegabili come approfondimenti.

Sul sito le tre consegne diventano **un solo progetto con tre moduli**; la sintesi deve bastare per capire competenze, risultati e limiti. La separazione dei file non determina il numero delle sezioni pubbliche. Seguire [PORTFOLIO](PORTFOLIO.md) e [COMPETENZE](../percorso/COMPETENZE.md) per navigazione e dichiarazioni verificate.

Non scrivere in anticipo una storia di miglioramento. Preparare il report quando esistono almeno prima misura e una diagnosi. Struttura: problema e domanda → banco di prova → confronto → errori che spiegano le scelte → risultato e limiti. Indicativamente 1.200–1.600 parole, ma la chiarezza prevale sul conteggio.

`report/CONSEGNA.md` elenca materiali, sintesi home, figure, approfondimenti e stato editoriale. La sessione del sito riceve report e consegna, non diario/studio. Dati e codice riproducibili accessibili separatamente; verificare cosa è redistribuibile prima di pubblicarlo. Questo progetto non cambia l'HTML del portfolio.

Prima di consegnare: ogni numero rintracciabile; esempi senza selezione ingannevole; limiti del dominio/campione espliciti; distinzione tra ciò che Andrea ha realizzato e ciò che proviene da librerie o dataset; nessuna submission dichiarata se non avvenuta. L'esito nullo o negativo resta un risultato valido.
