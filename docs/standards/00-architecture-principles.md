# 00 – Architekturprinzipien (Power Platform Default)

**Ziel**: Modulare Architektur, schnelle MVPs, hohe Wartbarkeit. Power Platform (Code Apps/React, Dataverse, Power Automate, Azure) ist der **Default-Stack**.

## Leitprinzipien
- **Clean Architecture**: Trennung von UI (Code Apps/React), Domain (Services/Logik), Infrastruktur (Dataverse, Azure Functions, Connectors).
- **Separation of Concerns**: Kein Datenzugriff direkt im UI; Logik in Services/Flows/Functions.
- **Security by Design**: Entra ID, Least Privilege, Secrets nie im Code.
- **Extensibility First**: Konfiguration vor Code; Custom Connector statt direkter HTTP-Calls im UI.
- **Telemetry by Default**: Jede Story definiert Metriken/Events.
- **Accelerate MVPs**: Erst die minimale Ende-zu-Ende-Kette (UI → API/Flow → Dataverse), dann vertiefen.

## Zielarchitektur (High Level)
- **Frontend**: Power Platform Code Apps (React) – nur Darstellung/Interaktion.
- **Integration/Services**: Power Automate Flows **oder** Azure Functions (HTTP/Queue) als Backing Services.
- **Daten**: Primär Dataverse; optional Azure SQL/Storage, wenn begründet (ADR).
- **Identität**: Entra ID (Benutzer & Service Principals).

## [🟩 Erweiterung: Projekt-spezifische Prinzipien]
- ...
