# BMAD Agent Playbook für Cursor (inkl. Story-Template, Agent-Prompts und Best Practices)

> Zielgruppe: Teams, die BMAD-METHOD für agentische Planung & Entwicklung nutzen und **in Cursor** arbeiten.  
> Fokus: Klare Prompts, saubere Artefakte (PRD, Architektur, Stories) und reibungsloser IDE-Zyklus.  
> Hinweis: In der Web-UI von BMAD gibt es Stern-Befehle (z. B. `*analyst`). Für **Cursor** nutzen wir in diesem Playbook konsistent **Agent-Handles** im Format `@<rolle>.mdc` (Teamkonvention).

---

## 0) Setup in Cursor (Quickstart)

1. **Voraussetzungen**
   - Node.js **v20+** installiert.
   - Git Repository lokal geklont oder neues Projekt in Cursor angelegt.
2. **BMAD installieren/aktualisieren (im Cursor-Terminal)**
   ```bash
   npx bmad-method install
   # oder (bei bestehender Installation)
   git pull
   npm run install:bmad
   ```
   - Der Installer erkennt bestehende v4-Installationen, aktualisiert **nur** geänderte Dateien und legt bei Konflikten **.bak**-Backups an.
3. **Ordnerstruktur (empfohlen)**
   ```text
   /docs
     /prd
     /architecture
     /stories
     /decisions (ADRs)
   /src (oder /apps, /services, je nach Projekt)
   ```
4. **Arbeitsweise in Cursor**
   - Für Planung & Durchstich: Cursor „Chat“ nutzen; Prompts an die Rollen über die untenstehenden **Agent-Handles** senden (z. B. `@analyst.mdc`).
   - **Artefakte** (PRD, Architektur, Stories) als Markdown-Dateien in den jeweiligen Ordnern pflegen.
   - Pro Story eine eigene Datei (Dateiname: `STORY-<ID>-<kurzname>.md`).

---

## 1) Agent-Handles in Cursor

| Rolle          | Handle        | Zweck (Kurz)                                                                                  |
|----------------|---------------|-----------------------------------------------------------------------------------------------|
| Orchestrator   | `@orchestrator.mdc` | Fluss steuern, Rollenwechsel, Blocker lösen, Fragen routen                                  |
| Analyst        | `@analyst.mdc`     | PRD erstellen/verfeinern (Use Cases, Akzeptanzkriterien, NFRs, Domänenbegriffe)            |
| Product Manager| `@pm.mdc`          | MVP/Release-Plan, Priorisierung, DoR/DoD                                                    |
| Architect      | `@architect.mdc`   | Zielarchitektur, Datenmodell, Schnittstellen, Security, ADRs                                |
| Scrum Master   | `@sm.mdc`          | Sharding in Epics/Stories, Vollkontext je Story, Abhängigkeiten                             |
| Developer      | `@dev.mdc`         | Umsetzung gem. Story, offene Punkte dokumentieren                                           |
| QA (Senior Reviewer) | `@qa.mdc`    | Review gegen Akzeptanzkriterien, Testkatalog, Sicherheit/Architektur-Checks                 |

> Die Endung `.mdc` ist eine **Teamkonvention** für „Mode/Domain Command“. Du kannst die Handles bei Bedarf an eure Namensgebung anpassen.

---

## 2) End-to-End Prozess (Artefaktfluss)

1. **PRD** (`@analyst.mdc`)  
2. **Release-Plan** (`@pm.mdc`)  
3. **Architektur** (`@architect.mdc`) + **ADRs**  
4. **Stories** (`@sm.mdc`) – je Story Vollkontext  
5. **Implementierung** (`@dev.mdc`)  
6. **Review & Tests** (`@qa.mdc`)  
7. **Merge/Done** (`@orchestrator.mdc` koordiniert)

---

## 3) Prompt-Starter je Agent (Kurz + Lang)

> Hinweise:  
> - **Kurzprompts** eignen sich für Schnelleinstiege.  
> - **Langprompts** kannst du als **Cursor-Snippet** speichern (z. B. „/bmad-architect-start“).  
> - Ersetze die `{{…}}`-Platzhalter durch Projektdaten.

### 3.1 Orchestrator – `@orchestrator.mdc`

**Kurzprompt**
```
@orchestrator.mdc
Kontext: {{Produkt/Projekt}}. Ziel: {{Zielbild}}. Randbedingungen: {{Constraints}}.
Bitte führe mich durch den BMAD-Fluss und nenne als Nächstes die logisch nächste Rolle mit 1–2 klaren To-dos.
```

