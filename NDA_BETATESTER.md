# ACCORDO DI PARTECIPAZIONE AL PROGRAMMA BETA E DI RISERVATEZZA
# BETA PROGRAM PARTICIPATION AND NON-DISCLOSURE AGREEMENT

**PurifyFactory v9.1.6 — Mentora Technologies**

> **Nota per il Titolare / Note for the Titolare:**
> Compilare i campi `[NOME COGNOME]` con i propri dati prima di inviare il documento al Betatester.
> Fill in `[NOME COGNOME]` fields with your own details before sending this document to the Beta Tester.
>
> In caso di discrepanza tra la versione italiana e quella inglese, prevale la versione italiana.
> In case of discrepancy between the Italian and English versions, the Italian version shall prevail.

---

## PARTE I — VERSIONE ITALIANA

---

### ACCORDO DI PARTECIPAZIONE AL PROGRAMMA BETA E DI RISERVATEZZA

**Tra:**

**[NOME COGNOME]** (di seguito «il Titolare»), persona fisica residente in Grottaferrata (RM), Italia, operante sotto il nome commerciale **Mentora Technologies**, raggiungibile all'indirizzo email: a.lattanzi@mentoratechnologies.com

**e**

**[NOME COGNOME DEL BETATESTER]**, persona fisica i cui dati identificativi completi sono indicati nella sezione Firme del presente documento (di seguito «il Betatester»).

Il presente documento («Accordo») disciplina i termini e le condizioni di partecipazione al programma beta di PurifyFactory v9.1.6. Sottoscrivendo il presente Accordo, il Betatester dichiara di averne letto, compreso e accettato integralmente il contenuto.

---

### Articolo 1 — Definizioni

Nel presente Accordo si intende per:

- **«Software»**: PurifyFactory v9.1.6, pipeline industriale per la pulizia e normalizzazione di testi tramite intelligenza artificiale, di proprietà esclusiva del Titolare, distribuita al Betatester in formato binario compilato.
- **«Binario»**: il file eseguibile compilato di PurifyFactory v9.1.6, non modificabile né decompilabile, distribuito in licenza esclusivamente al Betatester per finalità di beta testing.
- **«Licenza»**: file `license.json` rilasciato dal Titolare al Betatester, firmato crittograficamente con chiave RSA 4096-bit, vincolato all'hardware del Betatester tramite impronta hardware univoca.
- **«Log»**: file di registro prodotti dal Software durante l'elaborazione, esportabili tramite il comando `export-debug`.
- **«Log Anonimizzati»**: Log dai quali sono stati rimossi automaticamente dal Software tutti i dati sensibili identificati all'Art. 10.3 prima dell'esportazione.
- **«Dataset di Input»**: file di dati testuali forniti dal Betatester al Software come materiale da elaborare.
- **«Dataset di Output»**: file di dati prodotti dal Software come risultato dell'elaborazione del Dataset di Input.
- **«API Key»**: chiave di autenticazione personale del Betatester, rilasciata da Provider AI terzi, necessaria per l'utilizzo dei servizi di intelligenza artificiale.
- **«Provider AI»**: servizi di intelligenza artificiale di terze parti (es. OpenAI, Anthropic, Google Gemini) utilizzati dal Software per l'elaborazione del testo tramite le API Key del Betatester.
- **«Informazioni Riservate»**: qualsiasi informazione relativa al Software, al suo funzionamento interno, al suo codice sorgente, agli algoritmi, al sistema di licensing, ai risultati del testing e a qualsiasi aspetto tecnico o commerciale non accessibile pubblicamente.
- **«Feedback»**: qualsiasi segnalazione di bug, suggerimento, idea, proposta o contributo di qualsiasi natura fornito dal Betatester al Titolare nell'ambito del programma beta.

---

### Articolo 2 — Oggetto dell'Accordo

Il presente Accordo regola:

a) la concessione al Betatester di una licenza limitata, personale, non esclusiva e non trasferibile per l'utilizzo del Software a fini di valutazione e testing;
b) gli obblighi di riservatezza del Betatester nei confronti del Titolare;
c) i termini di gestione, condivisione e conservazione dei dati prodotti durante il beta testing;
d) i diritti di proprietà intellettuale e la titolarità del Feedback;
e) le condizioni di utilizzo, i costi e le responsabilità.

---

### Articolo 3 — Descrizione del Software

PurifyFactory v9.1.6 è una pipeline industriale per la pulizia e normalizzazione di testi tramite modelli di linguaggio di grandi dimensioni (LLM). Il Software consente di:

- elaborare dataset testuali in formato JSONL tramite Provider AI configurabili dall'utente;
- supportare molteplici Provider AI (OpenAI, Anthropic/Claude, Google Gemini, modelli locali);
- gestire l'elaborazione in parallelo con suddivisione automatica del dataset in blocchi (batch);
- monitorare lo stato dell'elaborazione in tempo reale tramite strumenti CLI dedicati;
- riprendere automaticamente l'elaborazione in caso di interruzione.

**Modalità di elaborazione dei dati**: Il Software opera localmente sulla macchina del Betatester. I testi da elaborare vengono inviati direttamente ai Provider AI remoti tramite le API Key personali del Betatester. **Il Titolare non riceve né accede ai dati in nessuna fase di questo processo automatizzato.** Il Betatester può tuttavia scegliere, autonomamente e dopo l'elaborazione, di condividere volontariamente i propri dataset con il Titolare per finalità di certificazione, secondo quanto previsto all'Art. 12. I dati elaborati sono soggetti alle privacy policy e ai termini di servizio dei rispettivi Provider AI scelti dal Betatester.

---

### Articolo 4 — Licenza di Beta Testing

**4.1 Concessione della licenza.** Il Titolare concede al Betatester una licenza personale, non esclusiva, non trasferibile e revocabile per utilizzare il Software esclusivamente per le seguenti finalità:

