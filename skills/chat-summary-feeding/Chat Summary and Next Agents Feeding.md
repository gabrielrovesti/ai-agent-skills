```markdown
# Skill: Context Compaction / Agent Handoff

## Obiettivo

Creare un unico file Markdown che **compatti l’intero contesto della conversazione** in forma strutturata, tecnica e riusabile.

Il file deve permettere a qualsiasi altro agente (o alla stessa istanza in una nuova sessione) di riprendere il lavoro **senza rileggere la chat originale** e senza perdita di informazioni rilevanti.

Non è un riassunto narrativo: è una **compressione operativa del contesto**.

---

## Trigger di Attivazione

Attivare questa skill quando:

- La conversazione diventa lunga o dispersiva
- Sono stati eseguiti più step tecnici (codice, debug, comandi)
- Sono state prese decisioni rilevanti
- Il contesto rischia di degradare (limiti finestra)
- Il lavoro deve essere ripreso successivamente
- Serve interoperabilità tra agenti diversi

---

## Output

Creare o aggiornare un file unico:

```

CHAT_HANDOFF.md

```

Possibili posizionamenti:

```

/docs/CHAT_HANDOFF.md

```

oppure:

```

/.agent/CHAT_HANDOFF.md

`````

Non creare duplicati. Aggiornare sempre lo stesso file.

---

## Struttura Obbligatoria

````markdown
# Chat Handoff

## 1. Obiettivo

Descrizione precisa del problema iniziale e del risultato atteso.

---

## 2. Stato Attuale

- Cosa è stato completato
- Cosa è ancora in sospeso

---

## 3. Decisioni Chiave

Decisioni tecniche prese, con motivazioni.

Includere alternative scartate se impattano il seguito.

---

## 4. File Coinvolti

Elenco file rilevanti con stato attuale.

Esempio:

- `src/.../Service.java`
  - Logica aggiornata
  - Mancano test

---

## 5. Comandi Eseguiti

Comandi rilevanti e risultati.

```bash
./gradlew build
`````

Risultato:

```text
Errore su test X
```

---

## 6. Errori / Problemi

* Stack trace
* Bug riscontrati
* Tentativi falliti
* Blocchi attuali

---

## 7. Contesto Critico

Informazioni indispensabili per la continuità:

* Regole di business
* Convenzioni tecniche
* API e contratti
* Branch Git
* Dipendenze
* Assunzioni operative

Non includere segreti o dati sensibili.

---

## 8. Prossimi Step

Lista numerata e concreta:

1. Azione precisa
2. Azione precisa
3. Azione precisa

---