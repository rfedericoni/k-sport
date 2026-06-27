# K-FANS — Architecture Overview

## Scopo

Questo documento definisce una prima architettura logica per l'ecosistema K-FANS, derivata dai workflow di invito giocatore, gestione consensi, abbonamento, trial e associazione device.

## Vista logica

```text
Dynamix2
  |
  | API / Eventi
  v
K-FANS Backend
  |-- Identity Service
  |-- Consent Service
  |-- Player Invitation Service
  |-- Notification Service
  |-- Subscription Service
  |-- Device Service
  |-- Order / Ecommerce Integration
  |-- Audit / Logging
  |
  v
K-FANS Mobile App
```

## Domini applicativi

### Identity

Gestisce utenti, autenticazione, credenziali temporanee, cambio password, ruoli, profili e associazione tra giocatore Dynamix2 e account K-FANS.

Responsabilità:

- Creazione account giocatore invitato.
- Login con credenziali temporanee.
- Cambio password obbligatorio.
- Gestione profilo utente.
- Associazione identità giocatore/app.

### Consent

Gestisce il ciclo di vita dei consensi tra giocatore e squadra.

Responsabilità:

- Creazione richiesta consenso.
- Accettazione consenso.
- Revoca consenso.
- Storico consensi.
- Visibilità dati verso Dynamix2.
- Audit compliance.

### Player Invitation

Gestisce l'invito del giocatore partendo da Dynamix2.

Responsabilità:

- Ricezione richiesta da Dynamix2.
- Generazione credenziali temporanee.
- Invio comunicazione al giocatore.
- Stato dell'invito.
- Retry e gestione errori.

### Notification

Gestisce email, SMS e notifiche.

Responsabilità:

- Email invito giocatore.
- SMS OTP.
- Email post acquisto.
- Notifiche operative.
- Retry invii falliti.

### Verification

Gestisce verifica identità.

Responsabilità:

- OTP via SMS.
- Raccolta selfie.
- Face match.
- Stato verifica identità.

### Subscription

Gestisce trial, abbonamenti e funzionalità PRO.

Responsabilità:

- Trial PRO 7 giorni.
- Attivazione abbonamento.
- Scadenza abbonamento.
- Stato funzionalità PRO.
- Bundle device + abbonamento 2 anni.

### Device

Gestisce device/POD FANS.

Responsabilità:

- Associazione device ad account.
- Stato device.
- Validazione device.
- Collegamento device/abbonamento.

### Ecommerce Integration

Gestisce il collegamento con sito ecommerce o flusso manuale temporaneo.

Responsabilità:

- Evento ordine completato.
- Associazione ordine/account/device/abbonamento.
- Stato spedizione.
- Workaround operativo temporaneo.

## Integrazione Dynamix2

Dynamix2 deve poter:

- Censire o modificare un giocatore con email e telefono.
- Richiedere consenso tramite azione `Ask for consent`.
- Visualizzare lo stato del consenso.
- Bloccare i dati se il consenso manca.
- Sbloccare i dati quando il consenso è accettato.
- Mostrare contatore consensi accettati/totali.

## Eventi di dominio iniziali

```text
PlayerConsentRequested
PlayerInvitationSent
PlayerTemporaryCredentialsCreated
PlayerFirstLoginCompleted
PlayerPasswordChanged
PlayerConsentAccepted
PlayerConsentRevoked
PlayerOtpVerified
PlayerIdentityVerified
PlayerDataVisibilityUnlocked
PlayerDataVisibilityLocked
ConsumerRegistered
TrialStarted
TrialExpired
DeviceAssociated
SubscriptionActivated
SubscriptionExpired
OrderCompleted
OrderLinkedToAccount
```

## API candidate

### Dynamix2 → K-FANS

```http
POST /api/player-invitations
GET /api/players/{playerId}/consent-status
POST /api/players/{playerId}/request-consent
```

### K-FANS → Dynamix2

```http
POST /api/dynamix2/players/{playerId}/consent-accepted
POST /api/dynamix2/players/{playerId}/consent-revoked
```

### App → K-FANS Backend

```http
POST /api/auth/login
POST /api/auth/change-password
POST /api/consents/{consentId}/accept
POST /api/consents/{consentId}/revoke
POST /api/identity/otp/request
POST /api/identity/otp/verify
POST /api/identity/face-match
POST /api/devices/associate
GET /api/subscriptions/current
POST /api/trials/start
```

## Sicurezza

- TLS obbligatorio.
- Password temporanee a scadenza.
- Cambio password obbligatorio al primo accesso.
- OTP SMS per verifica identità.
- Audit log per tutte le operazioni sui consensi.
- Principio del minimo privilegio.
- Separazione ruoli: preparatore, giocatore, consumer, admin.
- Nessuna visibilità dei dati biometrici senza consenso valido.

## Osservabilità

- Log strutturati.
- Correlation ID tra Dynamix2, backend K-FANS e notifiche.
- Metriche su invii email/SMS.
- Metriche su consensi accettati/revocati.
- Metriche su trial, device associati e abbonamenti.
- Alert su errori di sincronizzazione consenso.

## Decisioni ancora da prendere

- Stack tecnologico backend.
- Stack mobile app.
- Provider OTP SMS.
- Provider face match.
- Provider ecommerce/subscription.
- Modalità integrazione con Dynamix2: REST, eventi, webhook o ibrida.
- Strategia multi-tenant per società/squadre.
- Strategia di migrazione dati esistenti.
