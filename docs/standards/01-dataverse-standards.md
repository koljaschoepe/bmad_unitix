# 01 – Dataverse Standards (Default-Datenhaltung)

**Ziel**: Einheitliche Datenmodelle und Security für CRM-Workloads.

## Benennung
- **Tabellen**: `org_<bereich>_<entitaet>` (z. B. `org_sales_kunde`)
- **Spalten**: `schema_<name>_<typ>` (z. B. `schema_closeDate_dt`), Primärschlüssel `id` (GUID)
- **Choice/Option Sets**: `opt_<bereich>_<name>`

## Beziehungen & Modellierung
- **1:n** bevorzugen; **n:n** nur bei echter Many-to-Many‑Domäne.
- Pflicht: Diagramm/Übersicht in `docs/architecture/ARCH-*.md` verlinken.
- **Referentielle Regeln** (Cascade, Restrict) dokumentieren.

## Sicherheit
- **Row-Level Security** via Teams/Rollen; Datensatz‑Ownership bewusst wählen (User vs. Team).
- **Feldsicherheit** für sensible Spalten; Audit aktivieren.
- **Datenklassifizierung** (öffentlich/internal/vertraulich) pro Tabelle.

## Solutions & ALM
- **Dev/Test**: Unmanaged; **Prod**: Managed.
- **SemVer** (MAJOR.MINOR.PATCH); Solution‑Exporter im CI.
- **Environment Variables** statt Hardcodings.

## Integration
- **Power Automate** für Standard‑Workflows/Events (Trigger: Dataverse, HTTP).
- **Custom Connector** als API‑Fassade; Auth via Entra.
- **Retry/Backoff** & **Idempotenz** bei wiederholten Events.

## [🟩 Erweiterung: Projektspezifische Tabellen & Policies]
- ...
