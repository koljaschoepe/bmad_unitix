# 04 – Qualität & Testing

**Ziel**: Hohe Qualität durch klare Gates. Dev‑Agent erstellt Tests mit.

## Testpyramide
- **Unit**: Logik/Helper/Rules.
- **Integration**: API/Connector/Dataverse‑Interaktion.
- **E2E**: User‑Flows (z. B. Playwright, Test Studio).

## Definition of Ready (DoR)
- Story‑Template vollständig (Regeln, Daten, Schnittstellen, Security, Telemetrie).
- Akzeptanzkriterien **testbar**.

## Definition of Done (DoD)
- Tests **grün**, Telemetrie integriert.
- Security‑Check **pass** (mind. Checkliste).
- Doku/ADR aktualisiert.

## QA‑Gate
- **QA‑Agent** prüft Story & PR gegen DoD/NFR/Security.
- Automatisierte Checks bevorzugen; manuelle Reviews nur ergänzend.

## [🟩 Erweiterung: Tools/Frameworks pro Projekt]
- ...
