# Appunti 1 — come funziona l'embedding nella fase di retrieval

> Revisione tecnica del 12 settembre: corretto il pooling di BGE e precisata nDCG. Queste rettifiche non sono ancora state discusse con Andrea; la comprensione dichiarata si riferiva alla versione studiata.

Note di sintesi, scritte dopo una sessione di domande e correzioni su U00-U03. Obiettivo: poter
tornare qui invece di rifare tutto il ragionamento da capo. Non sostituisce le unità del percorso
(`GUIDA_DOCENTE_M1.md`), è un riferimento rapido su ciò che è stato già chiarito.

## 0. Vocabolario di base della ricerca (IR)

- **Retriever**: il componente che, data una domanda, restituisce un elenco ordinato di passaggi
  dal corpus ritenuti pertinenti. È il nome generico del "motore di ricerca".
- **Corpus**: l'insieme fisso di passaggi (brevi porzioni di testo, già segmentati ufficialmente
  in MTRAG/FiQA) su cui si cerca.
- **Qrels** (query relevance judgments): per ogni domanda, l'elenco ufficiale degli ID di
  passaggio considerati corretti. Servono solo per **misurare** la qualità del retriever, non
  vanno mai dati in input al retriever stesso.
- **Split**: divisione dei dati in sottoinsiemi (es. sviluppo/test), per non tarare le scelte
  sugli stessi dati su cui poi si riporta il punteggio finale.
- **Cutoff**: il K in "primi K risultati" (es. il 10 in nDCG@10). Va tenuto fisso quando si
  confrontano metodi diversi.
- **Recall@K**: quota dei passaggi corretti (secondo i qrels) effettivamente presenti nei primi K
  risultati. Non guarda l'ordine, solo se il documento è stato trovato o no.
- **nDCG@K**: misura il guadagno della classifica, scontato per posizione e normalizzato rispetto
  alla classifica ideale. Non è Recall con l'ordine aggiunto: può anche usare gradi di pertinenza.
  Trovare un passaggio molto pertinente in alto dà più guadagno che trovarlo in basso.
- **Baseline lessicale (BM25)**: ricerca basata su corrispondenza di parole esatte, pesando di
  più le parole rare nel corpus e correggendo per la lunghezza del testo. Non capisce sinonimi.
  Punto di riferimento minimo che ogni metodo più complesso deve giustificare di battere.

## 1. Perché serve tradurre il testo in numeri

Un modello è una catena di moltiplicazioni tra matrici: riceve solo numeri in input, mai lettere.
Tutta la catena sotto è il procedimento, passo per passo, per trasformare una frase in un vettore
di numeri confrontabile con un altro.

## 2. Tokenizzazione — dal testo ai "pezzi"

Si spezza il testo in **token**: non parole intere (vocabolario impossibile da fissare) né singoli
caratteri (sequenze troppo lunghe, nessuna struttura), ma pezzi intermedi (**sub-word**) — parole
comuni restano intere, parole rare si spezzano in pezzi più piccoli già noti.

