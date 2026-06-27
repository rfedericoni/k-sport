# K-FANS — Coding Guidelines

## Obiettivo

Queste linee guida servono a rendere il progetto sviluppabile in modo ordinato da team umani e agenti AI come Codex.

Ogni modifica deve essere tracciabile, testabile e collegata a una issue GitHub.

## Regole generali

- Ogni attività deve partire da una GitHub Issue.
- Ogni branch deve riferirsi a una issue.
- Ogni pull request deve chiudere o collegare una issue.
- Ogni modifica deve includere test quando possibile.
- Ogni decisione architetturale rilevante va documentata.
- Evitare implementazioni implicite non descritte nella issue.
- In caso di ambiguità, documentare l'assunzione nella PR.

## Convenzione branch

```text
feature/<issue-number>-short-description
fix/<issue-number>-short-description
docs/<issue-number>-short-description
spike/<issue-number>-short-description
```

Esempi:

```text
feature/12-player-consent-request
feature/18-device-association
docs/4-api-contract
```

## Convenzione commit

Usare commit piccoli e leggibili.

Formato consigliato:

```text
<type>: <description>
```

Tipi consigliati:

- `feat`
- `fix`
- `docs`
- `test`
- `refactor`
- `chore`
- `ci`

Esempi:

```text
feat: add player consent request endpoint
fix: prevent data visibility without accepted consent
docs: add subscription flow notes
```

## Pull Request

Ogni PR deve contenere:

- Scopo della modifica.
- Issue collegata.
- Cosa è stato implementato.
- Cosa non è stato implementato.
- Test eseguiti.
- Screenshot se modifica UI/mobile.
- Note per reviewer.

## Definition of Ready

Una issue è pronta per lo sviluppo quando contiene:

- Obiettivo chiaro.
- Contesto funzionale.
- Criteri di accettazione.
- Dipendenze note.
- Ambito escluso, se necessario.
- Eventuali mockup o riferimenti.

## Definition of Done

Una issue è completata quando:

- Il codice è implementato.
- I test passano.
- La documentazione è aggiornata se necessario.
- La PR è revisionata.
- I criteri di accettazione sono soddisfatti.
- Non ci sono regressioni note.

## Regole per agenti AI

Gli agenti AI devono:

1. Leggere sempre `AGENTS.md` prima di modificare il repository.
2. Leggere la issue collegata.
3. Verificare i documenti in `docs/`.
4. Fare modifiche minime e coerenti con la issue.
5. Non inventare requisiti non presenti.
6. Segnalare eventuali ambiguità nella PR.
7. Non introdurre dipendenze pesanti senza motivazione.
8. Non modificare aree non correlate.
9. Aggiungere o aggiornare test.
10. Aggiornare la documentazione se cambia il comportamento.

## Testing

Quando lo stack sarà definito, aggiungere regole specifiche per:

- Unit test.
- Integration test.
- API contract test.
- End-to-end test.
- Test mobile.
- Test sicurezza/permessi.

## Sicurezza e privacy

- Non committare segreti.
- Non usare dati reali nei test.
- Non salvare immagini selfie reali nel repository.
- Non loggare password, OTP o token.
- Non loggare dati biometrici.
- Mascherare email e telefono nei log se non necessari.
- Le operazioni sui consensi devono essere auditabili.

## Documentazione tecnica

Aggiornare i documenti quando cambiano:

- Architettura.
- API.
- Modello di dominio.
- Workflow utente.
- Decisioni tecniche.

## Review checklist

Prima di approvare una PR verificare:

- La modifica risolve la issue.
- I criteri di accettazione sono coperti.
- Non ci sono side effect evidenti.
- Naming e struttura sono coerenti.
- Test e documentazione sono adeguati.
- Privacy e sicurezza sono rispettate.