a) valutazione delle funzionalità del Software su dati interni del Betatester;
b) identificazione e segnalazione di bug, anomalie e difetti al Titolare;
c) fornitura di Feedback funzionale al miglioramento del Software.

**4.2 Usi non consentiti.** Sono espressamente vietati, senza possibilità di deroga:

a) qualsiasi utilizzo del Software per finalità produttive, commerciali o di servizio a terzi;
b) la cessione, sublicenza, prestito, noleggio o qualsiasi altra forma di trasferimento del Binario a soggetti terzi, compresi colleghi o dipendenti, in assenza di accordo scritto separato con il Titolare;
c) il decompiling, il disassembling, il reverse engineering, l'emulazione o qualsiasi altro tentativo di ricostruire o derivare il codice sorgente del Software;
d) la manomissione del Binario, dei file di licenza, dei meccanismi di protezione o di qualsiasi altro componente del Software;
e) la rimozione, l'alterazione o l'oscuramento di qualsiasi avviso di copyright, marchio o identificativo di proprietà del Titolare;
f) la pubblicazione, la divulgazione o la diffusione di benchmark, metriche di performance, confronti con prodotti concorrenti o qualsiasi risultato derivante dal testing, senza il preventivo consenso scritto del Titolare.

---

### Articolo 5 — Gestione della Licenza

**5.1 Durata.** La Licenza ha validità di **90 (novanta) giorni** a decorrere dalla data di emissione indicata nel file `license.json`. La Licenza non è prorogabile né rinnovabile.

**5.2 Vincolo hardware.** La Licenza è vincolata all'impronta hardware della macchina del Betatester al momento dell'emissione, ottenuta tramite identificatori univoci del sistema (CPU, piattaforma, architettura). In caso di sostituzione o modifica rilevante dell'hardware, il Software cesserà di funzionare. **Il Titolare non è tenuto a riemettere la Licenza** per variazioni hardware intervenute durante il periodo di validità.

**5.3 Validazione offline.** La Licenza viene validata localmente ad ogni avvio del Software tramite verifica crittografica della firma RSA e dell'impronta hardware. Non è richiesta connessione a internet per la validazione della Licenza.

**5.4 Scadenza automatica.** Alla scadenza dei 90 giorni il Software cessa automaticamente di funzionare. Il Betatester è tenuto a eliminare definitivamente il Binario e tutti i file correlati entro **7 (sette) giorni** dalla scadenza della Licenza.

**5.5 Revoca anticipata.** Il Titolare si riserva il diritto di revocare la Licenza con effetto immediato in caso di violazione di qualsiasi disposizione del presente Accordo.

---

### Articolo 6 — API Key e Costi di Utilizzo

**6.1** Il Software richiede l'utilizzo di API Key personali dei Provider AI. Il Betatester è tenuto a procurarsi autonomamente le proprie API Key presso i rispettivi Provider AI.

**6.2** Tutti i costi derivanti dall'utilizzo delle API dei Provider AI — inclusi, a titolo esemplificativo, costi per token elaborati, abbonamenti, costi di banda e qualsiasi altro onere — sono **esclusivamente e integralmente a carico del Betatester**. Il Titolare non è parte del rapporto commerciale tra il Betatester e i Provider AI.

**6.3** Il Titolare non fornisce, in nessuna circostanza e a nessun titolo, rimborsi, crediti, compensi monetari o in natura relativi ai costi API sostenuti dal Betatester nell'ambito del programma beta.

**6.4** Il Betatester è responsabile del rispetto dei termini di servizio e delle policy d'uso dei Provider AI utilizzati. Il Titolare non è responsabile di eventuali sospensioni o limitazioni degli account API del Betatester.

---

### Articolo 7 — Riservatezza

**7.1** Il Betatester si impegna a mantenere strettamente riservate tutte le Informazioni Riservate e a non divulgarle, trasmetterle o renderle accessibili a terzi, in qualsiasi forma, senza il preventivo consenso scritto del Titolare.

**7.2** L'obbligo di riservatezza comprende, in via esemplificativa e non esaustiva:

a) l'architettura interna, le funzionalità e il funzionamento del Software;
b) il sistema di licensing, protezione e sicurezza del Software;
c) qualsiasi vulnerabilità, difetto o limitazione riscontrata durante il testing;
d) i risultati dell'elaborazione e le metriche di performance;
e) qualsiasi informazione tecnica o commerciale appresa durante il programma beta.

**7.3 Eccezioni.** L'obbligo di riservatezza non si applica alle informazioni che:

a) siano di dominio pubblico al momento della comunicazione, o lo diventino successivamente per cause non imputabili al Betatester;
b) fossero già in possesso documentabile del Betatester prima della firma del presente Accordo;
c) siano state legittimamente ricevute da terzi non vincolati da obblighi di riservatezza;
d) debbano essere divulgate per obbligo di legge o per ordine dell'autorità giudiziaria competente, previa tempestiva notifica al Titolare ove consentito dalla legge.

**7.4 Durata.** L'obbligo di riservatezza rimane in vigore per **5 (cinque) anni** dalla scadenza o dalla risoluzione anticipata del presente Accordo.

---

### Articolo 8 — Divulgazione Responsabile delle Vulnerabilità

**8.1** Nel caso in cui il Betatester scopra o ragionevolmente sospetti l'esistenza di una vulnerabilità di sicurezza nel Software, è tenuto a:

a) notificare il Titolare entro **7 (sette) giorni** dalla scoperta, all'indirizzo a.lattanzi@mentoratechnologies.com con oggetto «[SECURITY] Vulnerabilità — PurifyFactory v9.1.6»;
b) fornire una descrizione dettagliata della vulnerabilità, comprensiva delle modalità di riproduzione e dell'impatto stimato;
c) mantenere riservata la vulnerabilità per un periodo di **15 (quindici) giorni** dalla data di notifica al Titolare, per consentire la correzione.

