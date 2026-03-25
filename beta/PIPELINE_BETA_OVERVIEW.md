# PurifyFactory v9.1.6 — Pipeline Overview (BETA)

---

## Flusso della Pipeline

```
┌─────────────────────────────────────────────────────────────────────┐
│                         INPUT                                        │
│   file.jsonl  (campo "sentence" o "text", uno per riga)             │
└──────────────────────────┬──────────────────────────────────────────┘
                           │
                           ▼
┌──────────────────────────────────────────────────────────────────────┐
│  1. SPLIT                                                            │
│     purifyfactory split --config config.json                        │
│                                                                      │
│  • Legge il JSONL di input                                           │
│  • Divide in batch da N record (default BETA: 50)                   │
│  • Scrive i batch in  data/queue/  come file JSON                   │
│  • Genera  data/state/split.state                                   │
└──────────────────────────┬───────────────────────────────────────────┘
                           │   data/queue/batch_0001.json ...
                           ▼
┌──────────────────────────────────────────────────────────────────────┐
│  2. ORCHESTRATE                                                      │
│     purifyfactory orchestrate --config config.json                  │
│                                                                      │
│  • Legge i batch in queue/                                           │
│  • Assegna stato "queued" a ciascuno                                │
│  • Prepara la coda di lavoro per il processo                        │
│  • Genera  data/state/orchestrate.state                             │
└──────────────────────────┬───────────────────────────────────────────┘
                           │   batch pronti all'elaborazione
                           ▼
┌──────────────────────────────────────────────────────────────────────┐
│  3. PROCESS                                                          │
│     purifyfactory process --config config.json                      │
│                                                                      │
│  • Lancia N worker in parallelo (default BETA: 4)                   │
│  • Ogni worker prende un batch dalla coda                           │
│  • Chiama il provider AI (Anthropic / OpenAI / Gemini / Local)      │
│  • Pulisce i testi con il prompt configurato                        │
│  • Scrive output in  data/output/                                   │
│  • Aggiorna stato batch: queued → processing → completed / error    │
└───────────┬───────────────────────────┬─────────────────────────────┘
            │ batch completati          │ batch in errore
            ▼                           ▼
   data/output/*.jsonl          data/errors/*.json
                                        │
                           ┌────────────┘
                           ▼
┌──────────────────────────────────────────────────────────────────────┐
│  4. SUPERVISE  ★ solo BETA / ENTERPRISE                              │
│     purifyfactory supervise --config config.json                    │
│                                                                      │
│  • Daemon continuo (non termina da solo)                            │
│  • Monitora i batch in errore ogni N secondi                        │
│  • Re-inserisce automaticamente in coda i batch falliti             │
│  • Rilancia process se necessario                                   │
│  • Arresto pulito con Ctrl+C / SIGTERM                              │
└──────────────────────────────────────────────────────────────────────┘

      OPPURE (senza supervise):

┌──────────────────────────────────────────────────────────────────────┐
│  RECOVER  (manuale, tutti i tier)                                    │
│     purifyfactory recover                                            │
│                                                                      │
│  • Rimette in coda tutti i batch in errore in una sola volta        │
│  • Da eseguire manualmente prima di un nuovo process                │
└──────────────────────────────────────────────────────────────────────┘
```

---

## Struttura dati su disco

```
data/
├── queue/              ← batch JSON in attesa di elaborazione
│   ├── batch_0001.json
│   └── batch_0002.json
├── output/             ← risultati puliti (JSONL)
│   └── output_0001.jsonl
├── errors/             ← batch falliti con traccia errore
│   └── batch_0003_err.json
├── state/              ← file di stato per ogni fase
│   ├── split.state
│   ├── orchestrate.state
│   └── process.state
└── usage/
    └── beta_usage.json ← contatore token/record (cap BETA)
```

---

## Provider AI supportati

