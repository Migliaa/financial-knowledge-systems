# Guida docente M1 — insegnare, costruire, verificare

> **SUPERATA COME SEQUENZA OBBLIGATORIA.** Per richiesta di Andrea del 12 settembre si segue PIANO_MASTER, fasi D1–D6 orientate al design applicativo. Queste scalette rimangono un archivio di esempi; non imporre loader Python, quiz di sintassi o U02 prima del confronto embedding. La prossima fase è D1. I criteri di esercizio sotto valgono soltanto se Andrea richiede quel dettaglio.

Questa è una scaletta operativa per SOL, non una dichiarazione di lezioni svolte. Si affianca a PIANO_MASTER (sequenza), COMPETENZE (prove) e HANDOVER (stato). Obiettivo: Andrea comprende e modifica il motore di ricerca, non riceve semplicemente un progetto funzionante. U00–U09 sono il perimetro corrente.

## Regola di ogni incontro

Aggiornamento di ritmo del 12 settembre: prevalgono le nuove regole AGENTS. Raggruppare spiegazione, codice ed esercizio in blocchi coerenti; evitare un concetto per messaggio e attese su micro-domande. U00 è completata; ripresa corrente U01, con teoria anticipata in Appunti1/Appunti2 e dubbio U02 non consolidato. Le scalette sotto restano riferimenti, non impongono di ripetere ciò che Andrea ha già compreso.

Apri con dove siamo e quale problema risolviamo oggi, in due frasi. Introduci al massimo pochi concetti nuovi. Alterna spiegazione, esempio, previsione di Andrea e codice. Non presentare subito la soluzione dell'esercizio. Aspetta la sua risposta quando il passaggio dipende da essa; usa il tempo indipendente solo per preparazione reversibile.

Per una funzione: mostra un input concreto, chiedi l'output atteso, percorri le trasformazioni e fai modificare un comportamento. Non spiegare ogni dettaglio del framework se non serve. Aiuti graduati: domanda orientativa → suggerimento → esempio analogo → soluzione discussa. Registrare l'aiuto effettivamente ricevuto.

Chiusura: Andrea spiega una scelta con parole proprie e applica un piccolo cambiamento. Se non riesce, tornare all'esempio; se riesce, non imporre altri esercizi equivalenti. Annotare un dubbio preciso e il punto da cui riprendere. Mai considerare «ok, continua» una dimostrazione di comprensione.

## U00 — Il problema, prima degli strumenti

Materiale: 00-il-problema.md. Leggere i tre documenti Aurora/Nebbia, presentare la domanda sul costo di apertura e chiedere quale fonte serva. Non dare subito la risposta presente nelle note docente.

Atteso: scegliere per contenuto, non per la sola parola Aurora. Se Andrea sceglie il documento sui prelievi, distinguere entità corretta e operazione sbagliata. Introdurre ricerca e risposta come due operazioni separate. Vettore ed embedding solo come anticipazione motivata, senza formule.

Trasferimento: proporre una domanda sulla chiusura del conto, non coperta dalle fonti. Andrea riconosce che manca l'informazione. Non installare modelli per dimostrare questa idea. Nota: fonte pertinente non significa conoscenza completa.

## U01 — Python necessario al progetto

Verifica iniziale: dare una lista di tre dizionari con id/testo e chiedere come trovare un ID. Se liste e funzioni sono già chiare, saltare l'introduzione corrispondente. Altrimenti mostrare accesso, ciclo e funzione su questo stesso esempio. Proseguire con file JSONL, una riga per record, e un record invalido.

Codice minimo: funzione load_records(path), lookup per ID e risultato prevedibile per ID assente. Spiegare return, eccezione e differenza fra dato mancante e errore di lettura. Non costruire subito un pacchetto completo.

Esercizio: aggiungere un campo source e segnalare una riga invalida senza perdere le altre. Atteso: Andrea distingue una lista da un dizionario e sa spiegare il flusso. Se serve pratica ripetuta, un laboratorio KodeKloud Python mirato, poi applicazione al loader. Git: osservare il diff del proprio cambiamento e scrivere un commit che ne spieghi lo scopo.

## U02 — Vettori senza magia

Esempio inventato: q=[1,0], a=[2,0], b=[0,1]. Chiedere quale direzione è simile; prodotto scalare e norma arrivano dopo. Spiegare coseno e perché a può cambiare lunghezza mantenendo la direzione. Gestire il vettore nullo come caso non definito, non inventare una similarità.

Codice: calcolo iniziale trasparente su liste, poi NumPy. Mostrare matrice con tre documenti e due coordinate; distinguere numero di documenti e dimensione del vettore. Esercizio: predire la forma dei punteggi dopo confronto con una query.

Errore tipico: attribuire alle coordinate reali etichette umane come «finanza» e «costo». Il disegno è didattico, non la descrizione dei pesi del modello. Verifica: Andrea spiega perché vicinanza non implica verità e sa individuare un errore di dimensioni.

## U03 — Embedding reali

Introdurre tokenizer, encoder e rappresentazione appresa usando gli stessi testi brevi. Consultare la scheda del candidato BGE e fissare revisione/istruzione query prima della prova. Non scaricare più modelli per mostrare una differenza che non sappiamo ancora interpretare.

