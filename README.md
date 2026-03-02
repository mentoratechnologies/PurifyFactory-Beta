# PurifyFactory v9.1.6 — Programma Beta

**Pipeline industriale di pulizia testo tramite AI** — Mentora Technologies

---

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

### 1 — Scarica il pacchetto

Vai alla sezione [**Releases**](../../releases) di questo repository e scarica:
- `purifyfactory-v9.1.6-beta.1-linux-x64.tar.gz` — il software (65 MB)
- `purifyfactory-v9.1.6-beta.1-linux-x64.tar.gz.sha256` — checksum di verifica

**Verifica l'integrità prima di estrarre** (consigliato):
```bash
sha256sum -c purifyfactory-v9.1.6-beta.1-linux-x64.tar.gz.sha256
# Deve rispondere: purifyfactory-v9.1.6-beta.1-linux-x64.tar.gz: OK
```

### 2 — Estrai e installa la licenza

```bash
# Estrai il pacchetto
tar xzf purifyfactory-v9.1.6-beta.1-linux-x64.tar.gz
cd purifyfactory-v9.1.6-beta.1-linux-x64

# Installa la licenza ricevuta via email
mkdir -p ~/.config/purifyfactory
cp /percorso/del/license.json ~/.config/purifyfactory/license.json

# Verifica che tutto funzioni
./purifyfactory validate
# Risposta attesa: ✅ License VALID — tier=beta
```

### 3 — Configura la tua API key

Copia il file di configurazione di esempio e inserisci la tua API key:

```bash
cp beta/examples/config_quickstart.json mio_config.json
```

Apri `mio_config.json` con un editor di testo e sostituisci `INSERISCI_LA_TUA_OPENAI_API_KEY_QUI` con la tua chiave reale. Se usi un provider diverso da OpenAI, modifica anche `"provider"` e `"model"`:

```json
"ai": {
  "provider": "openai",
  "model": "gpt-4o-mini",
  "api_keys": {
    "openai": "sk-..."
  }
}
```

Provider e modelli supportati:

| Provider | Valore `provider` | Modelli consigliati |
|---|---|---|
| OpenAI | `"openai"` | `"gpt-4o-mini"`, `"gpt-4o"` |
| Anthropic | `"anthropic"` | `"claude-haiku-4-5-20251001"` |
| Google Gemini | `"gemini"` | `"gemini-2.0-flash"` |
| Locale (Ollama) | `"local"` | nome modello Ollama (es. `"llama3.2"`) |

---

## Elaborare un dataset

La pipeline si esegue in tre passi sequenziali. Il tuo dataset deve essere un file **JSONL** (una riga JSON per record) con un campo di testo da pulire.

### Struttura del file di input

```jsonl
{"id": "001", "testo": "Il prodotto  è ottimo ottimo davvero."}
{"id": "002", "testo": "Spedizione  veloce  e imballaggio  ok ."}
```

Il campo che contiene il testo da pulire va specificato nel prompt di sistema del config. Il nome del campo non importa — PurifyFactory legge l'intero record e lo passa al modello AI.

### Step 1 — Split: suddividi il dataset in blocchi

```bash
./purifyfactory split \
  --input /percorso/del/tuo/dataset.jsonl \
  --config mio_config.json
```

Il software suddivide automaticamente il file in blocchi da 100 record (configurabile). Per dataset piccoli (<100 record) crea un unico blocco.

### Step 2 — Orchestrate: organizza i batch di lavoro

```bash
./purifyfactory orchestrate --config mio_config.json
```

Ogni blocco viene ulteriormente suddiviso in batch da 10 record (configurabile), pronti per l'elaborazione parallela.

### Step 3 — Process: elabora con l'AI

```bash
./purifyfactory process --config mio_config.json
```

I batch vengono elaborati in parallelo (1 worker in configurazione beta di default). Il progresso è visibile in tempo reale con `./purifyfactory watch` in un secondo terminale.

Al termine, il risultato si trova in:
```
~/.config/purifyfactory/data/output/final_output.jsonl
```