**8.2** Trascorsi i 15 giorni dalla notifica senza che il Titolare abbia rilasciato una correzione, il Betatester è libero di divulgare la vulnerabilità pubblicamente, previa ulteriore comunicazione al Titolare.

**8.3** Il Betatester non può in alcun caso sfruttare le vulnerabilità scoperte per finalità proprie o per conto di terzi.

---

### Articolo 9 — Proprietà Intellettuale e Feedback

**9.1 Proprietà del Software.** PurifyFactory v9.1.6, inclusi il Binario, il codice sorgente, la documentazione, il sistema di licensing, gli algoritmi, i modelli di dati e tutti i componenti, sono e rimarranno di **esclusiva proprietà del Titolare**. Il presente Accordo non trasferisce al Betatester alcun diritto di proprietà intellettuale sul Software, né su qualsiasi suo componente.

**9.2 Proprietà del Feedback.** Qualsiasi Feedback fornito dal Betatester al Titolare durante o dopo il programma beta diventa automaticamente e irrevocabilmente **proprietà esclusiva del Titolare**, senza obbligo di compenso, riconoscimento o notifica al Betatester. Il Betatester rinuncia esplicitamente a qualsiasi rivendicazione di diritti di proprietà intellettuale, diritti morali, rimborsi o compensi di qualsiasi natura relativi al Feedback fornito.

**9.3** Il Titolare è libero di incorporare, modificare, distribuire, commercializzare o non utilizzare il Feedback a propria esclusiva discrezione.

---

### Articolo 10 — Trattamento dei Dati Personali

**10.1 Titolare del Trattamento.** Ai sensi del Regolamento (UE) 2016/679 («GDPR»), il Titolare del trattamento è **[NOME COGNOME]**, persona fisica residente in Grottaferrata (RM), Italia, raggiungibile all'indirizzo a.lattanzi@mentoratechnologies.com.

**10.2 Dati raccolti.** Il Titolare tratta esclusivamente i dati volontariamente trasmessi dal Betatester nell'ambito del programma beta:

- dati identificativi forniti in fase di sottoscrizione del presente Accordo (nome, cognome, email);
- Log Anonimizzati esportati tramite il comando `export-debug`, come descritti all'Art. 10.3;
- Dataset di Input e/o Output, esclusivamente se il Betatester sceglie di condividerli ai sensi dell'Art. 12.

**10.3 Anonimizzazione automatica dei Log.** Il comando `export-debug` rimuove automaticamente dalle righe di log le seguenti categorie di dati prima di qualsiasi esportazione:

| Categoria | Esempio originale | Risultato dopo anonimizzazione |
|---|---|---|
| API Key | `sk-abc123...` | `sk-***REDACTED***` |
| Indirizzi email | `utente@dominio.com` | `***EMAIL***` |
| Percorsi utente (Linux/macOS) | `/home/utente/...` | `/home/***USER***/...` |
| Percorsi utente (Windows) | `C:\Users\utente\...` | `C:\Users\***USER***\...` |
| Impronte hardware | `a7f3b8c9d4e2...` | `***FINGERPRINT***` |
| Chiavi di licenza | `license_key: ABC...` | `license_key: ***REDACTED***` |
| Bearer token | `Bearer eyJ...` | `Bearer ***REDACTED***` |

**10.4 Finalità e base giuridica.** Il trattamento è effettuato per le seguenti finalità:

- sviluppo, debugging e miglioramento del Software (base giuridica: esecuzione del contratto, Art. 6(1)(b) GDPR);
- certificazione TRL5 e documentazione tecnica (base giuridica: legittimo interesse del Titolare, Art. 6(1)(f) GDPR).

**10.5 Conservazione dei dati.**

| Tipologia di dato | Periodo di conservazione |
|---|---|
| Dati identificativi del Betatester | Durata del programma beta + 5 anni |
| Log Anonimizzati (senza dati personali residui) | Tempo indeterminato, per finalità di sviluppo |
| Log contenenti dati personali residui | Eliminati entro 12 mesi dalla ricezione |
| Dataset di Input | Eliminati entro 12 mesi dalla ricezione |
| Dataset di Output | Eliminati entro 12 mesi dalla ricezione |

In ogni caso, su richiesta esplicita del Betatester, i dati personali saranno eliminati entro 30 giorni dalla richiesta, compatibilmente con eventuali obblighi di legge.

**10.6 Diritti dell'interessato.** Il Betatester ha diritto di accesso, rettifica, cancellazione («diritto all'oblio»), limitazione del trattamento, portabilità dei dati e opposizione al trattamento, da esercitare scrivendo a a.lattanzi@mentoratechnologies.com. Ha inoltre il diritto di proporre reclamo all'Autorità Garante per la protezione dei dati personali (www.garanteprivacy.it).

**10.7 Trasferimento a terzi.** I dati non vengono ceduti, venduti o comunicati a soggetti terzi, salvo che per obbligo di legge o ordine dell'autorità giudiziaria.

---

### Articolo 11 — Canali di Comunicazione Ufficiali

**11.1** I canali ufficiali di comunicazione tra il Betatester e il Titolare sono i seguenti:

| Finalità | Canale e modalità |
|---|---|
| Supporto tecnico e segnalazione bug | a.lattanzi@mentoratechnologies.com |
| Invio Log anonimizzati (`export-debug`) | a.lattanzi@mentoratechnologies.com — allegare il file ZIP prodotto dal comando |
| Invio Feedback strutturato | a.lattanzi@mentoratechnologies.com — allegare `FEEDBACK_TEMPLATE.md` compilato |
| Condivisione volontaria Dataset | a.lattanzi@mentoratechnologies.com |
| Segnalazione vulnerabilità di sicurezza | a.lattanzi@mentoratechnologies.com — oggetto: [SECURITY] |
| Comunicazioni legali | a.lattanzi@mentoratechnologies.com |