| Provider   | Modelli esempio                        | Note                        |
|------------|----------------------------------------|-----------------------------|
| Anthropic  | claude-sonnet-4-6, claude-haiku-4-5   | Default consigliato         |
| OpenAI     | gpt-4o, gpt-4o-mini                   |                             |
| Gemini     | gemini-1.5-flash, gemini-1.5-pro      |                             |
| Local      | Ollama, vLLM (qualsiasi modello)      | Nessun costo API, on-prem   |

---

## Feature BETA vs PRO vs ENTERPRISE

| Feature                        | BETA | PRO  | ENTERPRISE |
|--------------------------------|------|------|------------|
| Split / Orchestrate / Process  | ✅   | ✅   | ✅          |
| Recover (manuale)              | ✅   | ✅   | ✅          |
| Supervise (daemon auto)        | ✅   | ❌   | ✅          |
| TUI interattiva                | ✅   | ✅   | ✅          |
| Provider Local (Ollama/vLLM)   | ✅   | ✅   | ✅          |
| Worker paralleli               | 4    | 4    | illimitati |
| Batch size max                 | 50   | 50   | 1.000      |
| Cap token/record (beta)        | ✅   | —    | —          |
| Validazione licenza online     | ✅   | ✅   | ✅          |

---

## Strumenti di Monitoraggio

### CLI — comandi disponibili

```
purifyfactory status          Riepilogo contatori (completed/queue/errors/processing)
purifyfactory watch           Monitoraggio live aggiornato ogni 5s (Ctrl+C per uscire)
purifyfactory report          Totali: record, token, costo USD, success rate
purifyfactory statistics      Statistiche per record: token medi, costo medio, uso BETA
purifyfactory errors          Lista batch in errore con dettaglio
purifyfactory queue list      Lista batch in coda
purifyfactory validate        Verifica licenza: tier, holder, scadenza
purifyfactory info            Info sistema e licenza
purifyfactory hardware-id     Hardware fingerprint della macchina
purifyfactory export-debug    Esporta ZIP con log, stato, config (senza API key)
```

### TUI — interfaccia interattiva

```
purifyfactory tui --config config.json
```

```
┌─ PurifyFactory v9.1.6 ─────────────── [BETA] ──────────────────────┐
│ [1]Dashboard  [2]Pipeline  [3]Monitor  [4]Strumenti  [r]Aggiorna   │
├─────────────────┬───────────────────────────────────────────────────┤
│ NAVIGAZIONE     │                                                    │
│ > Dashboard [1] │  ← contenuto schermata attiva                     │
│   Pipeline  [2] │                                                    │
│   Monitor   [3] │                                                    │
│   Strumenti [4] │                                                    │
│                 │                                                    │
│ STATO           │                                                    │
│ OK   142        │                                                    │
│ Coda  18        │                                                    │
│ Err    3        │                                                    │
└─────────────────┴───────────────────────────────────────────────────┘
```

| Schermata      | Contenuto                                                     |
|----------------|---------------------------------------------------------------|
| Dashboard      | Stato pipeline, info licenza (tier/holder/scadenza), totali  |
| Pipeline       | Avvio split/orchestrate/process/supervise, log in tempo reale|
| Monitor        | Barra progresso, tabella batch per stato, costi, uso BETA    |
| Strumenti      | Errori, coda, diagnostica, export debug, hardware ID         |

**Tasti:** `1-4` naviga, `r` aggiorna manuale, `q` esci

---

## Ciclo completo tipico (BETA)

```bash
# 1. Configura
cp docs/config_example.json config.json
nano config.json   # imposta provider, API key, input_file

# 2. Pipeline
purifyfactory split      --config config.json
purifyfactory orchestrate --config config.json
purifyfactory process    --config config.json

# 3. Verifica
purifyfactory status
purifyfactory report

# 4. Se ci sono errori
purifyfactory errors          # vedi cosa è fallito
purifyfactory recover         # rimetti in coda
purifyfactory process --config config.json   # riprova

# 5. Oppure: supervisione automatica
purifyfactory supervise --config config.json  # ★ BETA only

# 6. Oppure: tutto dalla TUI
purifyfactory tui --config config.json
```
