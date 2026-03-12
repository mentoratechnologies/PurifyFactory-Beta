# PurifyFactory v9.1.6 — Guida Rapida Betatester

Benvenuto nel programma beta di PurifyFactory. Questa guida ti porta dall'installazione al primo risultato in circa 15 minuti.

---

## Prima di iniziare

Verifica di avere:
- **Python 3.10 o superiore** — controlla con `python3 --version`
- **Una API key** di OpenAI oppure Anthropic
- **Il file `license.json`** ricevuto via email da Mentora Technologies

---

## Parte 1 — Installazione (5 minuti)

### Opzione A: Binario compilato (consigliato)

```bash
# Estrai il pacchetto
tar xzf purifyfactory-v9.1.6-linux-x64.tar.gz
cd purifyfactory-v9.1.6

# Verifica che funzioni
./purifyfactory info
```

### Opzione B: Pacchetto Python

```bash
# Crea un ambiente virtuale isolato (obbligatorio su Ubuntu 23.04+)
python3 -m venv .venv
source .venv/bin/activate   # Linux / macOS
# oppure: .\.venv\Scripts\Activate.ps1   (Windows PowerShell)

# Installa con tutti i provider AI
pip install purifyfactory[full]

# Verifica che funzioni
python -m purifyfactory info
```

### Installa la licenza

Il binario salva la configurazione nella directory standard del sistema operativo:

```bash
# Linux
mkdir -p ~/.config/purifyfactory
cp /percorso/del/license.json ~/.config/purifyfactory/license.json

# macOS
mkdir -p ~/Library/Application\ Support/PurifyFactory
cp /percorso/del/license.json ~/Library/Application\ Support/PurifyFactory/license.json

# Windows (PowerShell)
New-Item -ItemType Directory -Force "$env:APPDATA\PurifyFactory"
Copy-Item license.json "$env:APPDATA\PurifyFactory\license.json"
```

```bash
# Verifica la licenza
./purifyfactory validate          # binario
# oppure: python -m purifyfactory validate   (Python)
```

Dovresti vedere: `✅ License validation: SUCCESS — tier=beta`

---

## Formato del dataset

PurifyFactory legge file **JSONL** (JSON Lines): un oggetto JSON per riga, codifica UTF-8.

**Campo testo**: ogni record deve contenere un campo chiamato `"sentence"` oppure `"text"`. Il sistema cerca prima `"sentence"`, poi `"text"` come fallback. Se nessuno dei due è presente, il record viene saltato (con un warning nel log).

```jsonl
{"sentence": "Testo da pulire.", "id": 1}
{"text": "Altro record.", "id": 2}
```

Gli altri campi del record (es. `"id"`, `"source"`) vengono ignorati durante il processing e **non vengono copiati nell'output**.

**Se il tuo dataset usa un campo con nome diverso** (es. `"content"`, `"body"`, `"description"`), rinominalo prima di avviare la pipeline:

```python
import json

with open("input.jsonl") as fin, open("prepared.jsonl", "w") as fout:
    for line in fin:
        record = json.loads(line)
        record["sentence"] = record.pop("content")  # adatta al tuo campo
        fout.write(json.dumps(record, ensure_ascii=False) + "\n")
```

Il dataset di esempio incluso (`beta/examples/sample_dataset.jsonl`) usa già il campo `"sentence"` e funziona senza modifiche.

---

## Parte 2 — Primo run con il dataset di esempio (10 minuti)

### 1. Configura la tua API key

Apri `beta/examples/config_quickstart.json` e sostituisci `INSERISCI_LA_TUA_OPENAI_API_KEY_QUI` con la tua chiave reale:

```json
"api_keys": {
  "openai": "sk-..."
}
```

Se usi Anthropic invece di OpenAI, cambia anche `provider` e `model`:

```json
"provider": "anthropic",
"model": "claude-haiku-4-5-20251001",
"api_keys": {
  "anthropic": "sk-ant-..."
}
```

### 2. Esegui la pipeline

I comandi seguenti usano il dataset di esempio (50 frasi di testo italiano con difetti tipici).

```bash
# Step 1: Suddividi il dataset in blocchi
./purifyfactory split \
  --config beta/examples/config_quickstart.json \
  --input beta/examples/sample_dataset.jsonl

# Step 2: Organizza i batch di lavoro
./purifyfactory orchestrate \
  --config beta/examples/config_quickstart.json

# Step 3: Elabora con l'AI
./purifyfactory process \
  --config beta/examples/config_quickstart.json
```

> Se usi il pacchetto Python, sostituisci `./purifyfactory` con `python -m purifyfactory`.

### 3. Controlla i risultati

```bash
# Stato dell'elaborazione
./purifyfactory status

# Report dettagliato con costi
./purifyfactory report

# Visualizza il testo pulito (Linux)
cat ~/.config/purifyfactory/data/output/final_output.jsonl
# macOS: cat ~/Library/Application\ Support/PurifyFactory/data/output/final_output.jsonl
# Windows: type %APPDATA%\PurifyFactory\data\output\final_output.jsonl
```

Il file `final_output.jsonl` contiene ogni record nella forma:
```json
{"original_text": "testo originale...", "cleaned_text": "testo pulito...", "provider": "openai", "tokens": 45, "cost": 0.000025}
```

---

## Monitoraggio in tempo reale

Durante un'elaborazione lunga puoi tenere aperto un secondo terminale e usare:

```bash
./purifyfactory watch       # aggiornamento automatico ogni 3 secondi (Ctrl+C per uscire)
./purifyfactory statistics  # utilizzo totale e costi per provider
./purifyfactory errors      # mostra eventuali batch falliti
```

