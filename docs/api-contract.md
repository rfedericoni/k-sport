# K-FANS — Initial API Contract

## Scopo

Questo documento propone un primo contratto API logico per integrare Dynamix2, K-FANS Backend, app mobile, ecommerce e sistemi di notifica.

Non è ancora una specifica OpenAPI definitiva: serve come base per decomporre il backlog in user story tecniche.

## Convenzioni

- Tutti gli endpoint usano JSON.
- Tutte le date sono in formato ISO 8601 UTC.
- Ogni richiesta applicativa dovrebbe portare un `correlationId`.
- Gli errori devono avere formato coerente.

Esempio errore:

```json
{
  "code": "CONSENT_NOT_FOUND",
  "message": "Consent request not found",
  "correlationId": "..."
}
```

---

## Player Invitation API

### Create player invitation

```http
POST /api/player-invitations
```

Request:

```json
{
  "teamId": "team-123",
  "externalDynamixPlayerId": "dyn-player-456",
  "firstName": "Janik",
  "lastName": "Rutz",
  "email": "janik.rutz@mail.com",
  "phoneNumber": "+393334445566",
  "requestedBy": "staff-user-123"
}
```

Response:

```json
{
  "invitationId": "inv-123",
  "playerId": "player-123",
  "consentId": "consent-123",
  "status": "Sent"
}
```

---

### Get invitation status

```http
GET /api/player-invitations/{invitationId}
```

Response:

```json
{
  "invitationId": "inv-123",
  "status": "Completed",
  "sentAt": "2026-06-27T08:00:00Z",
  "acceptedAt": "2026-06-27T08:30:00Z"
}
```

---

## Consent API

### Get player consent status

```http
GET /api/players/{playerId}/consent-status?teamId={teamId}
```

Response:

```json
{
  "playerId": "player-123",
  "teamId": "team-123",
  "status": "Accepted",
  "acceptedAt": "2026-06-27T08:30:00Z",
  "dataVisibility": "Unlocked"
}
```

---

### Accept consent

```http
POST /api/consents/{consentId}/accept
```

Request:

```json
{
  "userAccountId": "user-123",
  "privacyVersion": "v1.0",
  "acceptedTerms": true
}
```

Response:

```json
{
  "consentId": "consent-123",
  "status": "Accepted",
  "acceptedAt": "2026-06-27T08:30:00Z"
}
```

---

### Revoke consent

```http
POST /api/consents/{consentId}/revoke
```

Request:

```json
{
  "userAccountId": "user-123",
  "reason": "User requested revocation"
}
```

Response:

```json
{
  "consentId": "consent-123",
  "status": "Revoked",
  "revokedAt": "2026-06-27T09:00:00Z"
}
```

---

## Authentication API

### Login

```http
POST /api/auth/login
```

Request:

```json
{
  "email": "janik.rutz@mail.com",
  "password": "temporary-password"
}
```

Response:

```json
{
  "accessToken": "jwt-token",
  "refreshToken": "refresh-token",
  "mustChangePassword": true
}
```

---

### Change password

```http
POST /api/auth/change-password
```

Request:

```json
{
  "currentPassword": "temporary-password",
  "newPassword": "new-secure-password"
}
```

Response:

```json
{
  "success": true
}
```

---

## Identity Verification API

### Request OTP

```http
POST /api/identity/otp/request
```

Request:

```json
{
  "userAccountId": "user-123",
  "phoneNumber": "+393334445566"
}
```

Response:

```json
{
  "otpRequestId": "otp-123",
  "status": "Requested"
}
```

---

### Verify OTP

```http
POST /api/identity/otp/verify
```

Request:

```json
{
  "otpRequestId": "otp-123",
  "code": "123456"
}
```

Response:

```json
{
  "status": "Verified"
}
```

---

### Submit face match

```http
POST /api/identity/face-match
```

Request:

```json
{
  "userAccountId": "user-123",
  "selfieFileId": "file-123"
}
```

Response:

```json
{
  "status": "Verified",
  "verifiedAt": "2026-06-27T08:40:00Z"
}
```

---

## Subscription API

### Start trial

```http
POST /api/trials/start
```

Request:

```json
{
  "userAccountId": "user-123"
}
```

Response:

```json
{
  "trialId": "trial-123",
  "status": "Active",
  "startedAt": "2026-06-27T08:00:00Z",
  "expiresAt": "2026-07-04T08:00:00Z"
}
```

---

### Get current subscription

```http
GET /api/subscriptions/current
```

Response:

```json
{
  "userAccountId": "user-123",
  "proAccess": true,
  "source": "Trial",
  "expiresAt": "2026-07-04T08:00:00Z"
}
```

---

## Device API

### Associate device

```http
POST /api/devices/associate
```

Request:

```json
{
  "userAccountId": "user-123",
  "serialNumber": "FANS-001-ABC"
}
```

Response:

```json
{
  "deviceId": "device-123",
  "status": "Associated",
  "associatedAt": "2026-06-27T08:45:00Z"
}
```

---

## Ecommerce Integration API

### Order completed webhook

```http
POST /api/ecommerce/orders/completed
```

Request:

```json
{
  "externalOrderId": "order-123",
  "email": "user@mail.com",
  "bundle": "FANS_DEVICE_PLUS_PRO_2Y",
  "paymentStatus": "Paid",
  "shippingStatus": "Pending",
  "createdAt": "2026-06-27T08:00:00Z"
}
```

Response:

```json
{
  "orderId": "order-123",
  "status": "Linked",
  "userAccountId": "user-123",
  "subscriptionId": "sub-123"
}
```

## Note implementative

- Gli endpoint sono volutamente separati per dominio.
- La specifica va trasformata in OpenAPI quando lo stack backend sarà definito.
- Gli eventi di dominio dovrebbero essere preferiti per integrazioni asincrone dove possibile.