**Langprompt**
```
@orchestrator.mdc
Rolle: Orchestrator. Aufgabe: Fluss steuern, Rollenwechsel vorschlagen, offene Punkte konsolidieren.
Kontext: {{Projektname}}, Stakeholder: {{…}}, Scope/Out-of-Scope: {{…}}.
Bitte:
1) Prüfe meinen Kontext auf Lücken.
2) Schlage die nächste Rolle (Analyst/PM/Architect/SM/Dev/QA) mit klaren To-dos vor.
3) Dokumentiere Entscheidungen als Aufgaben für ADRs/Decisions.
```

### 3.2 Analyst – `@analyst.mdc`

**Prompt-Kern (Stichworte)**  
- Personas, Use Cases (priorisiert), Akzeptanzkriterien (testbar), NFRs, Glossar/Domänenbegriffe, Erfolgskriterien/ROI.

**Kurzprompt**
```
@analyst.mdc
Bitte erstelle ein PRD-Basisskelett mit: Ziele, Personas, priorisierte Use Cases, Akzeptanzkriterien, NFRs, Datenobjekte/Glossar, Erfolgskriterien.
Kontext: {{Business-Ziele}}, Randbedingungen: {{Compliance/Security}}.
```

**Langprompt**
```
@analyst.mdc
Ziel: Vollständiges PRD (testbare Akzeptanzkriterien).
Input: {{Zielbild, Stakeholder, Pain Points, Scope/Out-of-Scope, Annahmen, Abhängigkeiten}}.
Bitte liefere:
1) Personas + Top-Use-Cases (MoSCoW), je Use Case: Trigger, Happy Path, Edge Cases, Akzeptanzkriterien.
2) NFRs (Performance, Sicherheit, Verfügbarkeit, Datenschutz), Messgrößen.
3) Zentrale Datenobjekte (Feldkatalog high-level).
4) Erfolgskriterien/Business-Impact (z. B. Durchlaufzeit, Fehlerquote).
Output: `docs/prd/PRD-{{Projekt}}.md` (strukturierter Entwurf).
```

### 3.3 Product Manager – `@pm.mdc`

**Prompt-Kern**  
- MVP, Releases, Priorisierung (Value/Aufwand/Risiko), DoR/DoD.

**Kurzprompt**
```
@pm.mdc
Leite aus dem PRD einen MVP und einen Release-Plan ab. Formuliere DoR/DoD und priorisiere Epics nach Value/Aufwand/Risiko.
```

**Langprompt**
```
@pm.mdc
Input: PRD ({{Pfad}}). Ziele: MVP abgrenzen, Releases & Meilensteine, DoR/DoD.
Bitte:
1) Epic-Liste mit Scores (Value, Aufwand, Risiko) und Abhängigkeiten.
2) MVP-Scope (minimal lauffähig, messbarer Nutzen).
3) Release-Plan (M1/M2 …) inkl. überprüfbarer Outcomes.
4) DoR/DoD Definition für spätere Story-Erstellung.
Output: `docs/prd/RELEASE-{{Projekt}}.md`.
```

### 3.4 Architect – `@architect.mdc`

**Prompt-Kern**  
- Kontext-/Komponenten-/Sequenzansichten, Datenmodell, Schnittstellen/APIs, Security/Identity, ADRs.

**Kurzprompt**
```
@architect.mdc
Erstelle eine Zielarchitektur (Kontext-, Komponenten-, Sequenzskizzen), ein grobes Datenmodell, API-Verträge und Sicherheits-/Identity-Konzept. Liste Entscheidungen als ADRs.
```

**Langprompt**
```
@architect.mdc
Input: PRD {{Pfad}}, Release-Plan {{Pfad}}.
Bitte liefere:
1) Architekturübersicht mit Kontext-, Komponenten- und Sequenzbeschreibung (Text + einfache ASCII-Skizze/PlantUML bei Bedarf).
2) Datenmodell (Hauptentitäten, Beziehungen), Integrationspunkte (REST/GraphQL/Event).
3) Security/Identity (AuthN, AuthZ, Data Protection), Mandantenfähigkeit falls relevant.
4) ADR-Vorschläge (Titel, Kontext, Entscheidung, Konsequenzen).
Output: `docs/architecture/ARCH-{{Projekt}}.md`, ADRs unter `docs/decisions/ADR-*.md`.
```

### 3.5 Scrum Master – `@sm.mdc`

**Prompt-Kern**  
- Sharding von PRD/Architektur in Epics/Stories, Vollkontext je Story (Ziel, Regeln, UI, Daten, Schnittstellen, Tests, Telemetrie), Reihenfolge/Abhängigkeiten.

**Kurzprompt**
```
@sm.mdc
Zerlege PRD + Architektur in Epics und dann in hyperdetaillierte Stories mit Vollkontext (Ziel, Geschäftsregeln, UI-Hinweise, Datenfelder, API-Verträge, Edge-Cases, Tests, Telemetrie). Nenne die empfohlene Reihenfolge.
```

