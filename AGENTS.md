# AGENTS.md — K-Sport / K-FANS

## Purpose

This repository is intended to be developed with an AI-first workflow using GitHub Issues, pull requests and coding agents.

Agents must use the documents in `docs/` as source of truth for product vision, architecture, domain model, backlog and API contracts.

## Required reading order

Before making changes, read:

1. `docs/vision.md`
2. `docs/k-fans-backlog.md`
3. `docs/architecture.md`
4. `docs/domain-model.md`
5. `docs/api-contract.md`
6. `docs/coding-guidelines.md`
7. The GitHub Issue assigned to the task

## Working rules

- Work only on the scope described by the issue.
- Do not implement unrelated features.
- Do not introduce new architecture without documenting it.
- Do not add external dependencies unless clearly justified.
- Prefer small, reviewable changes.
- If a requirement is ambiguous, make the smallest reasonable assumption and document it in the pull request.
- Keep privacy, consent and auditability as first-class requirements.

## Product context

K-FANS connects:

- Dynamix2, used by teams and preparators.
- K-FANS mobile app, used by players and consumers.
- Consent management for biometric and sport data.
- Device/POD association.
- Trial and PRO subscription flows.
- Ecommerce integration.

The most important product rule is:

> A team must not see player biometric data unless the player has explicitly accepted a valid consent.

## Architectural domains

Expected domains:

- Identity
- Consent
- Player Invitation
- Notification
- Verification
- Subscription
- Device
- Ecommerce Integration
- Audit / Logging

## Branch naming

Use:

```text
feature/<issue-number>-short-description
fix/<issue-number>-short-description
docs/<issue-number>-short-description
spike/<issue-number>-short-description
```

## Commit style

Use simple conventional commits:

```text
feat: add consent request endpoint
fix: hide biometric data without consent
docs: update architecture overview
```

## Pull request requirements

Every pull request should include:

- Linked issue.
- Summary.
- Implementation notes.
- Tests performed.
- Open questions or assumptions.
- Screenshots for UI changes.

## Privacy constraints

Use only anonymized or synthetic data in examples and tests.

Avoid storing personal, biometric or verification-related sample data in the repository.

Consent, identity verification and subscription operations must be auditable.

## Definition of Done

A task is done only when:

- The issue acceptance criteria are satisfied.
- Tests are added or updated where applicable.
- Documentation is updated if behavior or architecture changes.
- The pull request is small enough to review.
- No known privacy or security regression is introduced.
