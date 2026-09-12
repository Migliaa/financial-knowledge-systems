# Ripresa — design ingegneristico di sistemi di conoscenza

Aggiornato il 12 settembre 2026 dopo la correzione di Andrea.

## Punto di ripartenza

**D1 del nuovo PIANO_MASTER: scelta di embedding e retrieval per un caso applicativo. Non U01 e non un esame U02.**

Andrea vuole progettare, scegliere e usare le tecnologie; l'IA si occupa dell'implementazione ordinaria. Non richiedere esercizi su JSONL, liste, sintassi o sottolibrerie. Leggere/modificare il codice che determina il comportamento di embedding, retrieval e RAG è pertinente; leggere un file non è la lezione.

U00 completata. U01 affrontata, con esercizio in percorso/u01; conferma formale non data e non necessaria per proseguire. Gli appunti teorici sono già studiati; un dubbio U02 resta aperto e va chiarito solo quando serve, senza presumere quale sia. Non cancellare il lavoro precedente, non ripeterlo.

## Primo incontro

1. Leggere AGENTS, questo file e D1 nel [PIANO_MASTER](PIANO_MASTER.md). Vecchie unità e GUIDA_DOCENTE sono riferimenti subordinati, non programma da eseguire.
2. Mostrare quattro richieste finanziarie: parafrasi, termine preciso, condizione numerica/eccezione, follow-up conversazionale. Spiegare la differenza fra somiglianza di tema e fonte che risponde.
3. Presentare MiniLM, BGE e BM25 come candidati con meccanismi e compromessi. Stessa dimensionalità non implica rappresentazione equivalente; due encoder densi non sono due famiglie totalmente diverse. Nessun modello è già vincitore.
4. Andrea formula ipotesi e criterio di scelta; l'assistente prepara campione, ambiente e primo pilot CPU. Nessun loader da far completare come esercizio.
5. Mostrare ranking/testi e poi proiezione 2D con vicini nello spazio originale; non dedurre qualità dalla bellezza dei cluster. Niente grafici con numeri inventati.
6. Proseguire a D2, RAG minimo D3 e subito confronto agente/wiki D4. API, Chroma, Redis e Docker non sono prerequisiti.

## Metodo richiesto

Spiegazione applicativa densa, alternative e schema, configurazione/codice rilevante, confronto, errori e decisione documentata. Definire i termini nuovi e ordinare i prerequisiti senza tono elementare. Non micro-domande né manuale teorico separato. Verificare che Andrea sappia motivare una scelta e cambiarla davanti ai dati; non chiedergli di riscrivere implementazioni generiche.

L'IA può implementare tutta l'infrastruttura necessaria. Non scegliere tutte le architetture senza spiegare le decisioni ad Andrea. Responsabilità ingegneristica: controllare evidenza e limiti del sistema, anche se il codice è generato.

## Stato effettivo

- Appunti1/Appunti2 studiati, con rettifiche segnalate nel turno precedente; dubbio U02 ancora aperto.
- Esercizio U01 presente; non controllato o valutato in questa revisione.
- Nessuna run embedding/retrieval/RAG nuova eseguita in questa revisione. Versioni e dipendenze da verificare prima del pilot.
- 24 GB RAM, Iris Xe, CPU, zero spese aggiuntive. FiQA come benchmark; mini-dossier simulato distinto se necessario per versioni/eccezioni.
- Repository AI e mockup indipendenti già collegate. Mockup aggiornato su richiesta alla V3: sei aree di competenza, stack separato, fine-tuning pratico eventuale. Anteprima 4174 riavviata senza cache; sito originale invariato. Ripresa didattica D1: non proseguire il redesign senza richiesta.
- Una scheda per decisione con configurazione, evidenza e limite; risultati generati automaticamente e pochi casi letti insieme.
- Fine-tuning: criteri spiegati già durante la scelta encoder; training solo se giustificato e sostenibile, non requisito.

La versione precedente del piano è in docs/archivio. La nuova sequenza è D1–D6; non tornare a U01 per la sua mancata conferma.

## Chiusura

Aggiornare qui decisione corrente, ciò che Andrea sa spiegare e prossimo confronto; una riga nel DIARIO e scheda/nota coinvolta. Nessun nuovo handover parallelo. Prompt di ripresa in [INIZIA_CON_SOL](INIZIA_CON_SOL.md).
