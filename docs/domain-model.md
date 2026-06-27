# K-FANS — Domain Model

## Scopo

Questo documento descrive il primo modello di dominio dell'ecosistema K-FANS.

È una base iniziale da validare con business, prodotto e team tecnico.

## Entità principali

### Team / Club

Rappresenta una società sportiva o una squadra che utilizza Dynamix2.

Campi candidati:

- `id`
- `name`
- `externalDynamixId`
- `sport`
- `country`
- `status`
- `createdAt`
- `updatedAt`

Relazioni:

- Ha molti giocatori.
- Può richiedere consensi ai giocatori.
- Può accedere ai dati solo se esiste consenso valido.

---

### Player

Rappresenta un atleta censito in Dynamix2.

Campi candidati:

- `id`
- `externalDynamixPlayerId`
- `firstName`
- `lastName`
- `email`
- `phoneNumber`
- `dateOfBirth`
- `teamId`
- `status`
- `createdAt`
- `updatedAt`

Relazioni:

- Appartiene a una o più squadre.
- Può essere collegato a un account K-FANS.
- Ha uno o più consensi.

---

### UserAccount

Rappresenta un account dell'app K-FANS.

Campi candidati:

- `id`
- `email`
- `phoneNumber`
- `passwordHash`
- `temporaryPasswordExpiresAt`
- `mustChangePassword`
- `role`
- `status`
- `createdAt`
- `updatedAt`

Ruoli candidati:

- `Player`
- `Consumer`
- `Admin`
- `Staff`

Relazioni:

- Può essere associato a un Player.
- Può avere device associati.
- Può avere trial e abbonamenti.

---

### PlayerInvitation

Rappresenta la richiesta di consenso/invito generata da Dynamix2.

Campi candidati:

- `id`
- `teamId`
- `playerId`
- `email`
- `phoneNumber`
- `status`
- `temporaryCredentialsCreatedAt`
- `sentAt`
- `expiresAt`
- `acceptedAt`
- `createdAt`
- `updatedAt`

Stati candidati:

- `Draft`
- `Pending`
- `Sent`
- `Opened`
- `Completed`
- `Expired`
- `Failed`

---

### Consent

Rappresenta il consenso dato dal giocatore alla squadra.

Campi candidati:

- `id`
- `playerId`
- `teamId`
- `userAccountId`
- `purpose`
- `status`
- `requestedAt`
- `acceptedAt`
- `revokedAt`
- `source`
- `privacyVersion`
- `createdAt`
- `updatedAt`

Stati candidati:

- `Requested`
- `Accepted`
- `Revoked`
- `Expired`

Regole:

- I dati biometrici sono visibili alla squadra solo se esiste un consenso `Accepted` valido.
- Ogni cambiamento di stato deve generare audit log.

---

### IdentityVerification

Rappresenta la verifica dell'identità del giocatore.

Campi candidati:

- `id`
- `userAccountId`
- `otpStatus`
- `faceMatchStatus`
- `verifiedAt`
- `createdAt`
- `updatedAt`

Stati OTP:

- `NotRequested`
- `Requested`
- `Verified`
- `Failed`
- `Expired`

Stati Face Match:

- `NotStarted`
- `Pending`
- `Verified`
- `Rejected`
- `Failed`

---

### Device

Rappresenta il device/POD FANS.

Campi candidati:

- `id`
- `serialNumber`
- `model`
- `status`
- `assignedToUserAccountId`
- `associatedAt`
- `createdAt`
- `updatedAt`

Stati candidati:

- `Available`
- `Shipped`
- `Associated`
- `Blocked`
- `Retired`

---

### Trial

Rappresenta un trial PRO dell'app K-FANS.

Campi candidati:

- `id`
- `userAccountId`
- `startedAt`
- `expiresAt`
- `status`

Stati candidati:

- `Active`
- `Expired`
- `Converted`
- `Cancelled`

Regole:

- Durata iniziale: 7 giorni.
- Un utente non dovrebbe poter attivare trial infiniti.

---

### Subscription

Rappresenta un abbonamento PRO.

Campi candidati:

- `id`
- `userAccountId`
- `orderId`
- `deviceId`
- `plan`
- `status`
- `startsAt`
- `expiresAt`
- `createdAt`
- `updatedAt`

Piani candidati:

- `ProMonthly`
- `ProYearly`
- `ProBundleTwoYears`

Stati candidati:

- `Active`
- `Expired`
- `Cancelled`
- `PendingActivation`

---

### Order

Rappresenta un ordine ecommerce o un ordine gestito tramite workaround operativo.

Campi candidati:

- `id`
- `externalOrderId`
- `userAccountId`
- `email`
- `status`
- `paymentStatus`
- `shippingStatus`
- `createdAt`
- `updatedAt`

Stati ordine:

- `Created`
- `Paid`
- `Fulfilled`
- `Cancelled`
- `Failed`

---

### AuditLog

Rappresenta una traccia immutabile delle operazioni rilevanti.

Campi candidati:

- `id`
- `actorId`
- `actorType`
- `entityType`
- `entityId`
- `operation`
- `payload`
- `createdAt`
- `correlationId`

Eventi da auditare:

- Richiesta consenso.
- Accettazione consenso.
- Revoca consenso.
- Verifica identità.
- Sblocco visibilità dati.
- Attivazione abbonamento.
- Associazione device.

## Relazioni principali

```text
Team 1---N Player
Team 1---N Consent
Player 1---N Consent
Player 0---1 UserAccount
UserAccount 1---N Device
UserAccount 1---N Trial
UserAccount 1---N Subscription
Order 0---1 UserAccount
Order 0---1 Subscription
Device 0---1 Subscription
```

## Domande aperte

- Un giocatore può appartenere contemporaneamente a più squadre?
- Il consenso è per squadra, per stagione, per finalità o per dataset?
- Cosa succede se una squadra cambia email a un giocatore già esistente?
- Il numero di telefono è obbligatorio per tutti i giocatori?
- Il face match è obbligatorio o può essere configurabile?
- Il trial è disponibile anche per giocatori professionisti invitati?
- Il device può essere trasferito da un account a un altro?
