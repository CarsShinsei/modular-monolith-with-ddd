# Technical Specification (AS-IS)

## Codebase Structure
- Solution: `src/CompanyName.MyMeetings.sln`.
- API: `src/API/CompanyName.MyMeetings.API/CompanyName.MyMeetings.API.csproj`.
- Modules: `src/Modules/<ModuleName>/` with `Application`, `Domain`, `Infrastructure`, and `IntegrationEvents` projects.
- Building blocks: `src/BuildingBlocks/`.
- Tests: `src/Tests/` and module-specific tests under `src/Modules/*/Tests/`.
Sources: `docs/00-repo-inventory.md`, `docs/02-module-catalog.md`, `AGENTS.md`.
Unknowns: None stated in sources.

## Dependency Rules
MSBuild rules enforce module layering and API-to-infrastructure references. Cross-module references are limited to IntegrationEvents except for explicitly documented cases.
Sources: `docs/03-dependency-rules.md`, `AGENTS.md`.
Unknowns: None stated in sources.

## Runtime and Configuration
Container topology is defined in `docker-compose.yml` with `backend`, `mymeetingsdb`, and `migrator` services. The API loads config from `appsettings.json`, `appsettings.{Environment}.json`, user secrets, and `Meetings_`-prefixed environment variables.
Sources: `docs/00-repo-inventory.md`, `docs/01-architecture-map.md`, `AGENTS.md`.
Unknowns: None stated in sources.

## Build and Test
NUKE build targets include compile, unit tests, architecture tests, and integration tests. CI runs `BuildAndUnitTests` and `RunAllIntegrationTests` in GitHub Actions, with Azure Pipelines also defined.
Sources: `docs/04-quality-gates.md`, `AGENTS.md`.
Unknowns: None stated in sources.