**11.2** Per tutte le comunicazioni, il Betatester deve indicare nell'oggetto dell'email: «Beta v9.1.6 — [Cognome del Betatester]».

**11.3** Il Betatester è responsabile di mantenere aggiornato il proprio indirizzo email e di verificare regolarmente la propria casella di posta. Comunicazioni inviate all'indirizzo email indicato in firma si intendono ricevute.

**11.4 Avviso di sicurezza.** Il Titolare non richiederà mai al Betatester API Key, password, credenziali di accesso o file di licenza tramite email o qualsiasi altro canale non concordato. Qualsiasi richiesta in tal senso deve essere considerata fraudolenta e segnalata immediatamente.

---

### Articolo 12 — Condivisione Volontaria dei Dati per la Certificazione

**12.1** Per le finalità di certificazione TRL5 e di miglioramento del Software, al Betatester è richiesto di condividere i seguenti materiali:

| Materiale | Stato | Note |
|---|---|---|
| Log anonimizzati (`export-debug`) | **Obbligatorio** | Generati tramite il comando dedicato; nessun dato sensibile incluso |
| Feedback compilato (`FEEDBACK_TEMPLATE.md`) | **Obbligatorio** | Questionario strutturato allegato alla documentazione |
| Dataset di Input | **Facoltativo** | Il Betatester sceglie liberamente se condividerlo |
| Dataset di Output | **Facoltativo** | Il Betatester sceglie liberamente se condividerlo |

**12.2** La condivisione dei Dataset di Input e/o Output è **completamente facoltativa**. Il Betatester può scegliere di non condividerli senza alcuna conseguenza sulla validità del presente Accordo o sull'accesso al Software.

**12.3** Il Betatester che sceglie di condividere i Dataset è consapevole che questi potrebbero contenere dati personali o riservati e ne autorizza il trattamento ai sensi dell'Art. 10, con conservazione massima di 12 mesi.

**12.4** Il Betatester è l'unico responsabile di verificare che i Dataset condivisi non contengano dati personali di terzi per cui non dispone delle necessarie autorizzazioni al trattamento.

**12.5 Requisiti minimi del Dataset per la certificazione TRL5.** Al fine di produrre dati utilizzabili ai fini della certificazione TRL5, il Dataset elaborato durante il programma beta deve rispettare le seguenti soglie minime:

| Dimensione del Dataset | Classificazione |
|---|---|
| Inferiore a 100 record | Non utilizzabile per la certificazione — solo apprendimento dello strumento |
| Da 100 a 999 record | Insufficiente — dati indicativi, non certificabili |
| Da 1.000 a 4.999 record | Soglia minima accettata per la certificazione TRL5 |
| 5.000 record o più | Consigliato per validazione completa della pipeline |

Il Betatester è invitato a elaborare dataset di dimensioni adeguate, preferibilmente composti da dati reali del proprio dominio applicativo. Il Software segnala automaticamente con un avviso (`WARNING`) qualora il dataset sia al di sotto della soglia minima. Il mancato rispetto delle soglie non invalida il presente Accordo, ma rende i risultati non certificabili ai fini TRL5.

---

### Articolo 13 — Esclusione di Garanzie

**13.1** Il Software è fornito nella sua configurazione di versione beta, «così come è» («as is»), **senza alcuna garanzia, espressa o implicita**.

**13.2** Il Titolare esclude espressamente qualsiasi garanzia relativa a:

a) l'assenza di difetti, errori, interruzioni o comportamenti inattesi nel Software;
b) l'idoneità del Software per uno scopo specifico del Betatester;
c) la continuità, affidabilità o disponibilità del Software;
d) la correttezza, completezza o adeguatezza dei risultati dell'elaborazione;
e) l'assenza di perdita di dati, danni al sistema o interruzioni operative derivanti dall'utilizzo del Software.

**13.3** Il Betatester riconosce esplicitamente di star utilizzando software in fase di sviluppo e testing, non destinato a uso produttivo, e accetta consapevolmente tutti i rischi connessi.

---

### Articolo 14 — Limitazione di Responsabilità

**14.1 Il Titolare non è responsabile, in nessuna misura e a nessun titolo, per qualsiasi danno diretto, indiretto, incidentale, speciale, consequenziale o punitivo** derivante dall'utilizzo, dall'impossibilità di utilizzo o dai risultati del Software, inclusi, in via esemplificativa: perdita di dati, perdita di profitti, interruzione dell'attività, danni al sistema hardware o software, costi API non previsti.

**14.2** Il Betatester si assume il **pieno e totale rischio** derivante dall'installazione, dalla configurazione, dall'utilizzo e dai risultati del Software. L'utilizzo del Software avviene interamente sotto la responsabilità esclusiva del Betatester.

**14.3** Il Betatester manleva e tiene indenne il Titolare da qualsiasi pretesa, azione legale, richiesta di risarcimento o spesa (incluse le spese legali) avanzate da terzi in connessione con l'utilizzo del Software da parte del Betatester.

---

### Articolo 15 — Durata e Risoluzione

**15.1** Il presente Accordo entra in vigore alla data di sottoscrizione di entrambe le parti e rimane valido fino alla scadenza della Licenza (90 giorni), salvo risoluzione anticipata.

**15.2 Risoluzione da parte del Titolare.** Il Titolare può risolvere anticipatamente il presente Accordo con effetto immediato, mediante comunicazione scritta, in caso di:

a) violazione di qualsiasi disposizione del presente Accordo;
b) utilizzo del Software per finalità non consentite dall'Art. 4;
c) diffusione non autorizzata del Binario o delle Informazioni Riservate.

**15.3 Recesso del Betatester.** Il Betatester può recedere dal presente Accordo in qualsiasi momento inviando comunicazione scritta al Titolare.

