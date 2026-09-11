# Brief — progetto "rag"

> Aggiornamento: la scelta è stata presa nelle sessioni successive. Vedere `README.md`, `AGENTS.md` e `HANDOVER.md` per il progetto corrente (MTRAG FiQA). Questo brief resta storico; alcune sue affermazioni su MTEB sono state corrette in `SCELTA.md`.

Scritto dalla centralina del sito personale per avviare questo progetto, il 2026-09-10. Non è
una decisione presa da questa sessione: è contesto, quello che c'è da imparare, e alcune strade
concrete già verificate — non un progetto già scelto. La scelta esatta (quale strada, quale
taglio, cosa lasciare fuori) spetta a chi apre questa cartella per lavorarci davvero. Andrea può
cambiare qualunque punto qui sotto in una frase.

## Perché questo progetto

`SitoPersonale/ricerca/ricerca-ai-engineer.md` (ricerca di mercato su 3.647 annunci, fatta a
settembre 2026) segna il RAG come uno dei pochi buchi veri rimasti: compare nel 40% delle
aziende e nel 35,9% dei ruoli, ed è l'unica delle quattro aree ancora scoperte in
`SitoPersonale/PROGETTI.md` ("recupero su dati propri") su cui Andrea non ha ancora niente da
mostrare.

La stessa ricerca segna il fine-tuning di un modello linguistico intero come quasi irrilevante
per l'assunzione (8,5% dei ruoli, "se compaiono sul sito tolgono spazio a cose che pesano dieci
volte tanto"). Non è un divieto — se il fine-tuning entra in questo progetto, ha senso solo
applicato a un modello di embedding piccolo, dentro il RAG stesso, non come obiettivo a sé.

## Cosa c'è da imparare

Vale per qualunque strada si scelga:

- **RAG (retrieval-augmented generation)** — dare a un modello linguistico accesso a documenti
  veri al momento della domanda, invece di farlo rispondere solo con quello che ha imparato in
  addestramento (fermo a una certa data, e senza sapere niente di documenti privati).
- **Embedding** — trasformare un testo in una lista di numeri costruita in modo che testi con
  significato simile abbiano numeri vicini. È il motore della ricerca semantica dentro un RAG:
  senza, si potrebbe cercare solo per parole esatte.
- **Spezzettamento (chunking)** — come si taglia un documento lungo in pezzi cercabili. La
  dimensione e il criterio del taglio cambiano cosa si trova dopo, ed è un problema meno banale
  di quanto sembri.
- **Ricerca ibrida** — combinare ricerca per parola esatta (es. BM25) con ricerca per
  significato (embedding): spesso batte entrambe prese da sole.
- **Misurare il recupero** — prima ancora di guardare la risposta finale del modello, si misura
  se i pezzi di testo giusti sono stati trovati (metriche come recall@k, precision, nDCG). È
  esattamente quello che la riga già scritta in `PROGETTI.md` chiama "la misura di quanto è
  stato trovato" — e riusa lo stesso istinto già allenato con τ²-bench: non fidarsi
  dell'impressione, misurare con ripetizioni.
- *(Solo se la strada scelta lo prevede)* **Fine-tuning di un modello di embedding** —
  specializzare un embedding generico su un dominio specifico, e misurare se batte quello
  generico sullo stesso compito. È fine-tuning vero, ma nella forma in cui il mercato lo chiede
  davvero (vedi sopra).

## Strade possibili — verificate il 2026-09-10, da scegliere

Non sono in ordine di preferenza.

1. **Un benchmark pubblico, senza submission formale, sullo stesso schema di `tassonomia`.**
   Si costruisce una pipeline RAG sopra un insieme di domande/documenti pubblico e riconosciuto
   (per esempio un sottoinsieme di **BEIR**, la suite standard per valutare il recupero puro), si
   confrontano scelte diverse di spezzettamento e ricerca, si misura con ripetizioni come già
   fatto con τ²-bench, si scrive il report. Nessuna procedura di invio: il benchmark stesso,
   essendo pubblico, è già la prova. È l'opzione più sicura sui tempi, perché non dipende da
   nessuno fuori da questa cartella.

2. **MTEB — la classifica pubblica per gli embedding.**
   `huggingface.co/spaces/mteb/leaderboard`, la più riconosciuta del settore (oltre 5.000
   risultati). Oggi sottomettere un risultato richiede pubblicare un modello di embedding che
   funziona davvero, non solo un punteggio — quindi questa strada ha senso solo insieme al pezzo
   di fine-tuning di un embedding descritto sopra. È l'opzione con più peso da mostrare a un
   recruiter, perché compare un link pubblico verificabile col proprio nome, ma anche la più
   lunga e la meno garantita (bisogna prima far funzionare il modello).

3. **CRAG / KDD Cup — controllato, non disponibile adesso.**
   La competizione di Meta sul RAG (Comprehensive RAG Benchmark) ha avuto edizioni nel 2024 e
   nel 2025 con submission pubbliche vere. L'edizione 2026 risulta ancora in fase di proposta a
   settembre 2026 — non accetta invii. Il dataset e il codice restano comunque pubblici su
   GitHub (`facebookresearch/CRAG`) e si possono usare come base per l'opzione 1, senza aspettare
   che la competizione riapra.

4. **Open-RAG-Eval (Vectara) come strumento, non come traguardo.**
   Framework open source, mantenuto da Vectara (la stessa azienda dietro la classifica pubblica
   sulle allucinazioni), pensato apposta per misurare una pipeline RAG costruita da terzi. Non
   dà un link pubblico col proprio nome, ma è un nome riconosciuto nel settore da citare nel
   report — utilizzabile dentro l'opzione 1 per rendere la misurazione più solida invece di
   inventare metriche da zero.

Le opzioni non si escludono a vicenda: per esempio, si può partire dall'opzione 1 e, se il tempo
lo permette, estendere verso la 2 in un secondo momento — ma va deciso all'inizio quanto tempo si
vuole dare al progetto, non scoperto strada facendo.

## Cosa NON deve essere

- Non un progetto che insegue il fine-tuning di un modello linguistico intero: la ricerca di
  mercato del sito lo esclude esplicitamente.
- Non troppo lungo. Se la strada scelta rischia di allungarsi — per esempio mettendo la pipeline
  in produzione con un endpoint, un container, un rilascio automatico — quella parte va
  scorporata in un progetto successivo (è già un'area segnata come scoperta in `PROGETTI.md`),
  non incollata qui.

## Consegna

Per default, lo stesso formato già usato per `tassonomia` e `metodo`: un `report/report.md`
(prosa + figure) più un `CONSEGNA.md` nella stessa cartella con le istruzioni per la sessione
del sito — vedi `../metodo/report/CONSEGNA.md` come esempio più recente. Se il materiale finale
diverge da questo formato — per esempio perché la strada scelta produce soprattutto codice, o un
link a una classifica pubblica invece di un report tradizionale — va bene lo stesso: lo si dice
esplicitamente nel `CONSEGNA.md`, e la sessione del sito si adatta.