**Langprompt**
```
@sm.mdc
Input: PRD {{Pfad}}, Architektur {{Pfad}}, DoR/DoD.
Bitte:
1) Epics ableiten, dann Stories sharden (je Story: ID, Titel, Ziel, Abhängigkeiten).
2) Je Story Vollkontext (siehe Story-Template unten) inkl. Testideen und Telemetrie.
3) Reihenfolge/Plan (kritischer Pfad, Parallelisierungsmöglichkeiten).
Output: `docs/stories/STORY-*.md` pro Story + `docs/stories/PLAN.md`.
```

### 3.6 Developer – `@dev.mdc`

**Prompt-Kern**  
- Umsetzung je Story, Architektur-Patterns einhalten, offene Punkte dokumentieren.

**Kurzprompt**
```
@dev.mdc
Implementiere STORY-{{ID}} gemäß Template. Dokumentiere offene Fragen direkt im Abschnitt „Offene Punkte“ der Story-Datei. Halte Architektur/Patterns ein.
```

**Langprompt**
```
@dev.mdc
Input: `docs/stories/STORY-{{ID}}-{{kurzname}}.md`.
Bitte:
1) Implementiere Code (Frontend/Service), Schnittstellen anbinden, Datenzugriff realisieren.
2) Notiere offene Punkte/Annahmen im Story-Dokument.
3) Aktualisiere den Status-Block (Umsetzung, Tests lokal, Screenshots/Logs).
Output: Code + aktualisierte Story-Datei. Bereite PR vor.
```

### 3.7 QA (Senior Reviewer) – `@qa.mdc`

**Prompt-Kern**  
- Review gegen Akzeptanzkriterien, Sicherheit/Architektur, Testfallkatalog, Freigabe/Änderungen.

**Kurzprompt**
```
@qa.mdc
Review für STORY-{{ID}}: Prüfe gegen Akzeptanzkriterien, Edge-Cases, Sicherheit/Architektur-Compliance. Ergänze einen kompakten Testfallkatalog. Ergebnis: „Pass“ oder „Änderungen notwendig“.
```

**Langprompt**
```
@qa.mdc
Input: Story {{Pfad}}, Code-Änderungen (Diff/PR).
Bitte:
1) Prüfe fachliche Kriterien, NFRs, Sicherheit/Fehlerpfade.
2) Erstelle/ergänze Testfälle (funktional/negativ/Last).
3) Dokumentiere Findings und Empfehlung (Pass/Changes).
Output: Kommentarblöcke in Story + Review-Notizen für PR.
```

---

## 4) Story-Datei – Master-Template (Markdown)

> Speichere pro Story eine Datei `docs/stories/STORY-<ID>-<kurzname>.md` und fülle die Blöcke konsistent. Dieses Template ist **werkzeugagnostisch** und passt sowohl für Power-Frontends als auch für Services/Functions.

```markdown
# STORY-<ID>: <Titel>

## 1. Ziel & Kontext
- Kurzbeschreibung
- Bezug zu PRD: Abschnitt/Use Case
- Business-Wert (Impact/KPI)

## 2. Fachliche Regeln
- Regeln & Validierungen
- Sonder-/Fehlerfälle (Edge Cases)
- Annahmen & Risiken

## 3. UI/UX (falls Frontend)
- Screens/Komponenten (z. B. Formular, Liste, Detail)
- Zustandslogik (Loading/Empty/Error)
- Accessibility/Internationalisierung
- Beispiel-Skizzen (ASCII/PlantUML optional)

## 4. Daten & Modell
- Entitäten/Felder (Name, Typ, Pflicht, Regeln)
- Beziehungen/Referenzen
- Mapping zu Persistenz/Datenquelle (z. B. Dataverse-Tabelle, SQL-View)

## 5. Schnittstellen
- Eingehende/ausgehende APIs (Pfad, Methode, Payload, Statuscodes)
- AuthN/AuthZ & Scopes
- Fehlerbehandlung & Retries

## 6. Sicherheit & Compliance
- Datenklassifizierung
- PII/DSGVO-Handhabung
- Audit/Protokollierung

## 7. Nicht-funktionale Anforderungen
- Performance-Ziele (Latenz, Throughput)
- Verfügbarkeit/Resilienz
- Observability (Logs/Metriken/Traces)

## 8. Tests
- Akzeptanzkriterien (Given/When/Then)
- Funktionale Tests (Happy/Unhappy Path)
- Randfälle/Lasttests
- Testdaten

## 9. Telemetrie & KPI
- Ereignisse/Metriken
- Dashboards/Alerts

## 10. Implementierungshinweise
- Architektur-Patterns/Konventionen
- Ordner/Module
- Reuse/Dependencies

## 11. Offene Punkte
- Fragen, Entscheidungen ausstehend
- Risiken/Nebenwirkungen

## 12. Review & Status
- Dev-Notizen (Datum, Commits, Screenshots)
- QA-Notizen (Findings, Teststatus)
- Entscheidung: Pass / Changes
```

