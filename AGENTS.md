# Repository Guidelines

## Project Structure & Module Organization
This repo is a .NET 8 modular monolith. `src/CompanyName.MyMeetings.sln` is the root solution. `src/API/CompanyName.MyMeetings.API` hosts the HTTP API. `src/Modules/<ModuleName>/` contains module subprojects (`Application`, `Domain`, `Infrastructure`, `IntegrationEvents`) and should remain encapsulated. `src/BuildingBlocks` holds shared cross-cutting code. `src/Database/` contains DbUp migrator and SQL scripts. `src/Tests` stores `ArchTests`, `IntegrationTests`, and `SUT`. `docs/` keeps architecture diagrams and reference material. `build/` contains NUKE build scripts. References: `docs/00-repo-inventory.md`, `docs/01-architecture-map.md`, `docs/02-module-catalog.md`.

## Non-Negotiable Modular Boundaries
Follow these module boundaries exactly (enforced via MSBuild rules and explicit references):
- Layering inside a module is fixed: `Domain` -> `Application` -> `Infrastructure`; `IntegrationEvents` is separate and referenced by `Application`. Reference: `src/Directory.Build.targets` (module layer project references) and `docs/03-dependency-rules.md`.
- Cross-module references must go through `IntegrationEvents` unless explicitly documented. Allowed explicit references are listed in `docs/03-dependency-rules.md` (e.g., `src/Modules/Registrations/Infrastructure/Users/UserAccessGateway.cs` calling `IUserAccessModule`).
- API depends on module `Infrastructure` only via the MSBuild rule; do not add direct API references to module `Domain` or `Application`. Reference: `src/Directory.Build.targets` and `docs/03-dependency-rules.md`.

## Build, Test, and Development Commands
- `./build.ps1 Compile` or `./build.sh Compile`: build the solution via NUKE (see `build/Build.cs`).
- `./build.ps1 UnitTests`: run unit tests (filtered by `UnitTests`, `build/Build.cs`).
- `./build.ps1 ArchitectureTests`: run architecture tests (filtered by `ArchTests`, `build/Build.cs`).
- `./build.ps1 RunAllIntegrationTests`: spin up SQL Server in Docker and run integration tests (`build/BuildIntegrationTests.cs`).
- `./build.ps1 MigrateDatabase --DatabaseConnectionString "..."`: apply DbUp migrations (see `README.md` “How to Run” and `build/Database.cs`).
- `dotnet run --project src/API/CompanyName.MyMeetings.API`: run the API locally after DB setup.
- `docker-compose up`: run the full stack (db + migrator + app), see `docker-compose.yml` and `docs/01-architecture-map.md`.

## Coding Style & Naming Conventions
Follow `.editorconfig` and `src/stylecop.json`; StyleCopAnalyzers are enabled. Keep formatting consistent with surrounding code (4-space C# indentation is the norm). Assemblies follow `CompanyName.MyMeetings.*` naming, and test projects use suffixes like `.UnitTests`, `.IntegrationTests`, and `.ArchTests`. Integration event contracts live only under `IntegrationEvents`. References: `src/.editorconfig`, `src/stylecop.json`, `docs/02-module-catalog.md`.

## Testing Guidelines
NUnit is the primary test framework with NSubstitute for isolation. Architecture tests use NetArchTest. Integration tests live in module-specific `*.IntegrationTests` projects and `src/Tests/IntegrationTests`. There is no explicit coverage threshold; prioritize business rules and critical paths. References: `src/Directory.Packages.props`, `docs/04-quality-gates.md`.

## Commit & Pull Request Guidelines
Commit subjects are short and imperative; Conventional Commit prefixes are common (e.g., `fix:`, `refactor(Meetings):`, `build(deps):`). Include issue/PR references when relevant, e.g., `(#321)`. PRs should describe behavior changes, link issues, list tests run, and call out database/migration or configuration changes.

## Security & Configuration
Store the DB connection string as `MeetingsConnectionString` in `appsettings.json` or user secrets; the API reads `appsettings.json`, `appsettings.{Environment}.json`, user secrets, and `Meetings_`-prefixed environment variables. Database scripts are under `src/Database/CompanyName.MyMeetings.Database/Scripts`. References: `src/API/CompanyName.MyMeetings.API/appsettings.json`, `src/API/CompanyName.MyMeetings.API/Startup.cs`, `docs/00-repo-inventory.md`.

## Workflow for Every Task
1. Locate the module and layer impacted using `docs/02-module-catalog.md` and `docs/03-dependency-rules.md` (respect Non-Negotiable Modular Boundaries).
2. Identify relevant build/test commands in `build/Build.cs`, `build/BuildIntegrationTests.cs`, and `docs/04-quality-gates.md`.
3. Make the smallest change that satisfies requirements and stays within allowed module references (see `src/Directory.Build.targets`).
4. Update/extend tests when behavior changes, following the test layout in `docs/02-module-catalog.md`.
5. Record any config implications using the known config sources (`src/API/CompanyName.MyMeetings.API/Startup.cs`, `src/API/CompanyName.MyMeetings.API/appsettings.json`).

## Quality Gates
- Local expectations mirror CI: `BuildAndUnitTests` and `RunAllIntegrationTests` are the two CI jobs (`.github/workflows/buildPipeline.yml`, `docs/04-quality-gates.md`).
- Treat warnings as errors and analyzers are on during build: `src/Directory.Build.props`.
- Integration tests require Docker and SQL Server; run `RunAllIntegrationTests` only when Docker is available (`build/BuildIntegrationTests.cs`).
