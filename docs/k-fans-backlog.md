# K-FANS — Backlog iniziale da slide invito e abbonamento

Documento di sintesi derivato dalla presentazione `KFans_App_Mockup invito_abbonamento_260621_vRF.pptx`.

## Epic 1 — Workflow invito giocatore da Dynamix2

Obiettivo: consentire alla squadra/preparatore di censire un giocatore, richiedere il consenso e rendere visibili i dati biometrici solo dopo accettazione.

### Task
- [ ] Aggiungere email e telefono al profilo giocatore in Dynamix2
- [ ] Aggiungere flag/stato consenso per singolo giocatore
- [ ] Implementare azione `Ask for consent` su Dynamix2
- [ ] Generare credenziali temporanee per il giocatore
- [ ] Inviare email automatica con link app/store e credenziali temporanee
- [ ] Gestire caso giocatore già registrato in app con link diretto
- [ ] Bloccare la visibilità dei dati biometrici fino all'accettazione del consenso
- [ ] Mostrare stato `Necessary consent needed` sui dati non visibili
- [ ] Mostrare contatore consensi accettati/totali in homepage Dynamix2
- [ ] Sincronizzare consenso accettato dall'app verso Dynamix2

### Criteri di accettazione
- Un preparatore può richiedere il consenso per un giocatore censito.
- Il giocatore riceve una comunicazione con istruzioni di accesso.
- I dati biometrici restano nascosti finché il consenso non viene accettato.
- Dynamix2 mostra chiaramente lo stato del consenso.

---

## Epic 2 — Onboarding giocatore su app K-FANS

Obiettivo: permettere al giocatore di accedere all'app, verificare la propria identità e accettare il consenso richiesto dalla squadra.

### Task
- [ ] Implementare login con credenziali temporanee
- [ ] Implementare cambio password obbligatorio al primo accesso
- [ ] Mostrare banner di richiesta consenso per la squadra richiedente
- [ ] Implementare accettazione consenso trattamento dati
- [ ] Implementare OTP via SMS
- [ ] Implementare raccolta selfie per verifica identità
- [ ] Integrare face match / verifica identità
- [ ] Implementare sezione profilo con consensi concessi alle squadre
- [ ] Consentire al giocatore di visualizzare i propri dati raccolti dalla squadra

### Criteri di accettazione
- Il giocatore può completare il primo accesso in sicurezza.
- Il consenso viene associato alla squadra richiedente.
- La verifica identità è completata tramite OTP e selfie/face match.
- Il giocatore può consultare i propri dati in app.

---

## Epic 3 — Gestione consensi e compliance GDPR

Obiettivo: modellare correttamente il ciclo di vita del consenso, con tracciabilità, revoca e audit.

### Task
- [ ] Definire modello dati del consenso
- [ ] Associare consenso a giocatore, squadra, finalità e data/ora
- [ ] Tracciare storico accettazione/revoca consenso
- [ ] Gestire revoca consenso da app
- [ ] Definire policy di visibilità dati lato Dynamix2
- [ ] Implementare audit log delle operazioni sui consensi
- [ ] Gestire caso email diversa per giocatore già esistente
- [ ] Definire messaggi/privacy copy da mostrare in app

### Criteri di accettazione
- Ogni consenso è tracciato e verificabile.
- La revoca aggiorna immediatamente la visibilità dati.
- Le operazioni rilevanti sono auditabili.
- I casi di identità duplicata o email modificata sono gestiti esplicitamente.

---

## Epic 4 — Sottoscrizione abbonamento, trial e device FANS

Obiettivo: gestire l'accesso consumer all'app, il trial PRO, l'associazione del dispositivo e l'attivazione dell'abbonamento.

### Task
- [ ] Implementare registrazione utente consumer su app K-FANS
- [ ] Implementare flusso `Ho un dispositivo FANS`
- [ ] Implementare flusso `Non ho un dispositivo FANS`
- [ ] Implementare trial PRO di 7 giorni
- [ ] Mostrare CTA verso sito K-FANS durante il trial
- [ ] Associare POD/device all'account utente
- [ ] Attivare funzionalità PRO dopo associazione device
- [ ] Gestire scadenza trial e blocco funzionalità PRO
- [ ] Gestire bundle device + abbonamento PRO 2 anni

### Criteri di accettazione
- Un nuovo utente può entrare in app anche senza device.
- Il trial PRO dura 7 giorni.
- L'utente può associare il device ricevuto al proprio account.
- Le funzionalità PRO sono attive solo se trial o abbonamento sono validi.

---

## Epic 5 — Integrazione ecommerce e workaround operativo

Obiettivo: definire il flusso temporaneo e quello a regime per attivare abbonamenti a partire dall'acquisto del bundle.

### Task
- [ ] Definire flusso temporaneo/manuale di attivazione abbonamento
- [ ] Definire flusso a regime da acquisto ecommerce
- [ ] Ricevere evento ordine completato
- [ ] Creare o aggiornare account utente dopo acquisto
- [ ] Associare ordine, utente, device e abbonamento
- [ ] Gestire stato spedizione dispositivo
- [ ] Inviare email post-acquisto con istruzioni app
- [ ] Gestire errore pagamento OK ma account non creato
- [ ] Gestire rinnovo/scadenza abbonamento

### Criteri di accettazione
- È disponibile un flusso minimo utilizzabile subito.
- Il flusso definitivo è progettato per integrarsi con ecommerce e subscription.
- Ogni ordine è riconciliabile con utente, device e abbonamento.

---

## Epic 6 — Architettura tecnica ecosistema K-FANS

Obiettivo: definire i domini applicativi, le API, gli eventi e il metodo di lavoro Agile/GitHub.

### Task
- [ ] Definire domini applicativi: Identity, Consent, Subscription, Device, Notification
- [ ] Definire modello dati utenti, giocatori, squadre, consensi, device, abbonamenti
- [ ] Definire API tra Dynamix2 e K-FANS
- [ ] Definire eventi di dominio principali
- [ ] Definire ruoli e permessi
- [ ] Definire strategia autenticazione app
- [ ] Definire logging, audit e monitoring
- [ ] Definire gestione errori e retry notifiche
- [ ] Definire workflow Agile su GitHub: epiche, issue, sprint, board, pull request

### Criteri di accettazione
- Esiste una prima architettura condivisa e documentata.
- I principali domini sono separati e comprensibili.
- Il team può trasformare le epiche in user story operative.
- Il lavoro può essere gestito tramite GitHub Issues/Projects.
