# Repository Guidelines

## Project Structure & Module Organization
This repo is a .NET 8 modular monolith. `src/CompanyName.MyMeetings.sln` is the root solution. `src/API/CompanyName.MyMeetings.API` hosts the HTTP API. `src/Modules/<ModuleName>/` contains module subprojects (`Application`, `Domain`, `Infrastructure`, `IntegrationEvents`) and should remain encapsulated. `src/BuildingBlocks` holds shared cross-cutting code. `src/Database/CompanyName.MyMeetings.Database` contains SQL scripts and migrations. `src/Tests` stores `ArchTests`, `IntegrationTests`, and `SUT`. `docs/` keeps architecture diagrams and reference material. `build/` contains NUKE build scripts.

## Build, Test, and Development Commands
- `./build.ps1 Compile` or `./build.sh Compile`: build the solution via NUKE.
- `./build.ps1 UnitTests`: run unit tests (filtered by `UnitTests`).
- `./build.ps1 ArchitectureTests`: run architecture tests (filtered by `ArchTests`).
- `./build.ps1 RunAllIntegrationTests`: spin up SQL Server in Docker and run integration tests.
- `./build.ps1 MigrateDatabase --DatabaseConnectionString "..."`: apply DbUp migrations.
- `dotnet run --project src/API/CompanyName.MyMeetings.API`: run the API locally after DB setup.
- `docker-compose up`: run the full stack (db + migrator + app).

## Coding Style & Naming Conventions
Follow `.editorconfig` and `src/stylecop.json`; StyleCopAnalyzers are enabled. Keep formatting consistent with surrounding code (4-space C# indentation is the norm). Assemblies follow `CompanyName.MyMeetings.*` naming, and test projects use suffixes like `.UnitTests`, `.IntegrationTests`, and `.ArchTests`. Integration event contracts live only under `IntegrationEvents`.

## Testing Guidelines
NUnit is the primary test framework with NSubstitute for isolation. Architecture tests use NetArchTest. Integration tests live in module-specific `*.IntegrationTests` projects and `src/Tests/IntegrationTests`. There is no explicit coverage threshold; prioritize business rules and critical paths.

## Commit & Pull Request Guidelines
Commit subjects are short and imperative; Conventional Commit prefixes are common (e.g., `fix:`, `refactor(Meetings):`, `build(deps):`). Include issue/PR references when relevant, e.g., `(#321)`. PRs should describe behavior changes, link issues, list tests run, and call out database/migration or configuration changes.

## Security & Configuration
Store the DB connection string as `MeetingsConnectionString` in `appsettings.json` or user secrets. Database scripts and seeds are under `src/Database/CompanyName.MyMeetings.Database/Scripts`.
