# Integrazione nel portfolio: un ingresso, approfondimenti facoltativi

> Revisione successiva richiesta da Andrea: M1/M2/M3 è il primo progetto e il fulcro della home; τ²-bench segue con testi identici. Il registro dei fallimenti è rimosso dal sito proposto e conservato soltanto nello snapshot storico. Revisione V3 del 12 settembre: sei aree (embedding/retrieval, design RAG, agenti/wiki, caching, scelta del fine-tuning, valutazione sperimentale), ciascuna con tre capacità. Stack in un approfondimento; API e database eventuali. Fine-tuning pratico condizionale e competenze future esplicitamente previste. Le indicazioni d'ordine precedenti sotto sono superate. GitHub collegato al profilo Migliaa; LinkedIn resta da completare.

11 settembre 2026. Andrea autorizza un mockup di ristrutturazione nella cartella RAG. È un'eccezione esplicita alla vecchia regola di non lavorare sull'HTML; il sito sorgente rimane intatto e il mockup vive in `mockup-portfolio/`. Nessuna pubblicazione, invio o sostituzione del sito attuale.

## Prima lettura

Pubblico: recruiter e responsabili tecnici per graduate/internship AI engineering; possibili clienti interessati al metodo. Non inserire età o titolo di studio. Nessuna promessa di produzione aziendale senza prove.

La home deve già rispondere a: chi è Andrea, cosa sa fare, dove lo dimostra, quale risultato ha ottenuto, cosa sta imparando e come contattarlo. Non serve aprire tre progetti per ricostruire il profilo. L'obiettivo progettuale è una lettura rapida, non una durata misurata con utenti.

Ordine del mockup:
1. Nome, direzione AI engineering, sintesi concreta e tre righe di capacità con collegamento alla prova.
2. τ²-bench con testo e numeri originali, portato sopra il metodo esteso.
3. **Un'unica scheda «Sistemi di conoscenza finanziaria»**, con M1/M2/M3 come moduli e stato pianificato.
4. Registro dei fallimenti, poi tre aree di competenza con strumenti e prova/stato.
5. Metodo originale, scritti esistenti e contatti.

Nessuna foto segnaposto, griglia di progetti vuoti, barre di padronanza, nove voci di menu o collezione di loghi. Il mockup usa tipografia, contrasto e gerarchia; non inventa grafici o risultati futuri.

## Architettura dell'informazione

| Lettore | Percorso | Che cosa deve trovare |
|---|---|---|
| Recruiter | Home | Ruolo cercato, capacità, prova, risultato e stato; nessun approfondimento obbligatorio |
| Responsabile tecnico | Una pagina di progetto | Problema, scelte, valutazione, risultati e limiti; stack con impiego preciso |
| Chi vuole verificare | Collegamento diretto dalla conclusione | Codice, run e fonti pertinenti, senza navigare prima nelle lezioni |
| Andrea / lettore interessato | Approfondimenti di studio | Note per argomento e piccoli esercizi; facoltativi e fuori dal menu principale |

I tre livelli documentali sono **tipi di materiale**, non nove sezioni del sito. Si conservano tre report autonomi per facilitare la chiusura dei moduli, ma la pagina pubblica li sintetizza in un solo progetto. Collegamenti diretti ad ancore o prove, niente catena home → indice → sottoindice → report → cartella.

Nella pagina conoscenza: sintesi visibile per tutti i moduli; dettagli di evidenza/studio espandibili nello stesso posto. Nessun report scaricabile o repository finto. Quando esistono artefatti, sostituire i dettagli pianificati con conclusioni misurate e link alla run/codice corrispondente.

## Stato delle competenze e promozione

Nel mockup: grigio + parola «Previsto»; il colore non è l'unico indicatore. Lo stato della scheda generale è «Pianificato». Dopo M1: scheda «In sviluppo — ricerca valutata», M1 con risultati, M2/M3 ancora previsti. Dopo M2: «Assistente locale», con perimetro. Dopo M3: confronto documentato; non «produzione».

Usare [COMPETENZE](../percorso/COMPETENZE.md) per decidere che cosa promuovere. Un pacchetto installato non basta. Non anticipare training, cloud, CI/CD, Redis o Docker se non esercitati. BGE e MiniLM sono nomi di modelli, non due competenze diverse.

Nel primo livello mostrare 3–5 strumenti pertinenti per progetto. Il dettaglio dei checkpoint, parametri e dipendenze sta nella pagina tecnica. Le librerie aiutano a riconoscere lo stack; la descrizione di cosa Andrea ha fatto è la prova.

## Contenuti protetti e consegna

Vincolo di Andrea: nessuna modifica alle parole di τ²-bench e della spiegazione sulle automazioni. Il mockup importa integralmente scheda τ²-bench e sezione metodo dalla home; copia i body completi di tassonomia.html e metodo.html, aggiungendo soltanto uno stylesheet nel head. Anche il registro dei fallimenti è conservato. La verifica si rigenera con `mockup-portfolio/build_mockup.py` e viene registrata in `preservation-check.json`.

Il nuovo testo introduttivo evita di trasformare il metodo descritto nell'articolo in una dichiarazione di esperienza aziendale già verificata. Non cambiare comunque l'articolo senza una nuova istruzione esplicita.

La home sorgente contiene link contatto segnaposto. L'indirizzo email reale è però già presente nell'articolo registro-dei-fallimenti: è riusato nel mockup. GitHub/LinkedIn restano dichiarati da collegare, senza inventare profili. dev.to è collegato ai riferimenti già presenti. Nessun invio di messaggi o candidatura.

La centralina riceverà questa proposta, il mockup e la matrice competenze tramite Andrea. Prima di adozione: collegare recapiti reali, verificare il rendering nel sito finale e mantenere testi protetti. Il mockup non richiede di cambiare framework: è HTML/CSS statico, coerente con l'attuale sito.
