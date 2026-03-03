# PurifyFactory v9.1.6 — Beta Program

**Industrial AI-powered text cleaning pipeline** — Mentora Technologies

---

🇮🇹 [Italiano](#italiano) · 🇬🇧 [English](#english)

---

<a name="italiano"></a>
# 🇮🇹 Italiano

## Benvenuto nel programma beta

Sei stato selezionato come betatester di PurifyFactory v9.1.6. Questo repository contiene tutto il necessario per installare il software, eseguire i test e inviarci il feedback.

Il tuo contributo è essenziale: i tuoi dataset reali e le tue osservazioni ci permettono di certificare il software per l'uso in produzione (TRL5) e di migliorarlo prima del rilascio commerciale. Grazie.

---

## ⚠️ Attenzione: API key richiesta — a tue spese

> **PurifyFactory utilizza API di intelligenza artificiale esterne per elaborare il testo.**
> **Queste API non sono incluse nel software e non sono fornite da Mentora Technologies.**
>
> Prima di usare il software devi disporre di una API key attiva e con crediti sufficienti di uno dei provider supportati:
>
> | Provider | Dove ottenerla | Costo indicativo |
> |---|---|---|
> | **OpenAI** | platform.openai.com | ~$0.15–$0.60 per milione di token (gpt-4o-mini) |
> | **Anthropic** | console.anthropic.com | ~$0.25–$1.00 per milione di token (claude-haiku) |
> | **Google Gemini** | aistudio.google.com | ~$0.075–$0.30 per milione di token (gemini-flash) |
> | **Locale (Ollama/vLLM)** | — | Costo di infrastruttura proprio |
>
> **I costi di elaborazione sono interamente a tuo carico.** Per un dataset di test da 1.000 record il costo tipico con gpt-4o-mini è inferiore a $0.10.
>
> Monitora i tuoi consumi direttamente sulla dashboard del tuo provider API.

---

## Cos'è PurifyFactory

PurifyFactory è uno strumento on-premise per la **normalizzazione e pulizia di grandi volumi di testo** tramite modelli di linguaggio AI. È progettato per automatizzare operazioni che richiederebbero ore di lavoro manuale:

- Rimozione di spazi multipli, caratteri anomali, punteggiatura malformata
- Eliminazione di parole ripetute in sequenza
- Normalizzazione di maiuscole/minuscole e apostrofi
- Correzione di errori tipografici sistematici
- Qualsiasi altra operazione di pulizia descrivibile in un prompt di sistema

Il software lavora su file in formato JSONL e produce un file di output con il testo originale e il testo pulito affiancati, campo per campo. Funziona in locale sul tuo computer: i dati non transitano su server di Mentora Technologies.

**Casi d'uso tipici:**
- Normalizzazione di dataset per l'addestramento di modelli ML
- Pulizia di feed di dati da CRM, ERP, e-commerce
- Preparazione di testi per pipeline RAG o full-text search
- Standardizzazione di dataset di testo aziendale prima dell'analisi

---

## Requisiti

- **Sistema operativo**: Linux x86_64 (Ubuntu 20.04+, Debian 11+, qualsiasi distro moderna)
- **File `license.json`**: ricevuto via email da Mentora Technologies — specifico per la tua macchina
- **API key**: OpenAI, Anthropic, Google Gemini oppure modello locale (Ollama/vLLM)
- **Spazio disco**: ~200 MB per il software + spazio per i tuoi dataset
- **RAM**: almeno 2 GB disponibili durante l'elaborazione

> Windows e macOS sono in roadmap per versioni successive.

---

## Installazione

### 1 — Scarica il pacchetto dalla sezione Releases

Vai alla sezione [**Releases**](../../releases) di questo repository e scarica:
- `purifyfactory-v9.1.6-beta.1-linux-x64.tar.gz` — il software (65 MB)
- `purifyfactory-v9.1.6-beta.1-linux-x64.tar.gz.sha256` — checksum di verifica

**Verifica l'integrità prima di estrarre** (consigliato):
```bash
sha256sum -c purifyfactory-v9.1.6-beta.1-linux-x64.tar.gz.sha256
# Deve rispondere: purifyfactory-v9.1.6-beta.1-linux-x64.tar.gz: OK
```

### 2 — Estrai il pacchetto e genera il tuo fingerprint hardware

```bash
tar xzf purifyfactory-v9.1.6-beta.1-linux-x64.tar.gz
cd purifyfactory-v9.1.6-beta.1-linux-x64
./purifyfactory hardware-id
```

Output atteso:
```
  HARDWARE FINGERPRINT:
  5a1d49815666cc3a82910ae4a42e8214fc7798d4d9d549cbf95f2de3c682ee35

  Send this fingerprint to Mentora Technologies
  to receive your license file.
```

**Invia questo fingerprint a support@mentoratechnologies.com** — riceverai la tua licenza personale entro 24 ore.

### 3 — Installa la licenza ricevuta

```bash
mkdir -p ~/.config/purifyfactory
cp /percorso/del/license.json ~/.config/purifyfactory/license.json
./purifyfactory validate
# Risposta attesa: ✅ License VALID — tier=beta
```

### 4 — Configura la tua API key

```bash
cp beta/examples/config_quickstart.json mio_config.json
```

Apri `mio_config.json` e inserisci la tua API key:

```json
"ai": {
  "provider": "openai",
  "model": "gpt-4o-mini",
  "api_keys": {
    "openai": "sk-..."
  }
}
```

| Provider | Valore `provider` | Modelli consigliati |
|---|---|---|
| OpenAI | `"openai"` | `"gpt-4o-mini"`, `"gpt-4o"` |
| Anthropic | `"anthropic"` | `"claude-haiku-4-5-20251001"` |
| Google Gemini | `"gemini"` | `"gemini-2.0-flash"` |
| Locale (Ollama) | `"local"` | nome modello Ollama (es. `"llama3.2"`) |

---

## Elaborare un dataset

Il dataset deve essere un file **JSONL** (una riga JSON per record).

```bash
# Step 1 — Suddividi il dataset in blocchi
./purifyfactory split --input /percorso/dataset.jsonl --config mio_config.json

# Step 2 — Organizza i batch di lavoro
./purifyfactory orchestrate --config mio_config.json

# Step 3 — Elabora con l'AI
./purifyfactory process --config mio_config.json
```

Il risultato si trova in `~/.config/purifyfactory/data/output/final_output.jsonl`:
```json
{"original_text": "testo originale...", "cleaned_text": "testo pulito...", "provider": "openai", "tokens": 45, "cost": 0.000010}
```

---

## Monitoraggio

| Comando | Cosa mostra |
|---|---|
| `./purifyfactory status` | Riepilogo: batch in coda, completati, errori |
| `./purifyfactory watch` | Aggiornamento automatico ogni 3 secondi (Ctrl+C per uscire) |
| `./purifyfactory queue` | Lista dei batch ancora in coda |
| `./purifyfactory errors` | Batch falliti con il motivo dell'errore |
| `./purifyfactory report` | Record elaborati, token usati, costo totale |
| `./purifyfactory statistics` | Costo medio per record, utilizzo disco |

---

## Se qualcosa va storto

**Batch falliti:**
```bash
./purifyfactory errors
./purifyfactory recover
./purifyfactory process --config mio_config.json
```

**⚠️ Ricominciare da zero con un nuovo dataset** — pulisci lo stato prima di `split`:
```bash
rm -rf ~/.config/purifyfactory/data/state/queue/*
rm -rf ~/.config/purifyfactory/data/state/errors/*
rm -rf ~/.config/purifyfactory/data/state/progress/*
rm -rf ~/.config/purifyfactory/data/splits/*
rm -f  ~/.config/purifyfactory/data/output/final_output.jsonl
```

> Non necessario se il job precedente si è completato con successo.

**Problemi comuni:**

| Sintomo | Soluzione |
|---|---|
| `License validation failed` | Verifica `~/.config/purifyfactory/license.json` |
| `Provider authentication failed` | Controlla la API key e il saldo sul portale del provider |
| Batch bloccati in errore | Esegui `recover` poi `process` |

**Log per il supporto:**
```bash
./purifyfactory export-debug --output debug_report.zip
# Invia il file a support@mentoratechnologies.com
```

---

## Dimensione del dataset per la certificazione

| Dimensione | Valido per certificazione TRL5? |
|---|---|
| < 100 record | ❌ Solo per apprendimento |
| 100 – 999 record | ⚠️ Insufficiente |
| 1.000 – 4.999 record | ✅ Minimo accettato |
| ≥ 5.000 record | ✅ Consigliato |
| ≥ 10.000 record | ✅ Ottimale |

---

## Feedback

Compila `FEEDBACK_TEMPLATE.md` e invialo a **feedback@mentoratechnologies.com** oppure apri una Issue in questo repository.

---

<a name="english"></a>
# 🇬🇧 English

## Welcome to the Beta Program

You have been selected as a beta tester for PurifyFactory v9.1.6. This repository contains everything you need to install the software, run tests, and send us feedback.

Your contribution is essential: your real-world datasets and observations allow us to certify the software for production use (TRL5) and improve it before commercial release. Thank you.

---

## ⚠️ Important: API key required — at your own expense

> **PurifyFactory uses external AI APIs to process text.**
> **These APIs are not included in the software and are not provided by Mentora Technologies.**
>
> Before using the software you must have an active API key with sufficient credits from one of the supported providers:
>
> | Provider | Where to get it | Indicative cost |
> |---|---|---|
> | **OpenAI** | platform.openai.com | ~$0.15–$0.60 per million tokens (gpt-4o-mini) |
> | **Anthropic** | console.anthropic.com | ~$0.25–$1.00 per million tokens (claude-haiku) |
> | **Google Gemini** | aistudio.google.com | ~$0.075–$0.30 per million tokens (gemini-flash) |
> | **Local (Ollama/vLLM)** | — | Your own infrastructure cost |
>
> **All processing costs are entirely your responsibility.** For a 1,000-record test dataset, the typical cost with gpt-4o-mini is under $0.10.
>
> Monitor your usage directly on your API provider's dashboard.

---

## What is PurifyFactory

PurifyFactory is an on-premise tool for **normalizing and cleaning large volumes of text** using AI language models. It automates operations that would otherwise take hours of manual work:

- Removing multiple spaces, anomalous characters, malformed punctuation
- Eliminating consecutive repeated words
- Normalizing capitalization and apostrophes
- Correcting systematic typographical errors
- Any other cleaning operation that can be described in a system prompt

The software works with JSONL files and produces an output file with the original and cleaned text side by side, field by field. It runs locally on your machine: your data never passes through Mentora Technologies servers.

**Typical use cases:**
- Normalizing datasets for ML model training
- Cleaning data feeds from CRM, ERP, e-commerce systems
- Preparing text for RAG pipelines or full-text search
- Standardizing corporate text datasets before analysis

---

## Requirements

- **Operating system**: Linux x86_64 (Ubuntu 20.04+, Debian 11+, any modern distro)
- **`license.json` file**: received via email from Mentora Technologies — specific to your machine
- **API key**: OpenAI, Anthropic, Google Gemini, or local model (Ollama/vLLM)
- **Disk space**: ~200 MB for the software + space for your datasets
- **RAM**: at least 2 GB available during processing

> Windows and macOS are on the roadmap for future versions.

---

## Installation

### 1 — Download the package from the Releases section

Go to the [**Releases**](../../releases) section of this repository and download:
- `purifyfactory-v9.1.6-beta.1-linux-x64.tar.gz` — the software (65 MB)
- `purifyfactory-v9.1.6-beta.1-linux-x64.tar.gz.sha256` — verification checksum

**Verify integrity before extracting** (recommended):
```bash
sha256sum -c purifyfactory-v9.1.6-beta.1-linux-x64.tar.gz.sha256
# Expected: purifyfactory-v9.1.6-beta.1-linux-x64.tar.gz: OK
```

### 2 — Extract the package and generate your hardware fingerprint

```bash
tar xzf purifyfactory-v9.1.6-beta.1-linux-x64.tar.gz
cd purifyfactory-v9.1.6-beta.1-linux-x64
./purifyfactory hardware-id
```

Expected output:
```
  HARDWARE FINGERPRINT:
  5a1d49815666cc3a82910ae4a42e8214fc7798d4d9d549cbf95f2de3c682ee35

  Send this fingerprint to Mentora Technologies
  to receive your license file.
```

**Send this fingerprint to support@mentoratechnologies.com** — you will receive your personal license within 24 hours.

### 3 — Install the received license

```bash
mkdir -p ~/.config/purifyfactory
cp /path/to/license.json ~/.config/purifyfactory/license.json
./purifyfactory validate
# Expected: ✅ License VALID — tier=beta
```

### 4 — Configure your API key

```bash
cp beta/examples/config_quickstart.json my_config.json
```

Open `my_config.json` and insert your API key:

```json
"ai": {
  "provider": "openai",
  "model": "gpt-4o-mini",
  "api_keys": {
    "openai": "sk-..."
  }
}
```

| Provider | `provider` value | Recommended models |
|---|---|---|
| OpenAI | `"openai"` | `"gpt-4o-mini"`, `"gpt-4o"` |
| Anthropic | `"anthropic"` | `"claude-haiku-4-5-20251001"` |
| Google Gemini | `"gemini"` | `"gemini-2.0-flash"` |
| Local (Ollama) | `"local"` | Ollama model name (e.g. `"llama3.2"`) |

---

## Processing a dataset

Your dataset must be a **JSONL** file (one JSON object per line).

```bash
# Step 1 — Split the dataset into chunks
./purifyfactory split --input /path/to/dataset.jsonl --config my_config.json

# Step 2 — Organize work batches
./purifyfactory orchestrate --config my_config.json

# Step 3 — Process with AI
./purifyfactory process --config my_config.json
```

The result is saved to `~/.config/purifyfactory/data/output/final_output.jsonl`:
```json
{"original_text": "original text...", "cleaned_text": "cleaned text...", "provider": "openai", "tokens": 45, "cost": 0.000010}
```

---

## Monitoring

| Command | What it shows |
|---|---|
| `./purifyfactory status` | Summary: batches queued, completed, errors |
| `./purifyfactory watch` | Auto-refresh every 3 seconds (Ctrl+C to exit) |
| `./purifyfactory queue` | Detailed list of batches still in queue |
| `./purifyfactory errors` | Failed batches with error reason |
| `./purifyfactory report` | Records processed, tokens used, total cost |
| `./purifyfactory statistics` | Average cost per record, disk usage |

---

## Troubleshooting

**Failed batches:**
```bash
./purifyfactory errors
./purifyfactory recover
./purifyfactory process --config my_config.json
```

**⚠️ Starting fresh with a new dataset** — clean the state before `split`:
```bash
rm -rf ~/.config/purifyfactory/data/state/queue/*
rm -rf ~/.config/purifyfactory/data/state/errors/*
rm -rf ~/.config/purifyfactory/data/state/progress/*
rm -rf ~/.config/purifyfactory/data/splits/*
rm -f  ~/.config/purifyfactory/data/output/final_output.jsonl
```

> Not needed if the previous job completed successfully.

**Common issues:**

| Symptom | Solution |
|---|---|
| `License validation failed` | Check `~/.config/purifyfactory/license.json` |
| `Provider authentication failed` | Check your API key and credit balance on the provider portal |
| Batches stuck in errors | Run `recover` then `process` |

**Logs for support:**
```bash
./purifyfactory export-debug --output debug_report.zip
# Send the file to support@mentoratechnologies.com
```

---

## Dataset size for certification

| Size | Valid for TRL5 certification? |
|---|---|
| < 100 records | ❌ Learning only |
| 100 – 999 records | ⚠️ Insufficient |
| 1,000 – 4,999 records | ✅ Minimum accepted |
| ≥ 5,000 records | ✅ Recommended |
| ≥ 10,000 records | ✅ Optimal |

---

## Feedback

Fill in `FEEDBACK_TEMPLATE.md` and send it to **feedback@mentoratechnologies.com** or open an Issue in this repository.

---

**Mentora Technologies** · support@mentoratechnologies.com · [mentoratechnologies.com](https://mentoratechnologies.com)
