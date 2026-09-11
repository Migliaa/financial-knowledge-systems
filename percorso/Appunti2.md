# Appunti 2 — preparazione teorica a U01, U02, U03

> Stato del 12 settembre: Andrea dichiara studiati e compresi questi appunti, **eccetto un punto di U02**, da identificare e riprendere a quella tappa. Nessun esercizio pratico ancora documentato. Rettifiche redazionali del 12 settembre su dict, memoria del loader e istruzione BGE: da richiamare quando pertinenti, senza presumere che siano già state studiate.

Lettura di preparazione, non sostituisce l'esercizio dal vivo (che resta da fare insieme, con
previsione e modifica di Andrea). Presuppone `Appunti1.md` già letto: qui non si ripete perché
serve un embedding, solo *come si scrive* il codice che lo produce e lo usa.

## U01 — Python per il loader dei dati

Il compito concreto: leggere un file **JSONL** (JSON Lines) — un file di testo dove **ogni riga è
un oggetto JSON indipendente e completo**, a differenza di un unico grande array JSON. Si legge
riga per riga, non tutto in un colpo, ed è per questo che il formato è comune per dataset grandi:
si può processare una riga alla volta senza tenere tutto in RAM.

Esempio di due righe di un file `corpus.jsonl`:

```
{"id": "D1", "text": "L'attivazione del conto Aurora non prevede una commissione iniziale."}
{"id": "D2", "text": "Per ogni prelievo con la carta Aurora viene applicata una commissione di due euro."}
```