**15.4 Obblighi post-scadenza.** Alla scadenza o risoluzione del presente Accordo, il Betatester è tenuto a:

a) cessare immediatamente ogni utilizzo del Software;
b) eliminare definitivamente il Binario, il file di Licenza e tutti i file correlati dalla propria macchina e da qualsiasi supporto di archiviazione o backup;
c) confermare per iscritto al Titolare l'avvenuta eliminazione entro **7 (sette) giorni**.

**15.5 Sopravvivenza.** Le disposizioni relative a riservatezza (Art. 7), proprietà intellettuale (Art. 9), trattamento dei dati (Art. 10) e limitazione di responsabilità (Art. 14) sopravvivono alla scadenza o alla risoluzione del presente Accordo.

---

### Articolo 16 — Legge Applicabile e Foro Competente

**16.1** Il presente Accordo è regolato dalla legge italiana.

**16.2** Per qualsiasi controversia relativa all'interpretazione, all'esecuzione o alla risoluzione del presente Accordo, le parti eleggono come **foro competente esclusivo il Tribunale Ordinario di Roma**.

**16.3** Prima di adire l'autorità giudiziaria, le parti si impegnano a tentare una risoluzione amichevole della controversia. La parte che intende avviare una procedura giudiziale è tenuta a notificare previamente per iscritto il proprio disaccordo, concedendo all'altra parte un termine di **30 (trenta) giorni** per raggiungere una soluzione concordata.

---

### Articolo 17 — Disposizioni Generali

**17.1 Integralità dell'Accordo.** Il presente Accordo costituisce l'intero accordo tra le parti in relazione all'oggetto del medesimo e sostituisce integralmente qualsiasi accordo, intesa o comunicazione precedente, scritta o verbale.

**17.2 Modifiche.** Qualsiasi modifica al presente Accordo deve essere effettuata per iscritto e firmata da entrambe le parti. Modifiche unilaterali non producono effetti.

**17.3 Nullità parziale.** L'eventuale nullità, inefficacia o inapplicabilità di una singola disposizione non pregiudica la validità delle restanti disposizioni, che rimarranno pienamente efficaci.

**17.4 Rinuncia.** La mancata o tardiva esecuzione di qualsiasi disposizione del presente Accordo da parte del Titolare non costituisce rinuncia al diritto di farla valere in futuro.

**17.5 Cessione.** Il Betatester non può cedere, trasferire o delegare i propri diritti o obblighi derivanti dal presente Accordo senza il preventivo consenso scritto del Titolare.

**17.6 Comunicazioni.** Tutte le comunicazioni formali previste dal presente Accordo devono essere effettuate per iscritto (email con ricevuta di lettura o raccomandata A/R).

---

### FIRME

Il presente Accordo è sottoscritto dalle parti, che dichiarano di averne letto e compreso integralmente il contenuto.

---

**IL TITOLARE**

Nome e Cognome: ___________________________________________________
(operante sotto il nome commerciale Mentora Technologies)

Luogo e Data: Grottaferrata (RM), ___________________________________

Firma: ____________________________________________________________

---

**IL BETATESTER**

Nome e Cognome: ___________________________________________________

Data di nascita: ___________________________________________________

Luogo di residenza: ________________________________________________

Codice Fiscale: ____________________________________________________

Indirizzo email: ___________________________________________________

Luogo e Data: _____________________________________________________

Firma: ____________________________________________________________

---
---

## PART II — ENGLISH VERSION

---

### BETA PROGRAM PARTICIPATION AND NON-DISCLOSURE AGREEMENT

**Between:**

**[FIRST NAME LAST NAME]** (hereinafter «the Owner»), a natural person residing in Grottaferrata (RM), Italy, operating under the trade name **Mentora Technologies**, reachable at: a.lattanzi@mentoratechnologies.com

**and**

**[BETA TESTER FULL NAME]**, a natural person whose complete identification details are provided in the Signatures section of this document (hereinafter «the Beta Tester»).

This document («Agreement») governs the terms and conditions for participation in the PurifyFactory v9.1.6 beta program. By signing this Agreement, the Beta Tester declares to have read, understood, and fully accepted its content.

---

### Article 1 — Definitions

In this Agreement, the following definitions apply:

- **«Software»**: PurifyFactory v9.1.6, an industrial-grade text cleaning and normalization pipeline powered by artificial intelligence, exclusively owned by the Owner, distributed to the Beta Tester in compiled binary format.
- **«Binary»**: the compiled, non-modifiable executable file of PurifyFactory v9.1.6, distributed under license exclusively to the Beta Tester for beta testing purposes.
- **«License»**: the `license.json` file issued by the Owner to the Beta Tester, cryptographically signed with a 4096-bit RSA key, bound to the Beta Tester's hardware via a unique hardware fingerprint.
- **«Logs»**: log files generated by the Software during processing, exportable via the `export-debug` command.
- **«Anonymized Logs»**: Logs from which all sensitive data (as identified in Art. 10.3) has been automatically removed by the Software prior to export.
- **«Input Dataset»**: text data files provided by the Beta Tester to the Software for processing.
- **«Output Dataset»**: data files produced by the Software as the result of processing the Input Dataset.
- **«API Key»**: a personal authentication key obtained by the Beta Tester from third-party AI Providers, required to use AI processing services.
- **«AI Provider»**: third-party artificial intelligence services (e.g., OpenAI, Anthropic, Google Gemini) used by the Software to process text via the Beta Tester's API Keys.
- **«Confidential Information»**: any information relating to the Software, its internal workings, source code, algorithms, licensing system, testing results, and any technical or commercial aspects not publicly available.
- **«Feedback»**: any bug report, suggestion, idea, proposal, or contribution of any nature provided by the Beta Tester to the Owner during the beta program.

---

### Article 2 — Subject Matter

This Agreement governs:

