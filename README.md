# PurifyFactory v9.1.6 — Beta Program

**Il tuo dataset. Ultra Pulito. Pronto per addestrare AI.** — Mentora Technologies

---

🇮🇹 [Italiano](#italiano) · 🇬🇧 [English](#english)

---

<a name="italiano"></a>
# 🇮🇹 Italiano

> **⚠️ Questo non è un progetto open source.**
> Il codice sorgente non è disponibile. Questo repository è il punto di accesso al **programma beta** di PurifyFactory: puoi scaricare il binario compilato, testarlo con i tuoi dati e inviarci feedback.
> Per partecipare alla beta è necessaria una **licenza personale** (gratuita durante la fase beta) da richiedere a [waitlist@purifyfactory.com](mailto:waitlist@purifyfactory.com).


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

## Cos'è PurifyFactory — e perché è fondamentale per l'AI

### Il problema: i dati sporchi distruggono i modelli AI

Ogni modello di linguaggio AI — che si tratti di un LLM, un sistema RAG, un classificatore o un motore di ricerca semantica — è esattamente buono quanto i dati su cui è stato addestrato. È il principio fondamentale del settore: **garbage in, garbage out**.

Il problema è che i dati reali sono quasi sempre sporchi. Testi estratti da CRM, ERP, siti web, documenti scansionati, feed automatici: contengono errori sistematici che sembrano innocui ma che, moltiplicati per milioni di record, degradano in modo misurabile le prestazioni del modello addestrato.

Spazi doppi, punteggiatura malformata, parole ripetute, apostrofi sbagliati, maiuscole inconsistenti, artefatti di encoding — ciascuno di questi errori introduce rumore nel dataset. Il modello impara il rumore insieme al segnale. Il risultato è un modello meno preciso, meno coerente, più difficile da controllare.

La pulizia manuale non è una soluzione: è impossibile da garantire su migliaia o milioni di record, è costosa, è lenta, ed è soggetta a sua volta a errori umani.

### La soluzione: dataset Ultra Puliti

**PurifyFactory è uno strumento on-premise che trasforma qualsiasi dataset testuale in un dataset Ultra Pulito** — ovvero privo di qualsiasi errore di forma, encoding, formattazione e coerenza — indipendentemente dalla dimensione del dataset.

Un dataset Ultra Pulito non significa solo "testo più leggibile". Significa:

- **Zero errori di formattazione**: nessuno spazio doppio, nessuna punteggiatura malformata, nessun carattere anomalo
- **Zero ripetizioni**: nessuna parola o frase ripetuta in sequenza
- **Coerenza totale**: maiuscole/minuscole, apostrofi e punteggiatura normalizzati secondo regole uniformi in ogni record
- **Zero artefatti**: niente residui di markup, encoding errati, tag HTML, caratteri di controllo
- **Qualità uniforme**: ogni singolo record del dataset rispetta lo stesso standard, dal primo all'ultimo

Questo standard di qualità è definito da te tramite il **prompt di sistema** — descrivi in linguaggio naturale le regole di pulizia che vuoi applicare, e PurifyFactory le applica in modo coerente e verificabile su ogni record del dataset.

### Perché questo cambia tutto per l'addestramento AI

Il mercato dell'AI è oggi il mercato tecnologico a più alta crescita al mondo. Migliaia di aziende stanno costruendo modelli proprietari, fine-tuning di LLM, sistemi RAG su dati aziendali. Tutte queste applicazioni dipendono da un fattore critico che viene spesso sottovalutato: **la qualità del dato di addestramento**.

La ricerca empirica è inequivocabile: un modello addestrato su dati puliti con metà dei record supera sistematicamente un modello addestrato su dati sporchi con il doppio dei record. La qualità batte la quantità.

PurifyFactory automatizza questa fase critica del pipeline ML:

- **Scalabile**: funziona su dataset di qualsiasi dimensione, da centinaia a milioni di record, con la stessa qualità garantita
- **Riproducibile**: lo stesso prompt produce gli stessi risultati su qualsiasi dataset — niente variabilità umana
- **Verificabile**: ogni record di output affianca il testo originale e il testo pulito, permettendo audit completi
- **On-premise**: i dati non lasciano mai la tua infrastruttura — fondamentale per dataset aziendali sensibili
- **Provider-agnostic**: funziona con OpenAI, Anthropic, Google Gemini o modelli locali (Ollama/vLLM)

**Ogni ora investita in PurifyFactory prima dell'addestramento vale decine di ore risparmiate in post-training, fine-tuning correttivo e gestione degli errori del modello.**

---

## Tier di partecipazione

Il programma beta prevede due livelli di partecipazione. Scegli quello adatto alla dimensione del tuo dataset.

---

### Tier 1 — Contributor

**Requisiti dataset:** 500–4.999 record

**Cosa ti chiediamo:**
- Elaborare il dataset con almeno 1 provider AI (online o locale)
- Compilare il FEEDBACK_TEMPLATE.md in modo completo
- Inviare i Log anonimizzati (`export-debug`) a betatesting@purifyfactory.com

**Cosa ricevi:**
- Accesso anticipato al SaaS PurifyFactory (prima del lancio pubblico)
- Licenza gratuita per 6 mesi dalla data di rilascio commerciale
- Rimborso forfettario di €5 (vedi condizioni sotto)

**Nota:** Il Tier 1 non contribuisce alla certificazione TRL5, ma è essenziale per il
feedback qualitativo, la ricerca di bug e la validazione dell'esperienza utente.

---

### Tier 2 — Certificatore

**Requisiti dataset:** ≥ 5.000 record (consigliati ≥ 10.000 per validazione completa)

**Cosa ti chiediamo:**
- Elaborare il dataset con **almeno 2 provider AI diversi** (es. OpenAI + Anthropic,
  oppure online + locale)
- Compilare il FEEDBACK_TEMPLATE.md in modo completo e dettagliato
- Inviare i Log anonimizzati (`export-debug`) a betatesting@purifyfactory.com
- Allegare un campione di almeno 20 coppie input/output rappresentative del dataset,
  prive di dati personali o confidenziali

**Cosa ricevi:**
- Accesso anticipato al SaaS PurifyFactory (prima del lancio pubblico)
- Licenza gratuita lifetime, senza diritto a futuri aggiornamenti automatici,
  con supporto limitato esclusivamente alla segnalazione e correzione di bug critici
- Rimborso forfettario di €10 (vedi condizioni sotto)

**Nota:** Il Tier 2 contribuisce direttamente alla certificazione TRL5 necessaria
per il rilascio del SaaS commerciale. Il tuo contributo è fondamentale.

---

### Condizioni di rimborso

Il rimborso è:
- **A discrezione esclusiva di Mentora Technologies**, che si riserva il diritto di
  non erogarlo in caso di feedback incompleto, non valido o non conforme ai requisiti
  del tier di appartenenza
- Erogato **entro 30 giorni** dalla validazione positiva del feedback da parte di
  Mentora Technologies
- Effettuato **esclusivamente tramite PayPal o bonifico bancario immediato** (SEPA)
- Non cumulabile con altri sconti, rimborsi o compensi
- Non trasferibile a terzi

Per ricevere il rimborso, il Betatester deve:
1. Completare il testing secondo i requisiti del proprio tier
2. Inviare il FEEDBACK_TEMPLATE.md compilato a betatesting@purifyfactory.com
3. Attendere la validazione da parte di Mentora Technologies
4. Comunicare i propri dati PayPal o IBAN su richiesta esplicita di Mentora Technologies

Mentora Technologies non è tenuta a giustificare la mancata erogazione del rimborso
in caso di feedback ritenuto non valido.

---

### Come candidarsi

Invia una email a **waitlist@purifyfactory.com** con:
- Oggetto: `[BETA] Candidatura — [Tier 1 / Tier 2] — [Tuo Cognome]`
- Dimensione approssimativa del tuo dataset
- Provider AI che intendi utilizzare
- Sistema operativo (Linux / Windows / macOS)
- Breve descrizione del tuo caso d'uso (1-3 righe)

Riceverai risposta entro 48 ore con le istruzioni per la firma dell'NDA e il rilascio
della licenza. Una volta accettato nel programma, utilizzerai **betatesting@purifyfactory.com**
per tutte le comunicazioni successive.

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
- `purifyfactory-v9.1.6-beta.3-linux-x64.tar.gz` — il software (~150 MB)
- `purifyfactory-v9.1.6-beta.3-linux-x64.tar.gz.sha256` — checksum di verifica

**Verifica l'integrità prima di estrarre** (consigliato):
```bash
sha256sum -c purifyfactory-v9.1.6-beta.3-linux-x64.tar.gz.sha256
# Deve rispondere: purifyfactory-v9.1.6-beta.3-linux-x64.tar.gz: OK
```

### 2 — Estrai il pacchetto e genera il tuo fingerprint hardware

```bash
tar xzf purifyfactory-v9.1.6-beta.3-linux-x64.tar.gz
cd purifyfactory-v9.1.6-beta.3-linux-x64
./purifyfactory hardware-id
```

Output atteso:
```
  HARDWARE FINGERPRINT:
  5a1d49815666cc3a82910ae4a42e8214fc7798d4d9d549cbf95f2de3c682ee35

  Send this fingerprint to Mentora Technologies
  to receive your license file.
```

**Invia questo fingerprint a [betatesting@purifyfactory.com](mailto:betatesting@purifyfactory.com)** — riceverai la tua licenza personale entro 24 ore.

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

Il dataset deve essere un file **JSONL** (una riga JSON per record, codifica UTF-8).

**Campo testo obbligatorio**: ogni record deve contenere un campo `"sentence"` (cercato per primo) oppure `"text"` (fallback). I record privi di entrambi vengono saltati con un warning nel log. Gli altri campi del record vengono ignorati e non trasferiti nell'output.

```jsonl
{"sentence": "Testo da pulire.", "id": 1}
{"text": "Altro record.", "id": 2}
```

**Se il tuo dataset usa un campo con nome diverso** (es. `"content"`, `"body"`, `"description"`), rinominalo prima di avviare la pipeline:

```python
import json
with open("input.jsonl") as fin, open("prepared.jsonl", "w") as fout:
    for line in fin:
        record = json.loads(line)
        record["sentence"] = record.pop("content")  # adatta al tuo campo
        fout.write(json.dumps(record, ensure_ascii=False) + "\n")
```

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
# Invia il file a betatesting@purifyfactory.com
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

Compila `FEEDBACK_TEMPLATE.md` e invialo a **[betatesting@purifyfactory.com](mailto:betatesting@purifyfactory.com)** oppure apri una Issue in questo repository.

---

<a name="english"></a>
# 🇬🇧 English

## Welcome to the Beta Program

> **⚠️ This is not an open source project.**
> The source code is not available. This repository is the access point to the **PurifyFactory beta program**: you can download the compiled binary, test it on your data, and send us feedback.
> To participate in the beta, you need a **personal license** (free during the beta phase) — request one at [waitlist@purifyfactory.com](mailto:waitlist@purifyfactory.com).

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

## What is PurifyFactory — and why it matters for AI

### The problem: dirty data destroys AI models

Every AI language model — whether an LLM, a RAG system, a classifier, or a semantic search engine — is exactly as good as the data it was trained on. This is the field's foundational principle: **garbage in, garbage out**.

The problem is that real-world data is almost always dirty. Text extracted from CRMs, ERPs, websites, scanned documents, automated feeds: it contains systematic errors that seem harmless individually but, multiplied across millions of records, measurably degrade the performance of the trained model.

Double spaces, malformed punctuation, repeated words, misplaced apostrophes, inconsistent capitalization, encoding artifacts — each of these errors introduces noise into the dataset. The model learns the noise alongside the signal. The result is a less accurate, less consistent, harder-to-control model.

Manual cleaning is not a solution: it cannot be guaranteed at scale across thousands or millions of records, it is expensive, slow, and itself prone to human error.

### The solution: Ultra Clean datasets

**PurifyFactory is an on-premise tool that transforms any text dataset into an Ultra Clean dataset** — free from any error of form, encoding, formatting, or consistency — regardless of dataset size.

An Ultra Clean dataset does not simply mean "more readable text." It means:

- **Zero formatting errors**: no double spaces, no malformed punctuation, no anomalous characters
- **Zero repetitions**: no word or phrase repeated in sequence
- **Total consistency**: capitalization, apostrophes, and punctuation normalized according to uniform rules across every record
- **Zero artifacts**: no markup residues, encoding errors, HTML tags, or control characters
- **Uniform quality**: every single record in the dataset meets the same standard, from first to last

This quality standard is defined by you through the **system prompt** — describe the cleaning rules you want to apply in natural language, and PurifyFactory applies them consistently and verifiably to every record in the dataset.

### Why this changes everything for AI training

The AI market is today the fastest-growing technology market in the world. Thousands of companies are building proprietary models, fine-tuning LLMs, and developing RAG systems on enterprise data. All of these applications depend on a critical factor that is frequently underestimated: **the quality of the training data**.

Empirical research is unambiguous: a model trained on clean data with half the records consistently outperforms a model trained on dirty data with twice the records. Quality beats quantity.

PurifyFactory automates this critical phase of the ML pipeline:

- **Scalable**: works on datasets of any size, from hundreds to millions of records, with the same guaranteed quality
- **Reproducible**: the same prompt produces the same results on any dataset — no human variability
- **Auditable**: every output record pairs the original text with the cleaned text, enabling complete audits
- **On-premise**: data never leaves your infrastructure — essential for sensitive enterprise datasets
- **Provider-agnostic**: works with OpenAI, Anthropic, Google Gemini, or local models (Ollama/vLLM)

**Every hour invested in PurifyFactory before training is worth tens of hours saved in post-training, corrective fine-tuning, and model error management.**

---

## Participation Tiers

The beta program has two levels of participation. Choose the one that matches your dataset size.

---

### Tier 1 — Contributor

**Dataset requirement:** 500–4,999 records

**What we ask:**
- Process the dataset with at least 1 AI provider (online or local)
- Complete the FEEDBACK_TEMPLATE.md in full
- Send anonymized logs (`export-debug`) to betatesting@purifyfactory.com

**What you receive:**
- Early access to PurifyFactory SaaS (before public launch)
- Free license for 6 months from commercial release date
- €5 flat reimbursement (see conditions below)

**Note:** Tier 1 does not contribute to TRL5 certification, but is essential for
qualitative feedback, bug hunting, and user experience validation.

---

### Tier 2 — Certifier

**Dataset requirement:** ≥ 5,000 records (≥ 10,000 recommended for full validation)

**What we ask:**
- Process the dataset with **at least 2 different AI providers** (e.g., OpenAI + Anthropic,
  or online + local)
- Complete the FEEDBACK_TEMPLATE.md in full and in detail
- Send anonymized logs (`export-debug`) to betatesting@purifyfactory.com
- Attach a sample of at least 20 representative input/output pairs from the dataset,
  free of personal or confidential data

**What you receive:**
- Early access to PurifyFactory SaaS (before public launch)
- Lifetime free license, without rights to future automatic updates,
  with support limited exclusively to reporting and fixing critical bugs
- €10 flat reimbursement (see conditions below)

**Note:** Tier 2 directly contributes to the TRL5 certification required for the
commercial SaaS release. Your contribution is essential.

---

### Reimbursement conditions

Reimbursement is:
- **At the sole discretion of Mentora Technologies**, which reserves the right not to
  issue it in case of incomplete, invalid, or non-compliant feedback
- Issued **within 30 days** of positive feedback validation by Mentora Technologies
- Made **exclusively via PayPal or immediate bank transfer** (SEPA)
- Non-cumulative with other discounts, reimbursements, or compensation
- Non-transferable to third parties

To receive reimbursement, the Beta Tester must:
1. Complete testing according to their tier requirements
2. Send the completed FEEDBACK_TEMPLATE.md to betatesting@purifyfactory.com
3. Await validation by Mentora Technologies
4. Provide PayPal details or IBAN upon explicit request from Mentora Technologies

Mentora Technologies is not obligated to justify non-payment of reimbursement
in the event of feedback deemed invalid.

---

### How to apply

Send an email to **waitlist@purifyfactory.com** with:
- Subject: `[BETA] Application — [Tier 1 / Tier 2] — [Your Last Name]`
- Approximate size of your dataset
- AI provider(s) you plan to use
- Operating system (Linux / Windows / macOS)
- Brief description of your use case (1–3 lines)

You will receive a reply within 48 hours with instructions for signing the NDA
and receiving your license. Once accepted into the program, use **betatesting@purifyfactory.com**
for all subsequent communications.

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
- `purifyfactory-v9.1.6-beta.3-linux-x64.tar.gz` — the software (~150 MB)
- `purifyfactory-v9.1.6-beta.3-linux-x64.tar.gz.sha256` — verification checksum

**Verify integrity before extracting** (recommended):
```bash
sha256sum -c purifyfactory-v9.1.6-beta.3-linux-x64.tar.gz.sha256
# Expected: purifyfactory-v9.1.6-beta.3-linux-x64.tar.gz: OK
```

### 2 — Extract the package and generate your hardware fingerprint

```bash
tar xzf purifyfactory-v9.1.6-beta.3-linux-x64.tar.gz
cd purifyfactory-v9.1.6-beta.3-linux-x64
./purifyfactory hardware-id
```

Expected output:
```
  HARDWARE FINGERPRINT:
  5a1d49815666cc3a82910ae4a42e8214fc7798d4d9d549cbf95f2de3c682ee35

  Send this fingerprint to Mentora Technologies
  to receive your license file.
```

**Send this fingerprint to [betatesting@purifyfactory.com](mailto:betatesting@purifyfactory.com)** — you will receive your personal license within 24 hours.

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

Your dataset must be a **JSONL** file (one JSON object per line, UTF-8 encoding).

**Required text field**: each record must contain a `"sentence"` field (checked first) or a `"text"` field (fallback). Records with neither field are skipped with a warning in the log. Other fields in the record are ignored and not included in the output.

```jsonl
{"sentence": "Text to clean.", "id": 1}
{"text": "Another record.", "id": 2}
```

**If your dataset uses a different field name** (e.g. `"content"`, `"body"`, `"description"`), rename it before running the pipeline:

```python
import json
with open("input.jsonl") as fin, open("prepared.jsonl", "w") as fout:
    for line in fin:
        record = json.loads(line)
        record["sentence"] = record.pop("content")  # adapt to your field name
        fout.write(json.dumps(record, ensure_ascii=False) + "\n")
```

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
# Send the file to betatesting@purifyfactory.com
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

Fill in `FEEDBACK_TEMPLATE.md` and send it to **[betatesting@purifyfactory.com](mailto:betatesting@purifyfactory.com)** or open an Issue in this repository.

---

**Mentora Technologies** · [betatesting@purifyfactory.com](mailto:betatesting@purifyfactory.com) · [mentoratechnologies.com](https://mentoratechnologies.com)