Ogni riga, una volta letta, diventa in Python un **dizionario** (`dict`): coppie chiave-valore,
`{"id": "D1", "text": "..."}` — si accede con `record["id"]` o `record["text"]`. Una raccolta di
record è una **lista** (`list`) di dizionari: `[{"id": "D1", ...}, {"id": "D2", ...}]`. Ricordare
la differenza: la lista ha un **ordine** e si accede per posizione (`records[0]`), il dizionario
conserva l'ordine di inserimento (garantito da Python 3.7), ma si accede per **chiave**
(`record["id"]`), non per posizione. Fonte: [documentazione Python](https://docs.python.org/3/library/stdtypes.html#dict).

**Leggere il file riga per riga** in Python:

```python
import json

def load_records(path: str) -> list[dict]:
    records = []
    with open(path, "r", encoding="utf-8") as f:
        for line in f:
            line = line.strip()
            if not line:
                continue  # riga vuota, la saltiamo
            try:
                record = json.loads(line)   # da stringa JSON a dizionario Python
            except json.JSONDecodeError:
                continue  # qui la saltiamo soltanto; la segnalazione va aggiunta nell'esercizio
            records.append(record)
    return records
```

Punti da capire, non solo da leggere: `with open(...)` apre il file e lo chiude automaticamente
anche in caso di errore (senza `with` rischi di lasciare il file aperto). Il ciclo `for line in f`
legge una riga alla volta, senza caricare l'intero file in memoria insieme. `try/except` cattura
un solo tipo di errore atteso (riga JSON malformata) e lo gestisce **senza far crashare l'intera
lettura** — una riga corrotta non deve perdere le altre 9999 buone. `return` restituisce il
risultato al chiamante; una funzione senza `return` esplicito restituisce `None` in Python (non un
errore, ma un valore "vuoto" — causa comune di bug se te lo aspetti diverso).

**Cercare un ID specifico** (funzione di lookup):

Precisazione sul loader precedente: il ciclo legge una riga per volta, ma `records.append`
conserva tutti gli oggetti in memoria fino al return. Non è quindi un elaboratore a memoria
costante. Inoltre `json.loads` non garantisce un dizionario con id/text validi: la validazione
dello schema e la registrazione delle righe scartate fanno parte del lavoro U01.

```python
def find_by_id(records: list[dict], target_id: str) -> dict | None:
    for r in records:
        if r["id"] == target_id:
            return r
    return None  # ID non trovato: valore esplicito, non un errore silenzioso
```

Distinzione importante che il progetto chiede esplicitamente di saper spiegare: **dato mancante**
(`find_by_id` restituisce `None` perché quell'ID semplicemente non esiste nel corpus — normale,
va gestito dal chiamante) contro **errore di lettura** (una riga del file era corrotta — un
problema nei dati stessi, da segnalare/loggare, non da confondere con un ID assente).

## U02 — da liste a NumPy

Il calcolo del coseno visto in Appunti1.md, fatto con liste Python semplici, funziona ma è lento
su tanti numeri. **NumPy** è la libreria standard per calcolo numerico: rappresenta vettori e
matrici come `ndarray`, con operazioni elemento-per-elemento molto più veloci (eseguite in codice
compilato, non in un ciclo Python interpretato).

```python
import numpy as np

q = np.array([1.0, 0.0])          # vettore, shape (2,)
docs = np.array([[2.0, 0.0],      # matrice, shape (3, 2) — 3 documenti, 2 dimensioni ciascuno
                  [0.0, 1.0],
                  [1.5, 0.5]])
```

**Shape** (forma) è la dimensione di un array: `q.shape` → `(2,)` (vettore a 2 elementi);
`docs.shape` → `(3, 2)` (3 righe, 2 colonne — 3 documenti, ciascuno rappresentato da un vettore a
2 dimensioni). Non confondere le due cose: 3 è il **numero di documenti**, 2 è la **dimensione**
del vettore di ciascuno — cambiando modello di embedding cambia la seconda (384 per BGE-small),
mai la prima (dipende solo da quanti documenti hai).

Confrontare una query con **tutti** i documenti insieme, senza un ciclo esplicito:

```python
# Normalizzo ogni riga a norma 1 (norma calcolata lungo l'asse delle colonne, axis=1)
docs_norm = docs / np.linalg.norm(docs, axis=1, keepdims=True)
q_norm = q / np.linalg.norm(q)

scores = docs_norm @ q_norm   # prodotto matrice-vettore: shape (3,) — un punteggio per documento
ranking = np.argsort(-scores)  # indici che ordinano scores dal più alto al più basso
```

`@` è l'operatore di prodotto matrice-vettore (equivalente a fare il prodotto scalare riga per
riga, ma con un'unica istruzione, molto più veloce di un ciclo `for`). `np.argsort` restituisce
gli **indici** che ordinerebbero l'array, non i valori — con il meno davanti a `scores` si ottiene
l'ordine decrescente (dal più simile).

**Predizione da fare dal vivo, non ora**: data una query e 3 documenti con coordinate note, quale
sarà `ranking`? È l'esercizio previsto in U02 — qui do solo lo strumento, non la risposta.

## U03 — embedding reali con sentence-transformers

La libreria fa da involucro attorno a tutta la catena vista in Appunti1.md (tokenizzazione →
tabella → forward pass → pooling → normalizzazione), esposta con un'unica chiamata:

```python
from sentence_transformers import SentenceTransformer

model = SentenceTransformer("BAAI/bge-small-en-v1.5")

docs = ["Opening the Aurora account has no initial fee.",
        "Every withdrawal with the Aurora card incurs a two-euro fee."]

doc_vectors = model.encode(docs, normalize_embeddings=True)
# doc_vectors.shape → (2, 384): 2 documenti, 384 dimensioni ciascuno — coerente con U02

query_vector = model.encode(
    "Represent this sentence for searching relevant passages: How much does it cost to open Aurora?",
    normalize_embeddings=True,
)
# query_vector.shape → (384,): un solo vettore, non una matrice
```

Tre dettagli specifici di questo modello, da tenere a mente perché non sono universali:

- **`normalize_embeddings=True`**: fa fare alla libreria la normalizzazione a norma 1 (vista in
  Appunti1.md) durante l'`encode`, così dopo basta il prodotto scalare (`@`), non serve rifare la
  divisione per la norma a mano come nell'esempio NumPy sopra.
- **Prefisso di istruzione sulla query**: la scheda BGE v1.5 raccomanda per query brevi e
  passaggi lunghi `"Represent this sentence for searching relevant passages: "`, solo sulle query.
  Non è un requisito per far funzionare il modello: v1.5 supporta anche l'uso senza istruzione.
  Nel nostro confronto fissiamo e registriamo la scelta; il suo effetto va misurato sui dati,
  non assunto. Fonte: [scheda ufficiale BGE](https://huggingface.co/BAAI/bge-small-en-v1.5).
- **Limite di 512 token**: testo più lungo viene troncato automaticamente dal tokenizer, senza
  avviso esplicito di default. Per i passaggi brevi di FiQA non dovrebbe essere un problema, ma è
  una verifica da fare quando si carica il corpus vero (U04), non da assumere.

`model.encode(...)` accetta sia una stringa singola (restituisce un array 1D) sia una lista di
stringhe (restituisce una matrice 2D, una riga per testo) — usare sempre la forma a lista quando
si codificano molti documenti insieme (**batch**), è molto più efficiente che chiamare `encode`
una volta per documento in un ciclo.

## Cosa serve installare (non ancora fatto)

`pip install sentence-transformers` dentro l'ambiente del progetto (mai globale). La prima
chiamata a `SentenceTransformer("BAAI/bge-small-en-v1.5")` scarica il modello da Hugging Face
(~130MB) e lo mette in cache locale; le chiamate successive lo riusano senza riscaricarlo. Va
confermato prima di eseguirlo per davvero, come per ogni installazione.