Ogni riga contiene:
```json
{"original_text": "Il prodotto  è ottimo ottimo.", "cleaned_text": "Il prodotto è ottimo.", "provider": "openai", "tokens": 45, "cost": 0.000010}
```

---

## Monitoraggio

Questi comandi puoi eseguirli in qualsiasi momento, anche durante un'elaborazione in corso, in un secondo terminale.

| Comando | Cosa mostra |
|---|---|
| `./purifyfactory status` | Riepilogo: split totali, batch in coda, completati, errori |
| `./purifyfactory watch` | Aggiornamento automatico ogni 3 secondi (Ctrl+C per uscire) |
| `./purifyfactory queue` | Lista dettagliata dei batch ancora in coda |
| `./purifyfactory errors` | Lista dei batch falliti con il motivo dell'errore |
| `./purifyfactory report` | Report completo: record elaborati, token usati, costo totale, distribuzione provider |
| `./purifyfactory statistics` | Statistiche aggregate: costo medio per record, utilizzo disco |

---

## Se qualcosa va storto

### Batch falliti

Se alcuni batch falliscono (API temporaneamente non disponibile, timeout, ecc.), puoi rimetterli in coda e riprovare:

```bash
./purifyfactory errors          # vedi quali batch hanno fallito e perché
./purifyfactory recover         # rimetti in coda i batch falliti
./purifyfactory process --config mio_config.json   # riprova
```

### ⚠️ Ricominciare da zero con un nuovo dataset

Se vuoi elaborare un **dataset diverso** dopo un test precedente, devi pulire lo stato manualmente prima di eseguire `split`:

```bash
rm -rf ~/.config/purifyfactory/data/state/queue/*
rm -rf ~/.config/purifyfactory/data/state/errors/*
rm -rf ~/.config/purifyfactory/data/state/progress/*
rm -rf ~/.config/purifyfactory/data/splits/*
rm -f  ~/.config/purifyfactory/data/output/final_output.jsonl
```

Poi esegui normalmente `split → orchestrate → process` con il nuovo dataset.

> Questo passaggio non è necessario se la tua elaborazione precedente si è **completata con successo**: in quel caso la pipeline assegna automaticamente un nuovo ID alla sessione.

### Problemi comuni

| Sintomo | Causa probabile | Soluzione |
|---|---|---|
| `License validation failed` | Licenza non trovata o scaduta | Verifica `~/.config/purifyfactory/license.json` |
| `Provider authentication failed` | API key errata o crediti esauriti | Controlla la chiave e il saldo sul portale del provider |
| `Batch bloccati in errore` | Errore temporaneo API | Esegui `recover` poi `process` |
| Nessun output dopo process | Tutti i batch in errore | Esegui `errors` per vedere il motivo |

### Esportare i log per il supporto

Se riscontri un problema che non riesci a risolvere, esporta i log anonimizzati:

```bash
./purifyfactory export-debug --output debug_report.zip
```

Il file non contiene API key, percorsi utente o dati personali. Invialo a **support@mentoratechnologies.com** con una breve descrizione del problema.

---

## Dimensione del dataset per la certificazione

Il dataset di esempio incluso nel pacchetto (50 record) serve solo per imparare ad usare lo strumento. Per contribuire alla certificazione TRL5 usa i tuoi dati reali:

| Dimensione | Valido per certificazione? |
|---|---|
| < 100 record | ❌ Solo per apprendimento |
| 100 – 999 record | ⚠️ Insufficiente |
| 1.000 – 4.999 record | ✅ Minimo accettato |
| ≥ 5.000 record | ✅ Consigliato |
| ≥ 10.000 record | ✅ Ottimale |

---

## Feedback

Il tuo feedback è la ragione per cui esiste questo programma beta. Compila il template in `beta/FEEDBACK_TEMPLATE.md` e invialo a **feedback@mentoratechnologies.com** oppure aprendo una Issue in questo repository.

Siamo interessati a sapere:
- Cosa ha funzionato bene e cosa no
- Casi d'uso che lo strumento non copre o copre male
- Qualsiasi comportamento inaspettato, anche se non è un errore vero e proprio

---

**Mentora Technologies** · support@mentoratechnologies.com · [mentoratechnologies.com](https://mentoratechnologies.com)
