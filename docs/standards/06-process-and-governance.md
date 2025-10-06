# 06 – Prozess & Governance (GitHub)

**Ziel**: Reproduzierbarer Ablauf für alle Projekte.

## Branching
- **Trunk‑based** mit kurzen Feature‑Branches (`feature/<id>-<kurzname>`).
- PRs früh und klein; Rebase bevorzugt.

## Pull Requests
- PR‑Template nutzen (Akzeptanzkriterien, Security, Telemetrie).
- Mind. **1 QA‑Review** + **1 Dev‑Review**.

## CI/CD
- Lint/Test auf jedem PR.
- Solution‑Build & Export (Managed für Prod).
- Release‑Tags nach **SemVer**.

## ADRs & Entscheidungen
- Architekturentscheidungen als `docs/decisions/ADR-*.md`.
- Jede Abweichung von Standards begründen.

## [🟩 Erweiterung: Release-Umgebungen, Gates, Namenskonventionen]
- ...
