# 02 – API Standards & Integration

**Ziel**: Konsistente, sichere Schnittstellen für Code Apps/Flows/Functions.

## Designgrundsätze
- **REST** mit Plural‑Ressourcen, sprechenden Pfaden; Versionierung `/v1`.
- **OpenAPI 3.x** Pflicht für eigene Services/Functions.
- **Kontrakte stabilisieren** (Consumer‑Driven Contracts).

## Authentifizierung/Autorisierung
- **Entra ID / OAuth2**; Delegated vs. App Roles dokumentieren.
- Keine API‑Keys im Client; Tokenverwaltung serverseitig/Connector.

## Fehlerformat (Problem+JSON)
```json
{
  "type": "https://errors.example.com/validation",
  "title": "Validation error",
  "status": 422,
  "detail": "Field 'amount' must be >= 0",
  "instance": "f8a3b1c8-..."
}
```

## Idempotenz & Zuverlässigkeit
- Wiederholbare Operationen: **Idempotency-Key** (Header) akzeptieren.
- **Retry mit Exponential Backoff**; Dead‑letter Strategien dokumentieren.

## Integrationsmuster
- **Custom Connector** vor direktem HTTP im UI.
- Ereignisse/Queues für asynchrone Vorgänge (z. B. Service Bus).
- Webhooks nur mit Validierung/Signatur.

## [🟩 Erweiterung: Service-Liste, Rate Limits, Throttling]
- ...
