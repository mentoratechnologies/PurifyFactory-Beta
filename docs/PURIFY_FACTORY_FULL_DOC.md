# PurifyFactory — Documentazione Completa

**Versione documento:** 1.0 — Marzo 2026
**Versione software:** 9.1.6 "Engine Solido"
**Autore:** Mentora Technologies

---

## Indice

1. [Che cos'è PurifyFactory](#1-che-cosè-purifyfactory)
2. [Problema che risolve](#2-problema-che-risolve)
3. [A chi è rivolto](#3-a-chi-è-rivolto)
4. [Come funziona — Architettura](#4-come-funziona--architettura)
5. [La Pipeline in dettaglio](#5-la-pipeline-in-dettaglio)
6. [Provider AI supportati](#6-provider-ai-supportati)
7. [Interfacce di utilizzo](#7-interfacce-di-utilizzo)
8. [Sistema di licenze](#8-sistema-di-licenze)
9. [Versioni e Tier](#9-versioni-e-tier)
10. [Modalità di distribuzione](#10-modalità-di-distribuzione)
11. [Sicurezza e protezione](#11-sicurezza-e-protezione)
12. [Stato attuale](#12-stato-attuale)
13. [Roadmap e sviluppi futuri](#13-roadmap-e-sviluppi-futuri)
14. [Stack tecnologico](#14-stack-tecnologico)
15. [Struttura del progetto](#15-struttura-del-progetto)

---

## 1. Che cos'è PurifyFactory

PurifyFactory è una **pipeline industriale di pulizia testi tramite intelligenza artificiale**, progettata per elaborare grandi volumi di dati testuali in modo affidabile, scalabile e controllato.

Il nome riflette la natura del prodotto: è una "fabbrica" (factory) che prende dati testuali grezzi in ingresso e produce in uscita testi "purificati" (purify) — corretti, normalizzati, filtrati o trasformati secondo le istruzioni definite dall'utente.

PurifyFactory non è un semplice wrapper attorno alle API AI: è un sistema di produzione completo, con gestione degli errori, retry automatico, parallelismo controllato, sistema di licenze enterprise, monitoraggio in tempo reale e protezione del binario.

---

## 2. Problema che risolve

### Il problema

Chiunque lavori con grandi dataset testuali — aziende, ricercatori, team di data science — si trova spesso a dover eseguire operazioni di pulizia massiva su testi:

- Correzione grammaticale e ortografica
- Normalizzazione del formato
- Rimozione di contenuti inappropriati o irrilevanti
- Traduzione o trasformazione stilistica
- Estrazione di informazioni strutturate
- Anonimizzazione (rimozione di PII)
- Qualsiasi altra trasformazione definibile tramite prompt AI

Usare direttamente le API AI per questo tipo di lavoro presenta problemi concreti:

| Problema | Impatto |
|---|---|
| Nessuna gestione dei rate limit | Il processo si blocca o perde dati |
| Nessun retry in caso di errore | I batch falliti vanno persi |
| Nessun parallelismo controllato | Lento su grandi dataset |
| Nessun monitoraggio | Non si sa a che punto è l'elaborazione |
| Nessuna ripresa dopo crash | Si riparte dall'inizio |
| Costi non tracciati | Sorprese in fattura |

### La soluzione

PurifyFactory risolve tutti questi problemi con un'architettura a pipeline:

- **Divide** il dataset in batch gestibili
- **Elabora** i batch in parallelo con worker multipli
- **Riprova** automaticamente in caso di errore (con backoff esponenziale)
- **Traccia** ogni batch (stato: in coda, in elaborazione, completato, errore)
- **Monitora** in tempo reale costi, token, progresso
- **Riprende** dall'ultimo punto in caso di interruzione
- **Funziona offline** una volta configurato (le API keys sono del cliente)

---

## 3. A chi è rivolto

### Clienti target primari (on-prem)

**Data scientist e team AI**
Lavorano con dataset di testo da decine di migliaia a milioni di record. Hanno bisogno di elaborare grandi quantità di testi in modo ripetibile e tracciabile, senza dover costruire da zero l'infrastruttura di gestione delle code e degli errori.

**Aziende con dati sensibili**
Non possono usare servizi cloud SaaS perché i loro dati non possono uscire dall'infrastruttura aziendale. PurifyFactory gira interamente on-prem: i dati non lasciano mai i server del cliente (le API key sono del cliente, le chiamate AI vengono fatte direttamente dal server del cliente verso Anthropic/OpenAI/Gemini).

**Team di NLP e ricerca**
Devono preparare corpus di addestramento per modelli linguistici. La pulizia e normalizzazione di grandi dataset testuali è un'operazione ricorrente e costosa in termini di tempo.

**Aziende che usano modelli locali**
Con il provider "Local" (Ollama, vLLM), PurifyFactory può elaborare dati senza alcuna chiamata a API esterne, con costo zero per token e massima privacy.

### Mercato secondario (futuro SaaS)

**Freelance e piccole aziende**
Non vogliono o non possono gestire infrastruttura on-prem. Accedono via browser con un piano mensile, pagano solo per quello che usano.

---

## 4. Come funziona — Architettura

### Visione d'insieme

```
INPUT (file .jsonl)
        │
        ▼
┌───────────────┐
│    SPLITTER   │  Divide il dataset in blocchi (split)
└───────┬───────┘
        │  N file split (es. 10.000 record → 10 split da 1.000)
        ▼
┌───────────────┐
│ ORCHESTRATOR  │  Suddivide ogni split in batch e li mette in coda
└───────┬───────┘
        │  M batch in queue/ (es. ogni split → 20 batch da 50 record)
        ▼
┌───────────────────────────────┐
│          MANAGER              │
│  ┌────────┐  ┌────────┐       │
│  │Worker 1│  │Worker 2│  ...  │  N worker in parallelo (subprocess)
│  └────────┘  └────────┘       │
│    AI API      AI API         │  Ogni worker chiama il provider AI
└───────┬───────────────────────┘
        │
        ├──── output/  (batch completati)
        └──── errors/  (batch falliti)
                │
                ▼
        ┌───────────────┐
        │   SUPERVISOR  │  (BETA/ENTERPRISE) — daemon che monitora
        │               │  e rimette in coda i batch falliti
        └───────────────┘
```

### Principi architetturali

**Subprocess isolation**
Ogni worker è un processo separato (`subprocess`), non un thread. Questo garantisce che un crash di un worker non abbatta l'intera pipeline. Il manager rileva il crash e rimette il batch in coda.

**Atomic file writes**
Ogni scrittura su disco usa il pattern `tmp + rename`: si scrive su un file temporaneo e solo quando la scrittura è completa si rinomina al file definitivo. Questo elimina la possibilità di file corrotti in caso di crash.

**File locking**
L'accesso concorrente ai file di stato è protetto da `portalocker`, che garantisce che due worker non scrivano mai sullo stesso file simultaneamente.

**Stato persistente**
Ogni batch ha uno stato (`queued`, `processing`, `completed`, `error`, `interrupted`) salvato su disco. Se il processo viene interrotto (Ctrl+C, crash, riavvio del server), alla ripresa PurifyFactory sa esattamente da dove ricominciare.

**Singleton PathManager**
Tutti i percorsi del filesystem (queue, output, errors, logs, state) passano attraverso un singleton `PathManager.instance()` che garantisce coerenza in tutta l'applicazione.

**Graceful shutdown**
Un `threading.Event` centralizzato (`shutdown_event`) coordina lo spegnimento ordinato di tutti i worker: ogni worker termina il batch corrente prima di fermarsi, senza perdere dati.

---

## 5. La Pipeline in dettaglio

### Fase 1 — SPLIT

```bash
purifyfactory split --config config.json
```

Il comando split legge il file di input (JSONL, un record per riga con campo `sentence` o `text`) e lo divide in N file di dimensione configurabile (`split_size`, default 10.000 record).

Ogni file split viene scritto in `data/splits/` e il suo stato viene registrato in `data/state/split.state`.

**Perché dividere in split?**
Lavorare su file da milioni di record in memoria non è praticabile. La divisione in split permette di:
- Tenere il consumo di memoria sotto controllo
- Riprendere da un punto intermedio in caso di interruzione
- Processare split diversi in parallelo (futuro)

### Fase 2 — ORCHESTRATE

```bash
purifyfactory orchestrate --config config.json
```

L'orchestratore legge i file split e crea i **batch**: unità di elaborazione atomiche da N record (default 50 per BETA/PRO, 1.000 per ENTERPRISE). Ogni batch è un file JSON in `data/queue/` con stato iniziale `queued`.

L'orchestratore è **idempotente**: se viene rieseguito su uno split già orchestrato, salta i batch già esistenti (non crea duplicati).

### Fase 3 — PROCESS

```bash
purifyfactory process --config config.json
```

Il manager lancia N worker in parallelo (N = `max_parallel_workers` da config, limitato dal tier). Ogni worker:

1. Preleva un batch dalla coda (con file lock per evitare conflitti)
2. Imposta lo stato del batch su `processing`
3. Per ogni record nel batch, chiama il provider AI con il prompt configurato
4. Gestisce rate limit con retry + backoff esponenziale
5. In caso di errore sul provider primario, tenta il fallback sul provider secondario
6. Scrive l'output in `data/output/`
7. Imposta lo stato del batch su `completed` o `error`

Il manager tiene vivi i worker finché la coda non è vuota, poi termina in modo ordinato.

### Fase 4 — SUPERVISE (BETA / ENTERPRISE)

```bash
purifyfactory supervise --config config.json
```

Il supervisor è un daemon che gira continuamente. Ogni N secondi (configurabile):
- Scansiona `data/errors/` per batch falliti
- Li rimette automaticamente in coda (`queued`)
- Rilancia il processo se necessario

Fornisce auto-recovery senza intervento manuale. Va avviato in background (es. con `nohup` o `systemd`).

### Recovery manuale (tutti i tier)

```bash
purifyfactory recover
```

Equivalente manuale del supervisor: rimette in coda tutti i batch in errore in un'unica operazione. Da eseguire prima di un nuovo `process`.

---

## 6. Provider AI supportati

PurifyFactory supporta quattro provider AI attraverso un'interfaccia unificata.

### Anthropic (Claude)

Modelli supportati:
- `claude-opus-4-6` — massima qualità, più costoso
- `claude-sonnet-4-6` — bilanciato (default consigliato)
- `claude-haiku-4-5-20251001` — veloce ed economico

Caratteristiche: gestione automatica rate limit 429, retry con backoff, stima token pre-chiamata.

### OpenAI (GPT)

Modelli supportati:
- `gpt-4o` — qualità massima
- `gpt-4o-mini` — economico, buona qualità
- `gpt-4-turbo` — contesto lungo

### Google Gemini

Modelli supportati:
- `gemini-2.5-pro` — qualità massima
- `gemini-2.0-flash` — veloce
- `gemini-1.5-pro` / `gemini-1.5-flash`

### Local (Ollama / vLLM / LM Studio)

Qualsiasi modello esposto tramite API compatibile OpenAI. Il provider Local permette:
- **Costo zero** per token (il modello gira localmente)
- **Massima privacy** (nessun dato inviato a server esterni)
- **Qualsiasi modello open source** (Llama 3, Mistral, Qwen, ecc.)

Configurazione: impostare `api_keys.local` con l'URL dell'endpoint (es. `http://localhost:11434/v1`).

### Fallback multi-provider

Se il provider primario restituisce un errore non recuperabile (quota esaurita, servizio non disponibile), PurifyFactory passa automaticamente al provider di fallback configurato, senza interrompere l'elaborazione.

---

## 7. Interfacce di utilizzo

### CLI — Command Line Interface

15 comandi disponibili:

| Comando | Descrizione |
|---|---|
| `split` | Divide il dataset in split |
| `orchestrate` | Crea i batch dalla coda |
| `process` | Elabora i batch con worker AI |
| `supervise` ★ | Daemon auto-recovery (BETA/ENTERPRISE) |
| `recover` | Rimette in coda i batch falliti (manuale) |
| `status` | Riepilogo contatori (completed/queue/errors) |
| `watch` | Monitoraggio live aggiornato ogni 5s |
| `report` | Report costi e statistiche elaborazione |
| `statistics` | Statistiche dettagliate per record |
| `errors` | Lista batch in errore con dettaglio |
| `queue list` | Lista batch in coda |
| `queue clear` | Svuota la coda |
| `validate` | Verifica licenza (tier, holder, scadenza) |
| `hardware-id` | Fingerprint hardware (per richiedere licenza) |
| `export-debug` | ZIP con log e stato anonimizzato (supporto) |
| `info` | Info sistema, percorsi, versione |
| `tui` | Avvia l'interfaccia TUI interattiva |

### TUI — Terminal User Interface

```bash
purifyfactory tui --config config.json
```

Interfaccia interattiva a schermo intero basata su [Textual](https://textual.textualize.io/). Richiede `pip install .[tui]` o la variante del pacchetto con TUI inclusa.

**Navigazione:** tasti `1`-`4` per cambiare schermata, `r` per aggiornare, `q` per uscire.

**Schermata 1 — Dashboard**
- Stato pipeline in tempo reale (completati / in coda / in errore / in elaborazione)
- Informazioni licenza (tier, holder, data scadenza)
- Totali: record elaborati, token consumati, costo USD
- Aggiornamento automatico ogni 5 secondi

**Schermata 2 — Pipeline Control**
- Avvio pipeline completa con un click (split → orchestrate → process)
- Avvio con Supervise incluso (solo ENTERPRISE/BETA)
- Controllo manuale fase per fase (Split, Orchestrate, Process, Supervise)
- Log output in tempo reale
- Pulsante Stop per interrompere il processo corrente

**Schermata 3 — Monitor**
- Barra di progresso percentuale
- Tabella batch per stato (completati, in coda, in elaborazione, errori, interrotti)
- Report costi aggiornato in tempo reale
- Uso BETA (cap token/record, colore verde/giallo/rosso)

**Schermata 4 — Strumenti**
- Visualizzazione errori e recovery
- Gestione coda (mostra, svuota)
- Diagnostica: valida licenza, info sistema, hardware ID
- Export debug ZIP
- Log output dei comandi

---

## 8. Sistema di licenze

### Architettura del sistema

Il sistema di licenze è basato su **crittografia RSA 4096-bit** con **hardware fingerprinting**. Ogni licenza è:

1. **Firmata** con la chiave privata RSA del vendor (Mentora Technologies)
2. **Legata all'hardware** del cliente tramite fingerprint (CPU ID, scheda madre, disco)
3. **Con scadenza** incorporata nel payload firmato
4. **Verificabile offline** (non richiede connessione internet per funzionare)

### Formato della licenza

```json
{
  "license_key": "PURIFY-PRO-XXXX-XXXX-XXXX",
  "tier": "pro",
  "issued_date": "2026-01-15",
  "expires": "2027-01-15",
  "holder": "Azienda Cliente Srl",
  "hardware_fingerprint": "a7f3b8c9d2e1f0...",
  "license_type": "offline_hardware",
  "signature": "BASE64_FIRMA_RSA_4096"
}
```

### Come funziona la validazione

All'avvio di qualsiasi comando, PurifyFactory:

1. Legge `license.json` dalla directory di configurazione
2. Verifica la firma RSA con la chiave pubblica embedded nel binario
3. Calcola l'Hardware ID della macchina corrente
4. Confronta con l'Hardware ID nella licenza
5. Verifica la data di scadenza
6. Applica i limiti del tier (workers, batch size, funzionalità)

Se la validazione fallisce, il software si blocca con un messaggio d'errore chiaro.

### Percorso del file licenza

| Sistema | Percorso |
|---|---|
| Linux | `~/.config/purifyfactory/license.json` |
| Windows | `%APPDATA%\PurifyFactory\license.json` |
| macOS | `~/Library/Application Support/PurifyFactory/license.json` |

### Flusso di emissione licenza (vendor)

```
1. Cliente acquista (Stripe Payment Link)
2. Cliente esegue: purifyfactory hardware-id
3. Cliente invia l'Hardware ID al vendor via email
4. Vendor esegue:
   python tools/license_tool.py issue \
     --tier pro \
     --holder "Nome Cliente" \
     --hardware-id "a7f3b8..." \
     --duration 365 \
     --output cliente_license.json
5. Vendor invia license.json al cliente via email
6. Cliente copia license.json nella directory di configurazione
7. Cliente verifica: purifyfactory validate
```

### Validazione online (opzionale)

Il software può effettuare verifiche periodiche con i server del vendor. In assenza di connessione, funziona in modalità offline per il periodo di grazia configurato (default 365 giorni per ENTERPRISE). La validazione online è un layer aggiuntivo di sicurezza, non un requisito per il funzionamento.

### Hardware fingerprinting

L'Hardware ID è calcolato combinando:
- Identificatore CPU (model name + serial se disponibile)
- Identificatore scheda madre (via DMI/WMI)
- Identificatore disco primario

L'algoritmo è deterministico: lo stesso hardware produce sempre lo stesso ID. Se l'hardware cambia (guasto, upgrade), il cliente deve richiedere una nuova licenza.

---

## 9. Versioni e Tier

### Panoramica

```
┌─────────────────────────────────────────────────────────────────────┐
│                      TIER SYSTEM                                     │
│                                                                      │
│  FREE          PRO             ENTERPRISE        BETA               │
│  (SaaS only)   (on-prem)       (on-prem)         (betatester)       │
│                                                                      │
│  Gratuito      €XX/anno        €XXX/anno         Gratuito (test)    │
└─────────────────────────────────────────────────────────────────────┘
```

### FREE (solo SaaS — futuro)

Il tier gratuito sarà disponibile esclusivamente nella versione SaaS (non esiste un binario on-prem FREE). Pensato per provare il servizio con volumi limitati.

| Parametro | Valore |
|---|---|
| Worker paralleli | 1 |
| Batch size | 20 record |
| Record/mese | 5.000 |
| Provider AI | Solo Anthropic |
| Fallback | No |
| Auto-recovery | No |
| Supervisor | No |
| Prezzo | Gratuito |

### PRO (on-prem)

Il tier principale per professionisti e piccole/medie aziende. Gira interamente on-prem, le API key sono del cliente.

| Parametro | Valore |
|---|---|
| Worker paralleli | 4 |
| Batch size | 50 record |
| Split size | 10.000 record |
| Provider AI | Tutti (Anthropic, OpenAI, Gemini, Local) |
| Fallback multi-provider | Sì |
| Auto-recovery | No (recovery manuale con `recover`) |
| Supervisor daemon | No |
| TUI interattiva | Sì |
| Logging JSON strutturato | No |
| Telemetria avanzata | No |
| Validazione licenza online | Sì (grace period: 90 giorni) |
| Distribuzione | Binario compilato (.tar.gz Linux / .zip Windows) |

### ENTERPRISE (on-prem)

Il tier per grandi aziende con volumi elevati e requisiti di continuità operativa.

| Parametro | Valore |
|---|---|
| Worker paralleli | 64 |
| Batch size | 1.000 record |
| Split size | 50.000 record |
| Provider AI | Tutti |
| Fallback multi-provider | Sì |
| Auto-recovery | Sì (supervisor daemon incluso) |
| Supervisor daemon | Sì |
| TUI interattiva | Sì (con Supervise attivo) |
| Logging JSON strutturato | Sì |
| Telemetria avanzata | Sì |
| Validazione licenza online | Sì (grace period: 365 giorni) |
| Distribuzione | Binario compilato (.tar.gz Linux / .zip Windows) |

### BETA (betatester — temporaneo)

Tier speciale per la fase di test con utenti selezionati. Ha le stesse funzionalità di ENTERPRISE ma con un cap sul consumo (token/record) per controllare i costi durante la fase beta.

| Parametro | Valore |
|---|---|
| Funzionalità | Come ENTERPRISE |
| Cap token/record | Sì (monitorato via `statistics`) |
| Durata licenza | 90 giorni |
| Costo | Gratuito |
| Provider Local | Sì |

### Confronto on-prem

| Feature | PRO | ENTERPRISE | BETA |
|---|---|---|---|
| Workers | 4 | 64 | 4 |
| Batch size | 50 | 1.000 | 50 |
| Split size | 10.000 | 50.000 | 10.000 |
| Fallback provider | ✅ | ✅ | ✅ |
| Recovery manuale | ✅ | ✅ | ✅ |
| Supervisor daemon | ❌ | ✅ | ✅ |
| TUI — Supervise attivo | ❌ | ✅ | ✅ |
| Provider Local | ✅ | ✅ | ✅ |
| Logging JSON | ❌ | ✅ | ✅ |
| Cap BETA | ❌ | ❌ | ✅ |

---

## 10. Modalità di distribuzione

### Canale A — On-Prem (attuale)

Il software viene distribuito come **binario compilato** (non come codice Python). Il cliente riceve un archivio compresso che contiene:

```
PurifyFactory_v9.1.6_linux_pro.tar.gz
├── purifyfactory          ← Binario ELF compilato con Nuitka
├── config_template.json   ← Template di configurazione
├── INSTALL.md             ← Guida installazione rapida
├── QUICKSTART.md          ← Avvio rapido
├── CONFIGURATION.md       ← Riferimento configurazione
└── LICENSE.txt            ← EULA
(+ license.json consegnato separatamente via email)
```

**Vantaggi del binario compilato:**
- Il cliente non ha accesso al codice sorgente Python
- Non richiede l'installazione di Python o dipendenze
- Avvio più veloce (nessun interprete da caricare)
- Protezione con obfuscation e integrity check SHA-256

**Piattaforme supportate:**
- Linux x86_64 (testato su Ubuntu 22.04+, Debian 11+)
- Windows x64 (in sviluppo)
- macOS (pianificato)

**Flusso di distribuzione on-prem:**
```
1. Cliente → Acquisto via Stripe Payment Link
2. Cliente → Scarica il binario (link sulla landing page)
3. Cliente → Esegue: ./purifyfactory hardware-id
4. Cliente → Invia Hardware ID via email al vendor
5. Vendor  → Genera license.json con tools/license_tool.py
6. Vendor  → Invia license.json via email al cliente
7. Cliente → Copia license.json in ~/.config/purifyfactory/
8. Cliente → Verifica: ./purifyfactory validate
9. Cliente → Utilizza il software
```

### Canale B — SaaS (futuro, Fase 3)

Il cliente non installa nulla: accede via browser a una piattaforma web. Carica il dataset, imposta il prompt, lancia l'elaborazione, scarica i risultati. Il modello di business è ad abbonamento mensile/annuale.

La pipeline PurifyFactory sarà il motore backend dell'applicazione SaaS, esposta tramite REST API.

**Stack pianificato:**
- Backend: Flask o FastAPI (wrappa la pipeline PurifyFactory)
- Frontend: applicazione web React/Vue
- Auth: email + password, eventualmente OAuth
- Pagamenti: Stripe Subscription
- Storage: S3 o equivalente per dataset upload/download
- Hosting: VPS o cloud (AWS, DigitalOcean, Hetzner)

---

## 11. Sicurezza e protezione

### Protezione del binario

**Compilazione Nuitka**
Il codice Python viene compilato a codice C e poi a binario nativo. Non è possibile fare un semplice `import` del codice o vederlo con un decompilatore Python standard.

**Obfuscation stringhe**
Le stringhe critiche (URL, costanti di licensing) vengono offuscate prima della compilazione tramite un modulo di obfuscation XOR.

**Integrity check SHA-256**
Il binario calcola il proprio hash all'avvio e lo confronta con un hash di riferimento salvato in `integrity_hash.json`. Se il file è stato modificato (patchato), il software si blocca.

**Key vault**
La chiave pubblica RSA è frammentata e distribuita in più punti del codice sorgente prima della compilazione, rendendo difficile l'estrazione e la sostituzione.

### Sicurezza licenza

**RSA 4096-bit**
La firma digitale usa RSA con chiave a 4096 bit. Non è computazionalmente fattibile falsificare una licenza senza la chiave privata.

**Hardware binding**
La licenza è legata all'hardware del cliente. Una licenza copiata su un'altra macchina non funziona.

**Revoca**
Il vendor può revocare una licenza in qualsiasi momento tramite `license_tool.py revoke`. Alla prossima validazione online, il software si blocca.

### Sicurezza applicativa

**VUL-1 — SSRF (risolto in v9.1.6)**
Il provider Local accetta solo URL `http://` e `https://`. Schemi non-HTTP (file://, ftp://) vengono bloccati per prevenire Server-Side Request Forgery.

**VUL-2 — Licenza silenziosa (risolto in v9.1.6)**
Se nessun file di licenza viene trovato, il software logga un WARNING esplicito invece di procedere silenziosamente.

**VUL-3 — Path traversal (risolto in v9.1.6)**
Il percorso di output da configurazione viene canonicalizzato con `.resolve()` per espandere `..` e symlink.

**VUL-5 — Log LOCAL endpoint (risolto in v9.1.6)**
Quando un betatester usa il provider Local, il software logga l'endpoint atteso per facilitare la diagnostica se Ollama/vLLM non è in ascolto.

---

## 12. Stato attuale

### Versione corrente: 9.1.6 "Engine Solido"

**Data rilascio beta.2:** 14 marzo 2026

### Cosa funziona

- ✅ Pipeline completa: split → orchestrate → process → supervise → recover
- ✅ 4 provider AI: Anthropic, OpenAI, Gemini, Local
- ✅ Fallback multi-provider automatico
- ✅ Sistema tier con enforcement (FREE/BETA/PRO/ENTERPRISE)
- ✅ Sistema licenze RSA 4096-bit con hardware binding
- ✅ License tool completo (keygen, issue, verify, embed, revoke, batch)
- ✅ Binario Linux compilato con Nuitka (123 MB)
- ✅ Pacchetto beta.2 su GitHub Release
- ✅ TUI interattiva a 4 schermate (Textual)
- ✅ Test suite: 124/124 test passati
- ✅ Documentazione cliente completa (GUIDA_UTENTE, QUICKSTART, CONFIG, INSTALL)
- ✅ Fix sicurezza VUL-1/2/3/5
- ✅ EULA bozza

### Metriche codebase

| Metrica | Valore |
|---|---|
| File Python | 48 |
| Righe di codice | ~15.400 |
| Test | 124 (84 unit + 17 integration + 23 e2e beta) |
| Copertura test | ~90% |
| Dipendenze produzione | loguru, orjson, pydantic, portalocker, requests, cryptography |
| Dipendenze opzionali | textual (TUI), anthropic, openai, google-genai |

### Test end-to-end confermati

Test eseguito con binario compilato, licenza BETA, provider OpenAI gpt-4o-mini, 15 record:
- ✅ split (1 split, 15 record)
- ✅ orchestrate (3 batch da 5 record)
- ✅ process (3/3 batch completati, 15/15 record puliti, 0 errori)
- ✅ status / report / statistics
- ✅ errors / recover / export-debug
- ✅ supervise (tier beta OK, shutdown pulito)

---

## 13. Roadmap e sviluppi futuri

### Fase 2 — Lancio commerciale (prossima)

| Task | Stato | Descrizione |
|---|---|---|
| Build Windows | 🔜 | Compilare il .exe con Nuitka su macchina Windows |
| Script assemblaggio Windows | 🔜 | Equivalente di assemble_beta_package.py per .zip |
| Aggiornamento pacchetto (TUI + EULA) | 🔜 | Il pacchetto beta.2 non include TUI né EULA |
| EULA | ✅ bozza | Contratto licenza utente finale (da finalizzare con avvocato) |
| Struttura fiscale | ⏳ | P.IVA individuale o SRL (prerequisito per vendere) |
| Landing page | 🔜 | Pagina vetrina con prezzi e Payment Link Stripe |
| Stripe | 🔜 | Account + Payment Link per PRO e ENTERPRISE |
| Primi clienti | 🔜 | Processo manuale: paga → licenza via email |

### Fase 2b — TUI (completata)

- ✅ TUI Textual a 4 schermate (Dashboard, Pipeline, Monitor, Strumenti)
- ✅ Compatibilità binario Nuitka (`__compiled__` detection)
- ✅ Aggiornamento automatico stato ogni 5 secondi

### Fase 3 — SaaS MVP

| Task | Stima |
|---|---|
| Backend API (FastAPI/Flask) | 2-3 settimane |
| Frontend web | 2-4 settimane |
| Auth + gestione utenti | 1-2 settimane |
| Database (PostgreSQL) | 1 settimana |
| Stripe subscription | 1-2 settimane |
| Storage dataset (S3) | 1 settimana |
| Hosting e infrastruttura | Continuativo |
| Beta pubblica SaaS | ~3-4 mesi dal via |

### Futuro a lungo termine

- **macOS build** — Nuitka supporta macOS, serve macchina o CI con runner macOS
- **Meccanismo di aggiornamento** — notifica al cliente quando esce una nuova versione
- **Installer** — pacchetto .deb / .rpm per Linux, .msi per Windows
- **API REST on-prem** — per integrare PurifyFactory in pipeline esistenti
- **Plugin system** — provider AI custom definibili dall'utente
- **Dashboard web per vendor** — gestione licenze, revoca, statistiche clienti

---

## 14. Stack tecnologico

### Runtime

| Componente | Tecnologia | Note |
|---|---|---|
| Linguaggio | Python 3.10+ | Type hints ovunque |
| Logging | loguru | TRACE level custom, log rotation |
| Serializzazione | orjson | JSON veloce con supporto tipi Python |
| Validazione config | pydantic v2 | Schema immutabile post-load |
| File locking | portalocker | Cross-platform, process-safe |
| Crittografia | cryptography (PyCA) | RSA 4096-bit per licenze |
| HTTP client | requests | Validazione licenza online |
| TUI | textual ≥ 0.47 | Opzionale (`pip install .[tui]`) |

### Provider AI

| Provider | SDK |
|---|---|
| Anthropic | `anthropic` SDK ufficiale |
| OpenAI | `openai` SDK ufficiale |
| Google Gemini | `google-genai ≥ 1.0.0` |
| Local | Chiamate HTTP dirette (compatibile OpenAI API) |

### Build e distribuzione

| Componente | Tecnologia |
|---|---|
| Compilazione | Nuitka 4.0+ |
| Compilatore C | GCC 13 (Linux) / MSVC (Windows) |
| Ottimizzazione | LTO (Link Time Optimization) |
| Packaging | tar.gz (Linux) / zip (Windows, in sviluppo) |
| Hash integrità | SHA-256 |

### Sviluppo

| Componente | Tecnologia |
|---|---|
| Test | pytest + pytest-cov |
| Linting | pyflakes |
| Versionamento | Git (GitHub) |
| Repository | mentoratechnologies/Purify-Factory_v9_1_6 (privato) |
| Release beta | mentoratechnologies/PurifyFactory-Beta (pubblico) |

---

## 15. Struttura del progetto

```
PurifyFactory_v9.1.6/
│
├── purifyfactory/                  ← Pacchetto principale (48 file, ~15.400 righe)
│   ├── config/                     ← Schema Pydantic, loader, defaults per tier
│   ├── core/                       ← Pipeline: splitter, orchestrator, worker,
│   │                               │   manager, supervisor, recovery, telemetry
│   ├── enforcement/                ← Tier gate, limits, runtime guard, usage meter,
│   │                               │   beta usage tracking
│   ├── licensing/                  ← RSA crypto, hardware fingerprint, validator,
│   │                               │   client validazione online
│   ├── providers/                  ← Anthropic, OpenAI, Gemini, Local (Ollama/vLLM)
│   ├── protection/                 ← Key vault, obfuscation XOR, integrity check SHA-256
│   ├── saas/                       ← Deployment detection, upload guard, job manager
│   ├── tui/                        ← TUI interattiva (Textual)
│   │   ├── app.py                  ← App principale, navigazione, polling stato
│   │   ├── runner/                 ← SubprocessRunner asincrono
│   │   ├── screens/                ← Dashboard, Pipeline, Monitor, Strumenti
│   │   ├── state/                  ← PipelineState, StatePoller (thread)
│   │   ├── widgets/                ← LogPanel, ConfirmModal, PhaseCard
│   │   └── styles/app.tcss         ← Tema scuro industriale
│   ├── utils/                      ← Atomic I/O, PathManager, cleanup
│   ├── _shutdown.py                ← Shutdown centralizzato (threading.Event)
│   ├── _version.py                 ← Single source of truth versione
│   ├── log_setup.py                ← Configurazione loguru + PII sanitization
│   └── __main__.py                 ← CLI (17 comandi + dispatch Nuitka)
│
├── tests/                          ← Test suite (124 test)
│   ├── unit/                       ← 84 test unitari
│   ├── integration/                ← 17 test di integrazione
│   └── test_e2e_beta_flow.py       ← 23 test e2e beta
│
├── tools/
│   └── license_tool.py             ← Generazione/verifica/revoca licenze (SOLO vendor)
│
├── build/
│   ├── nuitka_build.py             ← Script compilazione Nuitka (Linux + Windows)
│   ├── preflight_check.py          ← Verifica pre-build
│   ├── assemble_beta_package.py    ← Assemblaggio .tar.gz con manifesto SHA-256
│   ├── BUILD_GUIDE.md              ← Guida al build
│   └── BETA_LICENSE_WORKFLOW.md    ← Flusso operativo onboarding betatester
│
├── docs/
│   ├── GUIDA_UTENTE.md             ← Guida utente completa (italiano)
│   ├── config_example.json         ← Configurazione di esempio
│   └── EULA.md                     ← Contratto licenza utente finale (bozza)
│
├── beta/                           ← Materiale per betatester
│   ├── NDA_BETATESTER.md           ← Accordo non divulgazione (IT + EN)
│   ├── QUICKSTART.md               ← Avvio rapido per betatester
│   └── examples/                   ← Dataset di esempio
│
├── keys/                           ← Chiavi RSA private (MAI committare, MAI distribuire)
│
├── CLAUDE.md                       ← Istruzioni per Claude Code
├── CONFIG.md                       ← Riferimento configurazione completo
├── INSTALL.md                      ← Guida installazione
├── README.md                       ← Panoramica progetto
├── pyproject.toml                  ← Packaging moderno (setuptools)
└── setup.py                        ← Legacy packaging
```

---

*Documento generato il 16 marzo 2026. Soggetto ad aggiornamenti con l'evoluzione del prodotto.*

*© 2025-2026 Mentora Technologies — Tutti i diritti riservati.*