a) the grant to the Beta Tester of a limited, personal, non-exclusive, non-transferable license to use the Software for evaluation and testing purposes;
b) the Beta Tester's confidentiality obligations toward the Owner;
c) the terms for managing, sharing, and retaining data produced during beta testing;
d) intellectual property rights and ownership of Feedback;
e) conditions of use, costs, and liabilities.

---

### Article 3 — Description of the Software

PurifyFactory v9.1.6 is an industrial-grade pipeline for text cleaning and normalization using large language models (LLMs). The Software enables:

- processing of text datasets in JSONL format via user-configurable AI Providers;
- support for multiple AI Providers (OpenAI, Anthropic/Claude, Google Gemini, local models);
- parallel processing with automatic dataset segmentation into batches;
- real-time monitoring of processing progress via dedicated CLI tools;
- automatic resumption of processing after interruption.

**Data processing model**: The Software runs locally on the Beta Tester's machine. Text data to be processed is sent directly to remote AI Providers via the Beta Tester's personal API Keys. **The Owner does not receive or access data at any stage of this automated process.** The Beta Tester may however choose, independently and after processing is complete, to voluntarily share their datasets with the Owner for certification purposes, as provided in Art. 12. Processed data is subject to the privacy policies and terms of service of the respective AI Providers chosen by the Beta Tester.

---

### Article 4 — Beta Testing License

**4.1 License grant.** The Owner grants the Beta Tester a personal, non-exclusive, non-transferable, and revocable license to use the Software exclusively for the following purposes:

a) evaluating the Software's functionality using the Beta Tester's own internal data;
b) identifying and reporting bugs, anomalies, and defects to the Owner;
c) providing Feedback to support improvement of the Software.

**4.2 Prohibited uses.** The following are expressly and strictly prohibited:

a) any use of the Software for production, commercial, or third-party service purposes;
b) assignment, sublicensing, lending, renting, or any other form of transfer of the Binary to third parties, including colleagues or employees, absent a separate written agreement with the Owner;
c) decompiling, disassembling, reverse engineering, emulating, or any other attempt to reconstruct or derive the source code of the Software;
d) tampering with the Binary, license files, protection mechanisms, or any other component of the Software;
e) removing, altering, or obscuring any copyright notice, trademark, or ownership identifier of the Owner;
f) publishing, disclosing, or distributing benchmarks, performance metrics, competitor comparisons, or any results derived from testing without the Owner's prior written consent.

---

### Article 5 — License Management

**5.1 Duration.** The License is valid for **90 (ninety) days** from the issuance date stated in the `license.json` file. The License is not extendable or renewable under any circumstances.

**5.2 Hardware binding.** The License is bound to the hardware fingerprint of the Beta Tester's machine at the time of issuance, derived from unique system identifiers (CPU, platform, architecture). In the event of significant hardware replacement or modification, the Software will cease to function. **The Owner is not obligated to reissue the License** due to hardware changes occurring during the validity period.

**5.3 Offline validation.** The License is validated locally at each Software startup via cryptographic RSA signature verification and hardware fingerprint matching. No internet connection is required for License validation.

**5.4 Automatic expiration.** Upon expiration of the 90-day period, the Software automatically ceases to function. The Beta Tester is required to permanently delete the Binary and all related files within **7 (seven) days** of License expiration.

**5.5 Early revocation.** The Owner reserves the right to revoke the License with immediate effect in the event of any breach of this Agreement.

---

### Article 6 — API Keys and Usage Costs

**6.1** The Software requires the use of personal API Keys from AI Providers. The Beta Tester is solely responsible for obtaining their own API Keys from the respective AI Providers.

**6.2** All costs arising from the use of AI Provider APIs — including, without limitation, token processing costs, subscriptions, bandwidth costs, and any other charges — are **exclusively and entirely borne by the Beta Tester**. The Owner is not party to the commercial relationship between the Beta Tester and the AI Providers.

**6.3** The Owner provides no reimbursements, credits, monetary compensation, or compensation of any kind, under any circumstances or for any reason, in connection with API costs incurred by the Beta Tester during the beta program.

**6.4** The Beta Tester is responsible for compliance with the terms of service and acceptable use policies of any AI Provider used. The Owner bears no responsibility for any suspension or restriction of the Beta Tester's API accounts.

---

### Article 7 — Confidentiality

**7.1** The Beta Tester undertakes to keep strictly confidential all Confidential Information and not to disclose, transmit, or make accessible to any third party, in any form, without the Owner's prior written consent.

**7.2** The confidentiality obligation includes, by way of non-exhaustive example:

a) the internal architecture, features, and operation of the Software;
b) the licensing, protection, and security system of the Software;
c) any vulnerability, defect, or limitation identified during testing;
d) processing results and performance metrics;
e) any technical or commercial information learned during the beta program.

**7.3 Exceptions.** The confidentiality obligation does not apply to information that:

a) is in the public domain at the time of disclosure, or subsequently enters the public domain through no fault of the Beta Tester;
b) was already in the Beta Tester's documented possession prior to signing this Agreement;
c) was lawfully received from third parties not bound by confidentiality obligations;
d) is required to be disclosed by law or judicial order, provided the Owner is notified promptly where permitted by law.

**7.4 Duration.** The confidentiality obligation remains in force for **5 (five) years** following the expiration or early termination of this Agreement.

---

### Article 8 — Responsible Disclosure of Security Vulnerabilities

**8.1** Should the Beta Tester discover or reasonably suspect the existence of a security vulnerability in the Software, they are required to:

a) notify the Owner within **7 (seven) days** of discovery, at a.lattanzi@mentoratechnologies.com with subject line: «[SECURITY] Vulnerability — PurifyFactory v9.1.6»;
b) provide a detailed description of the vulnerability, including reproduction steps and estimated impact;
c) keep the vulnerability confidential for a period of **15 (fifteen) days** from the date of notification to the Owner, to allow time for remediation.

