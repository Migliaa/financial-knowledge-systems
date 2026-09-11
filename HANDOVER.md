# Passaggio a Sol

Ultimo aggiornamento: 11 settembre 2026.

## Decisione e stato

Il piano corrente è [PIANO_MASTER.md](PIANO_MASTER.md). Sostituisce la sequenza della precedente analisi strategica. Andrea è l'utilizzatore: niente ricerca di clienti, feedback esterno o candidatura come requisito di completamento.

Tre macroprogetti sequenziali, con codice condiviso e consegne autonome:
1. M1: ricerca lessicale, embedding, ricerca ibrida, reranking e conversazioni su MTRAG FiQA.
2. M2: assistente RAG personale, citazioni, diagnosi degli errori, caching e servizio locale minimo.
3. M3: confronto delimitato tra RAG, ricerca agentica su file e wiki compilata dalle fonti.

M3 è una parte pianificata con un controllo di fattibilità, non una promessa di prestazioni sul portatile. Fine-tuning: spiegazione prevista; addestramento pratico facoltativo dopo una misura di necessità e fattibilità. Alternativa B più leggera già autorizzata se le misure lo richiedono.

**Stato corrente: U00, non iniziata.** Esistono soltanto documenti di pianificazione. Nessun dato/modello scaricato, dipendenza installata, esperimento, risultato o submission. Python/uv presenti, compatibilità non verificata. BGE-small-en-v1.5 è il candidato iniziale per gli embedding, non una dipendenza già validata.

**Aggiornamento portfolio e competenze:** [COMPETENZE](percorso/COMPETENZE.md) definisce 13 capacità, esercizi e prove prima delle dichiarazioni pubbliche. Chroma: laboratorio U10b/L01 dopo M1; Redis: L02 facoltativo; FastAPI/Pydantic scelti per U16. [PORTFOLIO](docs/PORTFOLIO.md) definisce un solo progetto pubblico con tre moduli. Andrea ha autorizzato il mockup in `mockup-portfolio/`; sito sorgente preservato, testi protetti importati identici. Esiste il mockup HTML, non il sistema RAG. Non proseguire a rifare il sito quando si passa a Sol: iniziare U00.

## Primo turno

**Corso esplicito:** seguire [GUIDA_DOCENTE_M1](percorso/GUIDA_DOCENTE_M1.md), che contiene scalette U00–U09, esempi, esercizi, errori tipici e criteri di passaggio. Per Andrea: [INIZIA_CON_SOL](INIZIA_CON_SOL.md). Nessuna unità ancora svolta; non interpretare la pianificazione o il lavoro Git come lezione completata.

**Focus confermato:** soltanto M1 (U00–U09). Mockup isolato con snapshot autonomo e ZIP in backups; non riprendere il design. Repository `financial-knowledge-systems` per M1/M2/M3 e `portfolio-redesign` indipendente per il sito, collegate agli URL forniti da Andrea. Dettagli in [REPOSITORY](docs/REPOSITORY.md). Prima azione didattica sempre U00.

1. Leggi AGENTS, questo file, la sezione della tappa in PIANO_MASTER e [primo incontro](percorso/00-il-problema.md). Non rileggere tutto l'archivio.
2. Spiega il prodotto finale in poche frasi e parti dai tre documenti inventati del primo incontro: quale fonte serve per rispondere?
3. Fai ragionare Andrea prima di introdurre formule. Non presumere competenze Python: verificane poche con un esercizio U01.
4. Segui il ciclo problema → concetto → lettura di una piccola funzione → previsione → modifica di Andrea → verifica → nota.
5. Non implementare l'intera pipeline prima che Andrea ne abbia seguito i passaggi. Puoi preparare infrastruttura reversibile, senza saltare le tappe didattiche.
6. In U04 misura CPU, RAM e tempi su campione prima dell'indice completo; consulta ARCHITETTURA. Nessuna spesa aggiuntiva autorizzata.

## Comprensione

| Tema | Stato | Evidenza |
|---|---|---|
| Ricerca, vettori, embedding, RAG | Non affrontato insieme | Andrea dichiara di partire da zero |
| Python e lettura del codice | Da verificare | Nessun esercizio svolto qui |
| Valutazione, caching, agenti, wiki | Non affrontato insieme | Non inferire conoscenza dalle domande |
| Fine-tuning | Non affrontato insieme | Pratica non avviata |

Stati consentiti: non affrontato, spiegato, provato insieme, spiegato da Andrea. Registrare evidenza concreta, non dedurre comprensione dal silenzio.

## Vincoli e controlli futuri

24 GB RAM, Intel Iris Xe; CPU iniziale, zero euro aggiuntivi, circa 5 ore/giorno disponibili senza scadenza promessa. Laboratori KodeKloud mirati: [guida](percorso/KODEKLOUD.md), nessun corso a pagamento necessario.

U04: versioni/licenze, passaggi/qrels, split per conversazione, tokenizer/troncamento e prestazioni.
U11: generatore locale, massimo due candidati dopo pilot; se inadeguato, documentare il limite e conservare la consegna M1.
U16: API locale e client minimo; Docker solo quando utile e compatibile.
U18: piccolo corpus agentico fissato senza usare le risposte test; niente confronto spurio con il benchmark completo.

Nessun requisito di hosting o submission. Non è stata identificata una submission ufficiale aperta e pertinente; SemEval MTRAGEval 2026 è conclusa. Non confondere PR, pubblicazione propria e classifica.

## Chiusura di sessione

Aggiorna qui stato, comprensione e prossima azione; una riga nel DIARIO e la nota/scheda coinvolta. Non creare altri handover. I report nasceranno soltanto da risultati reali, secondo DOCUMENTAZIONE. La nota per la centralina è preparata, non inviata; non modificare il portfolio.