---

## 5) Typische Best-Use-Cases (für schnelle Erfolge)

1. **CRUD‑Apps & Workflows** (z. B. Anträge, Ticketing, Genehmigungen) – klare Domäne, gute Eignung für Story-Sharding.  
2. **Integrations-Layer** (APIs/ETL zwischen Systemen) – eindeutig spezifizierbare Verträge, robuste Tests möglich.  
3. **Dokumentengetriebene Prozesse** (Formulare, Upload, Prüfung, Archiv) – hohe Wiederverwendbarkeit von Story-Blocks.  
4. **Reporting/BI‑Backlogs** (Datenmodell + KPIs + Telemetrie) – Stories für Modellierung, Transformation, Visualisierung.  
5. **MVP‑Prototypen** – schnelles PRD/Architektur, wenige Epics, klare Abbruchkriterien.

---

## 6) Hinweise & Stolpersteine (aus der Praxis)

- **Gate-Reviews fest einplanen**: PRD → Architektur → Story-Sharding → Implementierung. Ohne Gate droht „Scheinpräzision“.
- **Story‑Granularität**: Lieber kleiner und vollständig. Jede Story muss „allein lauffähig“ (bzw. klar testbar) sein.
- **Kontexttransport**: Alle nötigen Infos gehören **in die Story-Datei** – nicht in Köpfe/Chats.
- **Security/Compliance**: Keine sensiblen Rohdaten in Prompts. Secrets über sichere Variablen/Key Vault referenzieren.
- **Modellwahl & Kosten**: Qualität/Kosten variieren je Modell. Für Konsistenz Prompts/Policies versionieren.
- **CI/CD & Review**: PRs verpflichtend; QA-Agent als eigenständiger Reviewer ernst nehmen.
- **Telemetry by default**: Story-Abschnitt „Telemetrie & KPI“ konsequent füllen, sonst fehlt später Transparenz.

---

## 7) Spezifika für Microsoft‑Hosting (Power Apps Code Apps + Azure)

- **Frontends**: UI/UX-Stories klar von **Service/Function‑Stories** trennen.
- **Dataverse/SQL**: Datenquellen früh im PRD/Architektur festlegen; Feldkataloge in Story „Daten & Modell“ übernehmen.
- **Power Automate**: Integrationen/Events als eigenständige Stories (Trigger, Payload, Fehlerpfade).
- **Azure Functions/Container**: API-Verträge und AuthZ (Entra ID) in „Schnittstellen“/„Sicherheit“ exakt beschreiben.
- **Power BI/Telemetry**: Kennzahlen/Events pro Story definieren; spätere Dashboards sparen Zeit.

---

## 8) Daily Flow (Kompakt-Checkliste)

- **Morgens**: `@orchestrator.mdc` – Blocker & Tagesplan.  
- **Vormittags**: `@sm.mdc` – neue/angepasste Stories sharden.  
- **Dev‑Slot**: `@dev.mdc` – Umsetzung je Story; Notizen direkt in die Datei.  
- **Nachmittags**: `@qa.mdc` – Reviews/Testfallkataloge; Freigaben.  
- **Abends**: Merge, Changelog, Telemetrie prüfen.

---

## 9) Mini‑Vorlagen für ADR & Decision

**ADR‑Skeleton (`docs/decisions/ADR-<nummer>-<titel>.md`)**
```markdown
# ADR-<nummer>: <titel>
## Kontext
## Entscheidung
## Konsequenzen
## Alternativen
```

**Decision‑Log (`docs/decisions/DECISIONS.md`)**
```markdown
- <datum> – <kurzbeschreibung der entscheidung> – <verantwortlich> – <referenzen>
```

---

## 10) Nächste Schritte

1. Repo klonen / Projekt öffnen, **Installationsbefehl** ausführen.  
2. `@orchestrator.mdc` starten → `@analyst.mdc` PRD erzeugen.  
3. `@pm.mdc` Release-Plan & DoR/DoD.  
4. `@architect.mdc` Architektur & ADRs.  
5. `@sm.mdc` Stories erzeugen.  
6. `@dev.mdc` umsetzen → `@qa.mdc` reviewen → merge.

---

**Viel Erfolg – und halte alle Artefakte konsistent im Repo.**  
Wenn du willst, können wir dieses Playbook im Projekt-Repo als `CONTRIBUTING-bmad.md` einchecken und CI‑Checks ergänzen, die z. B. unvollständige Story‑Blöcke erkennen.