**8.2** After 15 days from notification, if the Owner has not released a fix, the Beta Tester is free to publicly disclose the vulnerability, subject to prior further notification to the Owner.

**8.3** The Beta Tester shall not exploit discovered vulnerabilities for personal gain or for the benefit of any third party.

---

### Article 9 — Intellectual Property and Feedback

**9.1 Software ownership.** PurifyFactory v9.1.6, including the Binary, source code, documentation, licensing system, algorithms, data models, and all components, is and shall remain the **exclusive property of the Owner**. This Agreement does not transfer to the Beta Tester any intellectual property rights in the Software or any of its components.

**9.2 Feedback ownership.** Any Feedback provided by the Beta Tester to the Owner during or after the beta program automatically and irrevocably becomes the **exclusive property of the Owner**, without any obligation of compensation, credit, or notification to the Beta Tester. The Beta Tester expressly waives any claim to intellectual property rights, moral rights, reimbursements, or compensation of any nature in connection with Feedback provided.

**9.3** The Owner is free to incorporate, modify, distribute, commercialize, or not use the Feedback at its sole discretion.

---

### Article 10 — Personal Data Processing

**10.1 Data Controller.** Pursuant to Regulation (EU) 2016/679 («GDPR»), the Data Controller is **[FIRST NAME LAST NAME]**, a natural person residing in Grottaferrata (RM), Italy, reachable at a.lattanzi@mentoratechnologies.com.

**10.2 Data collected.** The Owner processes only data voluntarily transmitted by the Beta Tester in connection with the beta program:

- identification data provided upon signing (name, surname, email address);
- Anonymized Logs exported via the `export-debug` command, as described in Art. 10.3;
- Input and/or Output Datasets, exclusively if the Beta Tester elects to share them pursuant to Art. 12.

**10.3 Automatic log anonymization.** The `export-debug` command automatically removes the following categories of data from log lines prior to any export:

| Category | Original example | After anonymization |
|---|---|---|
| API Keys | `sk-abc123...` | `sk-***REDACTED***` |
| Email addresses | `user@domain.com` | `***EMAIL***` |
| User file paths (Linux/macOS) | `/home/user/...` | `/home/***USER***/...` |
| User file paths (Windows) | `C:\Users\user\...` | `C:\Users\***USER***\...` |
| Hardware fingerprints | `a7f3b8c9d4e2...` | `***FINGERPRINT***` |
| License keys | `license_key: ABC...` | `license_key: ***REDACTED***` |
| Bearer tokens | `Bearer eyJ...` | `Bearer ***REDACTED***` |

**10.4 Purposes and legal basis.** Processing is carried out for the following purposes:

- Software development, debugging, and improvement (legal basis: performance of contract, Art. 6(1)(b) GDPR);
- TRL5 certification and technical documentation (legal basis: legitimate interest of the Owner, Art. 6(1)(f) GDPR).

**10.5 Data retention.**

| Data type | Retention period |
|---|---|
| Beta Tester identification data | Duration of beta program + 5 years |
| Anonymized Logs (no residual personal data) | Indefinite, for development purposes |
| Logs containing residual personal data | Deleted within 12 months of receipt |
| Input Dataset | Deleted within 12 months of receipt |
| Output Dataset | Deleted within 12 months of receipt |

In all cases, upon explicit request from the Beta Tester, personal data will be deleted within 30 days of the request, subject to any applicable legal obligations.

**10.6 Data subject rights.** The Beta Tester has the right to access, rectify, erase («right to be forgotten»), restrict processing, data portability, and object to processing, exercisable by writing to a.lattanzi@mentoratechnologies.com. The Beta Tester also has the right to lodge a complaint with the Italian Data Protection Authority (Garante per la protezione dei dati personali, www.garanteprivacy.it).

**10.7 Third-party transfers.** Data is not sold, assigned, or communicated to third parties, except as required by law or judicial order.

---

### Article 11 — Official Communication Channels

**11.1** The official communication channels between the Beta Tester and the Owner are as follows:

| Purpose | Channel and method |
|---|---|
| Technical support and bug reports | a.lattanzi@mentoratechnologies.com |
| Submission of Anonymized Logs (`export-debug`) | a.lattanzi@mentoratechnologies.com — attach the ZIP file generated by the command |
| Structured Feedback submission | a.lattanzi@mentoratechnologies.com — attach completed `FEEDBACK_TEMPLATE.md` |
| Voluntary Dataset submission | a.lattanzi@mentoratechnologies.com |
| Security vulnerability disclosure | a.lattanzi@mentoratechnologies.com — subject: [SECURITY] |
| Legal communications | a.lattanzi@mentoratechnologies.com |

**11.2** For all communications, the Beta Tester must include in the email subject line: «Beta v9.1.6 — [Beta Tester Surname]».

**11.3** The Beta Tester is responsible for keeping their email address up to date and checking their inbox regularly. Communications sent to the email address provided in the Signatures section are considered received.

**11.4 Security notice.** The Owner will never request API Keys, passwords, login credentials, or license files from the Beta Tester via email or any other channel. Any such request should be considered fraudulent and reported immediately.

---

### Article 12 — Voluntary Data Sharing for Certification

**12.1** For TRL5 certification and Software improvement purposes, the Beta Tester is requested to share the following materials:

| Material | Status | Notes |
|---|---|---|
| Anonymized Logs (`export-debug`) | **Required** | Generated via the dedicated command; no sensitive data included |
| Completed Feedback (`FEEDBACK_TEMPLATE.md`) | **Required** | Structured questionnaire included in documentation |
| Input Dataset | **Optional** | Beta Tester freely chooses whether to share |
| Output Dataset | **Optional** | Beta Tester freely chooses whether to share |

**12.2** Sharing of Input and/or Output Datasets is **entirely voluntary**. The Beta Tester may choose not to share them without any consequence to the validity of this Agreement or access to the Software.