Se alcuni batch falliscono, puoi rimandare in coda per un nuovo tentativo:

```bash
./purifyfactory recover
./purifyfactory process --config beta/examples/config_quickstart.json
```

---

## ⚠️ Ricominciare da zero (nuovo dataset o nuovo tentativo)

**Quando ti serve**: se hai già eseguito una pipeline (anche parzialmente) e vuoi elaborare un **dataset diverso**, oppure vuoi **ripartire da zero** dopo una configurazione sbagliata.

**Il problema**: PurifyFactory mantiene lo stato tra una sessione e l'altra. Se nella coda sono rimasti batch di un'elaborazione precedente — perché è stata interrotta, o hai usato `recover` — verranno elaborati insieme ai nuovi batch del dataset successivo. Il risultato è un mix di dati che probabilmente non vuoi.

Questo succede tipicamente quando:
- Hai testato con `sample_dataset.jsonl` e ora vuoi elaborare i tuoi dati reali
- Hai cambiato provider o configurazione e vuoi ignorare il lavoro precedente
- Un'elaborazione è andata storta e vuoi buttare tutto e ripartire

**Soluzione**: prima di lanciare `split` sul nuovo dataset, pulisci lo stato manualmente.

```bash
# Linux
rm -rf ~/.config/purifyfactory/data/state/queue/*
rm -rf ~/.config/purifyfactory/data/state/errors/*
rm -rf ~/.config/purifyfactory/data/state/progress/*
rm -rf ~/.config/purifyfactory/data/splits/*
rm -f  ~/.config/purifyfactory/data/output/final_output.jsonl

# macOS
DATA="$HOME/Library/Application Support/PurifyFactory/data"
rm -rf "$DATA/state/queue/"* "$DATA/state/errors/"* "$DATA/state/progress/"*
rm -rf "$DATA/splits/"*
rm -f  "$DATA/output/final_output.jsonl"

# Windows (PowerShell)
$data = "$env:APPDATA\PurifyFactory\data"
Remove-Item "$data\state\queue\*"    -Recurse -Force
Remove-Item "$data\state\errors\*"   -Recurse -Force
Remove-Item "$data\state\progress\*" -Recurse -Force
Remove-Item "$data\splits\*"         -Recurse -Force
Remove-Item "$data\output\final_output.jsonl" -Force
```

Poi esegui la pipeline normalmente con il nuovo dataset.

> **Nota bene**: se la tua elaborazione precedente si è **completata con successo** (tutti i batch in stato `completed`), non hai questo problema. La pipeline assegna automaticamente un nuovo ID alla sessione successiva e i vecchi batch completati non interferiscono. La pulizia manuale è necessaria solo quando rimangono batch **in coda** o **in errore** da un'esecuzione precedente.

---

## Esportare i log per il supporto

Se riscontri un problema, esporta i log anonimizzati e inviaceli:

```bash
./purifyfactory export-debug --output debug_report.zip
```

Il file ZIP non contiene API key, percorsi utente o dati sensibili. Invialo a **support@mentoratech.com** con una breve descrizione del problema.

---

## Problemi comuni

| Sintomo | Soluzione |
|--------|-----------|
| `License validation failed` | Verifica che la licenza esista nel percorso corretto (`~/.config/purifyfactory/license.json` su Linux) e non sia scaduta |
| `Provider authentication failed` | Controlla che la API key nel config sia corretta e attiva |
| `ModuleNotFoundError` (solo Python) | Assicurati di aver attivato il venv: `source .venv/bin/activate` |
| Batch bloccati in coda | Esegui `./purifyfactory errors` per vedere il motivo, poi `recover` |
| Output vuoto | Esegui `./purifyfactory status` — verifica che tutti i batch siano `completed` |

---

## Dimensione del dataset per la certificazione TRL5

Il dataset di esempio incluso (50 record) è utile solo per imparare lo strumento. Per contribuire alla certificazione del software è necessario usare dataset di dimensioni adeguate con i propri dati reali.

| Dimensione | Significato | Certificazione TRL5 |
|---|---|---|
| < 100 record | Solo per apprendere lo strumento | ❌ Non utilizzabile |
| 100 – 999 record | Test funzionale di base | ⚠️ Insufficiente |
| 1.000 – 4.999 record | Soglia minima accettabile | ✅ Minimo accettato |
| ≥ 5.000 record | Validazione completa pipeline | ✅ Consigliato |
| ≥ 10.000 record | Validazione scalabilità e throughput | ✅ Ottimale |

Il software ti avvisa automaticamente con un messaggio `WARNING` se il dataset è al di sotto della soglia minima.

Per risultati ottimali, usa dati reali del tuo dominio applicativo (testi aziendali, documenti, feed di dati, ecc.) che rappresentino i casi d'uso che intendi normalizzare in produzione.

---

## Prossimi passi

- Prova con i **tuoi dati**: sostituisci `sample_dataset.jsonl` con il tuo file JSONL — prima leggi la sezione [⚠️ Ricominciare da zero](#️-ricominciare-da-zero-nuovo-dataset-o-nuovo-tentativo) per pulire lo stato del test precedente
- Adatta il **system prompt** nel config al tuo caso d'uso specifico
- Usa un dataset di **almeno 1.000 record** (consigliati ≥ 5.000) per contribuire alla certificazione
- Leggi `docs/GUIDA_UTENTE.md` per la documentazione completa
- Compila il **form di feedback** (vedi `beta/FEEDBACK_TEMPLATE.md`) — il tuo contributo è fondamentale

---

**Mentora Technologies** — support@mentoratech.com
