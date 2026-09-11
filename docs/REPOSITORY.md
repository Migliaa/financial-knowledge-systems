# Repository e perimetro corrente

11 settembre 2026. Andrea vuole conservare il sito e concentrarsi su M1.

## Decisione

Una repository per il sistema finanziario, tre traguardi M1/M2/M3. Una repository separata per il mockup del sito. Nessun submodule e nessuna duplicazione del motore di ricerca fra tre repository.

| Repository GitHub | Cartella da collegare | Responsabile | Stato |
|---|---|---|---|
| `Migliaa/financial-knowledge-systems` | `C:/Users/andre/Desktop/Progetti/rag` | Questa sessione, poi Sol per il percorso | Git locale e origin collegati all'URL fornito da Andrea; branch main |
| `Migliaa/portfolio-redesign` | `C:/Users/andre/Desktop/Progetti/rag/mockup-portfolio` | Centralina del sito o sessione dedicata al mockup | Repository Git indipendente, esclusa da quella padre; branch main |
| `Migliaa/SitoPersonale` | `C:/Users/andre/Desktop/Progetti/SitoPersonale` | Centralina esistente | Remote origin verificato, non modificato |

Andrea ha creato e fornito i due URL, autorizzando collegamento e caricamento. Il commit iniziale e la licenza MIT presenti in financial-knowledge-systems sono conservati come base; portfolio-redesign era vuota. Nessuna modifica di visibilità o comunicazione ad altre sessioni. Il caricamento Git conserva sorgenti e documentazione, non pubblica un sito navigabile. Verificare l'allineamento main/origin prima di dichiarare concluso ogni push.

`mockup-portfolio/` e `backups/` sono ignorati dalla repository RAG. Il mockup possiede un proprio Git e uno snapshot dei testi; può essere spostato in seguito senza perdere la ricostruzione. I backup ZIP restano copie locali separate dai sorgenti versionati. Le sessioni del corso non devono modificare il sito senza richiesta.

## M1 adesso

Ambito: U00–U09, comprensione del codice e confronto BM25/embedding/ibrida, reranking mirato e storia conversazionale. U10b Chroma appartiene all'ingresso successivo in M2. M1 si chiude con codice eseguibile, misure, errori e report; non aspetta generatore, API, cache, agenti o wiki.

La cartella corrente è già il workspace di M1. Non creare ora tre cartelle di applicazioni vuote. Quando servono, introdurre `src/`, `tests/`, configurazioni e script secondo ARCHITETTURA. Report e schede mantengono gli ID già stabiliti; non rinumerare unità o duplicare il piano.

Alla chiusura effettiva di M1: commit identificabile e tag proposto `m1-v1.0`; poi M2 estende lo stesso codice. Analogo per `m2-v1.0` e `m3-v1.0`. Non creare tag di completamento in anticipo. Branch temporanei per modifiche, non tre branch permanenti divergenti.

Separare un modulo in futuro solo se diventa un prodotto riusabile con dipendenze e ciclo di rilascio propri. Finché M2 e M3 dipendono direttamente dal recupero e dai dati M1, la separazione non serve.

## Ripartenza con Sol

Leggere AGENTS, HANDOVER e la prima lezione. Non lavorare sul mockup. Iniziare U00 e verificare le basi Python in U01. GitHub non è un prerequisito per il primo incontro; il caricamento remoto non deve sostituire l'apprendimento.