Il **vocabolario** (l'elenco fisso di tutti i pezzi ammessi, es. ~30.000 voci) si costruisce **una
sola volta**, prima che il modello esista, analizzando grandi quantità di testo (es. algoritmo
BPE: parte dai caratteri, fonde iterativamente le coppie più frequenti). Una volta fissato, non
cambia più durante l'uso del modello.

Esempio: `"aurora fee"` → `["aurora", "fee"]` (entrambe le parole esistono intere nel vocabolario
di esempio).

## 3. Da token a ID

Ogni pezzo del vocabolario ha una posizione fissa nella lista → un numero intero, l'**ID**.
`["aurora", "fee"]` → `[7481, 2986]`. Sono etichette di posizione, non portano ancora significato
(l'ID 7481 non è "più simile" al 7482 in nessun senso utile).

## 4. Da ID a vettore — la tabella di embedding

Il modello contiene una matrice (**tabella di embedding**): una riga per ogni ID del vocabolario,
ogni riga un vettore di N numeri (per BGE-small, N=384). Ottenere il vettore di un token è un
**lookup**: si legge la riga corrispondente, nessun calcolo.

Questo vettore è **statico**: la riga per l'ID di "fee" è identica in ogni frase in cui compare —
non dipende ancora dal contesto.

**Chi decide questi numeri**: sono parametri appresi durante il **training** del modello (fatto
una volta dai suoi creatori). Quando *usiamo* il modello (inferenza) questi numeri sono già fissi
e non li tocchiamo mai — né questa tabella né nessun altro strato, né per il modello di embedding
né per un eventuale modello generativo.

## 5. Le dimensioni

Il numero di colonne della tabella (384 per BGE-small) è la **dimensione** del vettore — quanti
numeri servono a descrivere un token. È una scelta architetturale del modello, fissata dai suoi
creatori, non qualcosa che decidiamo usando il modello. Le singole coordinate non hanno un
significato umano isolato interpretabile (nessuna colonna "è" la finanza): è la combinazione di
tutte a essere utile.

## 6. Il forward pass — N vettori entrano, N vettori (diversi) escono

I vettori statici di tutti i token della frase attraversano gli strati del transformer (oltre la
tabella): a ogni strato, ogni token "guarda" gli altri token della stessa frase (attenzione) e
aggiorna il proprio vettore. **Punto centrale da non confondere**: l'output del forward pass non è
un vettore unico — è ancora **N vettori**, uno per token in input, ma ora **contestuali** (il
vettore di "fee" in "Aurora fee" è diverso da quello in "Nebbia fee").

**Vettori (dati) ≠ pesi (modello)**: i pesi degli strati sono fissi durante l'uso normale
(inferenza) — mai riscritti dal passaggio di una frase. I vettori che li attraversano sono
temporanei, calcolati da zero per ogni richiesta specifica, e scompaiono a fine calcolo a meno che
non li salviamo noi in una nostra cache.

**Training vs inferenza**: nel training, l'output (dopo ulteriori passaggi, per un modello di
embedding: dopo pooling e coseno) viene confrontato con la risposta attesa, si calcola un errore, e
la **backpropagation** usa quell'errore per correggere all'indietro tutti i pesi, tabella inclusa.
Nell'uso normale (inferenza, quello che facciamo noi) c'è solo il forward pass, nessuna modifica ai
pesi.

**Generazione di testo vs retrieval**: entrambe condividono tokenizzazione→tabella→forward pass. Un
modello generativo, però, non passa mai da pooling+coseno: prende solo il vettore contestuale
dell'ultimo token e lo proietta (con uno strato finale dedicato) in un punteggio per ogni parola
del vocabolario, da cui sceglie il prossimo token da scrivere. RAG collega le due pipeline in
sequenza (prima retrieval completo, poi il testo trovato diventa input del modello generativo),
non le fonde in una sola.

## 7. Pooling — da N vettori a 1

Il forward pass lascia N vettori (uno per token), non uno per frase. Il **pooling** ricava una
rappresentazione unica. **Mean pooling**: media elemento per elemento dei vettori dei token,
escludendo il padding tramite una maschera. È una possibilità, non quella del nostro candidato:
**BGE-small-en-v1.5 usa CLS pooling**, cioè seleziona il vettore contestuale finale del token
speciale iniziale [CLS]. Quel vettore ha già attraversato gli strati che elaborano il contesto.
Fonte: [scheda ufficiale BGE, uso con Transformers](https://huggingface.co/BAAI/bge-small-en-v1.5#usage).

## 8. Confronto — cosine similarity

```
cos(q, d) = (q · d) / (‖q‖ ‖d‖)         ‖v‖ = norma di v = √(Σ vᵢ²)
```

Normalizza il prodotto scalare togliendo l'effetto della lunghezza dei vettori. Se i vettori sono
già normalizzati a norma 1, `cos(q,d) = q·d` (più veloce da calcolare). Vettore nullo → coseno non
definito, va gestito esplicitamente (mai restituire 0 come se fosse "ortogonale").

## 9. Cosa costruiamo noi, cosa resta scatola chiusa

Tokenizzazione, tabella di embedding, strati del transformer, pooling: **mai riscritti da noi**,
né per il modello di embedding né per un futuro modello generativo — si usano tramite libreria
(`sentence-transformers`, una chiamata `model.encode(...)`). Non è diverso da come si userà il
modello generativo in M2: entrambi scatole chiuse pre-addestrate.

Quello su cui lavoriamo davvero: quale modello di embedding scegliere, come indicizzare un corpus,
come cercare, come combinare metodi diversi, come misurare.

## 10. La pipeline di retrieval completa (M1)

Vedi figura: [figure/pipeline-retrieval-m1.svg](figure/pipeline-retrieval-m1.svg).

Due tracce indipendenti sulla stessa query, poi fuse:

- **Traccia lessicale (BM25)**: nessun embedding coinvolto, solo conteggi/pesi di parole →
  Classifica A.
- **Traccia semantica (embedding)**: tokenizzazione → tabella → forward pass → pooling → vettore
  query → coseno uno-contro-tutti contro i vettori documento (precalcolati una volta in fase di
  **indicizzazione**) → ordina → Classifica B.

**Fusione RRF**: combina Classifica A e B usando **solo le posizioni** (i punteggi grezzi di BM25
e coseno sono su scale incompatibili, sommarli richiederebbe pesi arbitrari) → una classifica
fusa, primi ~20 candidati.

**Reranking (cross-encoder, U08)**: per ciascuno di quei ~20, ricodifica query+documento
**insieme** (non più due vettori separati confrontati col coseno — un meccanismo diverso, dove il
modello confronta i due testi parola per parola) → nuovo punteggio → riordina → primi 10 = output
finale.

RRF e reranking non toccano mai il modo in cui si calcola un embedding: operano solo sulle
classifiche già prodotte, come strato ulteriore sopra le due tracce di ricerca.