Codice: codifica di pochi documenti e una query, forma degli array, normalizzazione e ordinamento. Distinguere Sentence Transformers (libreria), SentenceTransformer (classe) e BGE (modello). Spiegare che inferenza non è training. Visualizzare conteggio token e un caso di testo oltre limite.

Esercizio: cambiare una parola significativa/negazione e interpretare l'ordinamento ottenuto, senza promettere che il modello debba comportarsi in un certo modo. Verifica: Andrea sa indicare cosa entra e cosa esce dal modello e quale contenuto rischia di essere troncato.

## U04 — Un confronto che abbia senso

Prima dei dati completi: tre query, piccoli giudizi di pertinenza e una classifica. Spiegare corpus, query e qrels, separando ciò che il sistema vede da ciò che usa il valutatore. Una conversazione non deve essere spezzata ingenuamente fra sviluppo e test.

Acquisizione: provenienza/revisione/licenze, ID, conteggi, copertura dei riferimenti, hash e split. Fissare le scelte prima di leggere il test. Pilot CPU secondo ARCHITETTURA; mostrare formula di estrapolazione e incertezza, senza trasformare una stima in misura completa.

Esercizio: trovare un ID mancante e riconoscere un caso di leakage. Atteso: non passare le risposte attese al retriever. Se il carico è troppo alto usare i fallback già approvati, conservando lo stesso concetto didattico.

## U05 — BM25 e metriche

Esempio: due testi condividono la parola conto, soltanto uno contiene l'identificatore cercato. Introdurre frequenza delle parole, rarità nel corpus e normalizzazione della lunghezza senza derivare tutto in anticipo. Spiegare che BM25 è un algoritmo, non una libreria.

Prima metrica: due fonti pertinenti, solo una nei primi risultati → Recall=1/2. Poi mostrare perché l'ordine conta e introdurre DCG/nDCG con un esempio numerico piccolo. Distinguerle dall'accuratezza delle risposte generative.

E01: baseline riproducibile, criteri fissati prima, output per domanda. Andrea legge almeno un successo e un errore e cambia un aspetto didattico del tokenizzatore fuori dal test ufficiale. Misurare senza ottimizzare ogni parametro.

## U06 — Ricerca semantica su corpus

Riutilizzare il codice embedding già compreso. Mostrare salvataggio dei vettori e mappa riga→ID: non ricodificare l'intero archivio per ogni domanda. Codificare a batch e verificare cache/versioni. Ricerca esatta NumPy prima di introdurre un database.

E02: stesso corpus/query/split della baseline. Verifica: Andrea individua una query risolta grazie a una parafrasi e un identificatore o numero confuso dalla similarità. Non selezionare soltanto esempi favorevoli. Interpretare separatamente qualità, costo una tantum e latenza della query.

## U07 — Fusione ibrida

Mostrare due classifiche brevi con ID sovrapposti. Perché sommare i punteggi grezzi può essere arbitrario? Calcolare un esempio RRF usando le posizioni, poi leggere la funzione reale.

E03: stessa domanda, fusione delle classifiche, deduplicazione. Esercizio: un ID presente in entrambe le liste e uno assente da una; spiegare come vengono trattati. Un pareggio deve avere una regola riproducibile. Parametri fissati o scelti su sviluppo; non inseguire il test.

## U08 — Reranking

Disegnare il contrasto fra testi codificati separatamente e coppie domanda/passaggio lette insieme. Il cross-encoder non sostituisce gratis la ricerca sull'intero corpus: costa una valutazione per ogni coppia.

E04 su top 20, ritorno top 10: confronto con e senza reranker; candidato distinto dal modello all-MiniLM. Esercizio: spiegare perché una fonte non recuperata fra i candidati non può essere salvata dal reranker. Adottare solo se il compromesso è utile; esito negativo documentabile.

## U09 — Conversazioni e chiusura M1

Esempio: prima domanda su un conto, seconda «e per prelevare?». Distinguere ultima domanda isolata e storia disponibile, senza includere informazioni future o risposte gold non disponibili in uso reale.

E05: cambiare solo il trattamento della storia su configurazione congelata. Andrea spiega una correzione e un caso in cui la storia distrae. Le riscritture ufficiali non sono un componente gratuito del nostro sistema.

Chiusura: riprodurre una run, risalire da un numero alla configurazione, spiegare Recall/nDCG e un errore, modificare una funzione piccola. Scrivere report con risultati e limiti veri. Non avviare M2 per evitare di concludere M1. La comprensione registrata distingue spiegato, provato e spiegato autonomamente da Andrea.

## Risparmio di contesto e ripresa

All'inizio leggere soltanto AGENTS, HANDOVER, la sezione corrente di questa guida e i file coinvolti. Consultare il piano completo quando serve orientarsi. Non rileggere tutto il sito o la storia delle scelte a ogni incontro.

Quando Andrea dice «chiudiamo qui», aggiornare HANDOVER con unità, concetto verificato, esercizio svolto, difficoltà e prossimo passo esatto. Una riga nel DIARIO e link alla nota/run. Non creare un nuovo handover parallelo e non chiedere ad Andrea di imparare una skill per salvare il progresso.
