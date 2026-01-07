# Architecture Map

## High-Level View
- The system is a modular monolith with a thin HTTP API and internal modules. Source: `README.md` (Architecture sections 3.1 and 3.2).
- Modules include Administration, Meetings, Payments, Registrations, and UserAccess. Source: `src/Modules/`.
- Each module is split into `Application`, `Domain`, `Infrastructure`, and `IntegrationEvents` projects. Source: `src/Modules/<ModuleName>/` and `src/Directory.Build.targets`.
- Integration between modules uses events via IntegrationEvents contracts; direct cross-module calls are limited. Source: `README.md` (3.1 High Level View, 3.2 Module Level View) and project references in module `*.Application.csproj` files.

## API Boundary
- API project: `src/API/CompanyName.MyMeetings.API/CompanyName.MyMeetings.API.csproj`.
- API references all module Infrastructure projects via MSBuild conditions. Source: `src/Directory.Build.targets`.

## Runtime Topology (Repo-Defined)
- Local/container topology: `docker-compose.yml` defines `backend` (API), `mymeetingsdb` (SQL Server), and `migrator` (DbUp) services.
- The API container runs `CompanyName.MyMeetings.API.dll` via `src/entrypoint.sh` and is built from `src/Dockerfile`.
- Config sources and environment-specific settings: `src/API/CompanyName.MyMeetings.API/Startup.cs` loads `appsettings.json`, `appsettings.{Environment}.json`, user secrets, and env vars prefixed with `Meetings_`.

## Diagrams and References
- C4 diagrams and architecture visuals: `docs/C4/` and `docs/Images/`.
- Solution-level diagram reference: `docs/Images/VSSolution.png`.