**12.3** A Beta Tester who elects to share Datasets acknowledges that they may contain personal or confidential data and expressly authorizes their processing pursuant to Art. 10, with a maximum retention period of 12 months.

**12.4** The Beta Tester is solely responsible for verifying that any shared Datasets do not contain personal data of third parties for whom the Beta Tester does not hold the necessary processing authorizations.

**12.5 Minimum Dataset requirements for TRL5 certification.** In order to produce data usable for TRL5 certification purposes, the Dataset processed during the beta program must meet the following minimum thresholds:

| Dataset size | Classification |
|---|---|
| Fewer than 100 records | Not usable for certification — for tool learning only |
| 100 to 999 records | Insufficient — indicative data, not certifiable |
| 1,000 to 4,999 records | Minimum accepted threshold for TRL5 certification |
| 5,000 records or more | Recommended for full pipeline validation |

The Beta Tester is encouraged to process datasets of adequate size, preferably composed of real data from their own application domain. The Software automatically displays a `WARNING` message if the dataset falls below the minimum threshold. Failure to meet the thresholds does not invalidate this Agreement, but renders the results non-certifiable for TRL5 purposes.

---

### Article 13 — Disclaimer of Warranties

**13.1** The Software is provided in its beta configuration, «as is», **without any warranty, express or implied**.

**13.2** The Owner expressly disclaims any warranty regarding:

a) the absence of defects, errors, interruptions, or unexpected behavior in the Software;
b) the fitness of the Software for any particular purpose of the Beta Tester;
c) the continuity, reliability, or availability of the Software;
d) the accuracy, completeness, or adequacy of processing results;
e) the absence of data loss, system damage, or operational disruption resulting from use of the Software.

**13.3** The Beta Tester expressly acknowledges that this is software under development and testing, not intended for production use, and knowingly accepts all associated risks.

---

### Article 14 — Limitation of Liability

**14.1 The Owner shall not be liable, to any extent or under any theory, for any direct, indirect, incidental, special, consequential, or punitive damages** arising from the use of, inability to use, or results of the Software, including without limitation: data loss, loss of profits, business interruption, hardware or software system damage, unexpected API costs.

**14.2** The Beta Tester assumes the **full and entire risk** arising from the installation, configuration, use, and results of the Software. Use of the Software occurs entirely under the Beta Tester's sole responsibility.

**14.3** The Beta Tester shall indemnify and hold harmless the Owner from any claim, lawsuit, demand for damages, or expense (including reasonable legal fees) brought by third parties in connection with the Beta Tester's use of the Software.

---

### Article 15 — Term and Termination

**15.1** This Agreement enters into force on the date of signature by both parties and remains valid until expiration of the License (90 days), unless terminated earlier.

**15.2 Termination by the Owner.** The Owner may terminate this Agreement with immediate effect by written notice in the event of:

a) any breach of this Agreement by the Beta Tester;
b) use of the Software for purposes not permitted under Art. 4;
c) unauthorized distribution of the Binary or Confidential Information.

**15.3 Withdrawal by the Beta Tester.** The Beta Tester may withdraw from this Agreement at any time by sending written notice to the Owner.

**15.4 Post-termination obligations.** Upon expiration or termination of this Agreement, the Beta Tester is required to:

a) immediately cease all use of the Software;
b) permanently delete the Binary, the License file, and all related files from their machine and any storage media or backup;
c) confirm deletion in writing to the Owner within **7 (seven) days**.

**15.5 Survival.** The provisions relating to confidentiality (Art. 7), intellectual property (Art. 9), data processing (Art. 10), and limitation of liability (Art. 14) survive the expiration or termination of this Agreement.

---

### Article 16 — Governing Law and Jurisdiction

**16.1** This Agreement is governed by Italian law.

**16.2** For any dispute relating to the interpretation, performance, or termination of this Agreement, the parties elect as **exclusive competent court the Tribunale Ordinario di Roma** (Ordinary Court of Rome).

**16.3** Prior to initiating legal proceedings, the parties undertake to attempt an amicable resolution. The party intending to commence proceedings must first notify the other party in writing, granting a period of **30 (thirty) days** to reach an agreed solution.

---

### Article 17 — General Provisions

**17.1 Entire Agreement.** This Agreement constitutes the entire agreement between the parties with respect to its subject matter and supersedes all prior agreements, understandings, or communications, whether written or oral.

**17.2 Amendments.** Any amendment to this Agreement must be made in writing and signed by both parties. Unilateral amendments have no effect.

**17.3 Severability.** The invalidity, ineffectiveness, or unenforceability of any single provision shall not affect the validity of the remaining provisions, which shall remain in full force and effect.

**17.4 Waiver.** Failure or delay by the Owner to enforce any provision of this Agreement shall not constitute a waiver of the right to enforce it in the future.

**17.5 Assignment.** The Beta Tester may not assign, transfer, or delegate any rights or obligations under this Agreement without the Owner's prior written consent.

**17.6 Notices.** All formal notices required under this Agreement must be made in writing (email with read receipt or registered mail with acknowledgment of receipt).

---

### SIGNATURES

This Agreement is signed by the parties, who declare to have read and fully understood its content.

---

**THE OWNER**

Full Name: ___________________________________________________
(operating under the trade name Mentora Technologies)

Place and Date: Grottaferrata (RM), ________________________________

Signature: ____________________________________________________

---

**THE BETA TESTER**

Full Name: ____________________________________________________

Date of Birth: _________________________________________________

Place of Residence: ____________________________________________

Italian Tax Code (Codice Fiscale) / ID Number: _____________________

Email Address: ________________________________________________

Place and Date: ________________________________________________

Signature: ____________________________________________________

---

*In case of discrepancy between the Italian and English versions of this Agreement, the Italian version shall prevail.*

*In caso di discrepanza tra la versione italiana e quella inglese del presente Accordo, prevale la versione italiana.*
