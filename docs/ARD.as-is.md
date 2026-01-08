# Architecture Reference Document (AS-IS)

## System Overview
The system is a modular monolith with a thin HTTP API and internal modules. The root solution is `src/CompanyName.MyMeetings.sln`, with an API project under `src/API/CompanyName.MyMeetings.API`. Modules live under `src/Modules/<ModuleName>/` and shared code under `src/BuildingBlocks/`. Database tooling and scripts are under `src/Database/`.
Sources: `docs/00-repo-inventory.md`, `docs/01-architecture-map.md`, `docs/02-module-catalog.md`, `AGENTS.md`.
Unknowns: None stated in sources.

## Module Structure and Layers
Each module is split into `Application`, `Domain`, `Infrastructure`, and `IntegrationEvents` projects. Layering and dependency rules are enforced by shared MSBuild rules. Cross-module references should use `IntegrationEvents` unless explicitly allowed.
Sources: `docs/01-architecture-map.md`, `docs/03-dependency-rules.md`, `AGENTS.md`.
Unknowns: None stated in sources.

## API Boundary
The API project depends on module Infrastructure projects via MSBuild rules. Direct API references to module Domain or Application are not part of the defined dependency rules.
Sources: `docs/01-architecture-map.md`, `docs/03-dependency-rules.md`, `AGENTS.md`.
Unknowns: None stated in sources.

## Runtime Topology (Repo-Defined)
Local container topology is defined in `docker-compose.yml` with `backend` (API), `mymeetingsdb` (SQL Server), and `migrator` (DbUp) services. The API container runs `CompanyName.MyMeetings.API.dll` via `src/entrypoint.sh` and is built from `src/Dockerfile`.
Sources: `docs/00-repo-inventory.md`, `docs/01-architecture-map.md`.
Unknowns: None stated in sources.

## Configuration Sources
The API loads configuration from `appsettings.json`, `appsettings.{Environment}.json`, user secrets, and environment variables prefixed with `Meetings_`. Connection string keys are expected as `MeetingsConnectionString`.
Sources: `docs/00-repo-inventory.md`, `docs/01-architecture-map.md`, `AGENTS.md`.
Unknowns: None stated in sources.

## Diagrams and Assets
Architecture diagrams are stored under `docs/C4/` and `docs/Images/`, including a solution diagram.
Sources: `docs/01-architecture-map.md`.
Unknowns: None stated in sources.
