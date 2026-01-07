# Repo Inventory

## Top-Level Layout
- `.github/workflows/buildPipeline.yml`: GitHub Actions CI pipeline.
- `.nuke/` and `build/`: NUKE build system configuration and targets.
- `docs/`: architecture diagrams and documentation assets.
- `src/`: main solution, API, modules, building blocks, database scripts, and tests.
- `docker-compose.yml`, `Dockerfile`, `entrypoint.sh`: container and local runtime wiring.
- `build.ps1`, `build.sh`, `build.cmd`: build entry points.
- `runIntegrationTests.cmd`: Windows integration test runner.

## Solutions and Projects
- Solution: `src/CompanyName.MyMeetings.sln`.
- API project: `src/API/CompanyName.MyMeetings.API/CompanyName.MyMeetings.API.csproj`.
- Modules: `src/Modules/<ModuleName>/` (Administration, Meetings, Payments, Registrations, UserAccess).
- Building blocks: `src/BuildingBlocks/`.
- Database tools: `src/Database/` (DbUp migrator and SQL scripts).
- Tests: `src/Tests/` plus module-specific tests under `src/Modules/*/Tests/`.

## Key Configuration Files
- `src/Directory.Build.props`: build defaults and analyzer settings.
- `src/Directory.Build.targets`: shared project reference rules.
- `src/Directory.Packages.props`: shared package versions.
- `src/.editorconfig`, `src/stylecop.json`: formatting and StyleCop settings.

## Runtime Hosting and Environments (Repo-Defined)
- Local container topology is defined in `docker-compose.yml` with services `backend`, `mymeetingsdb`, and `migrator`, plus the network `starfish-crm-network`.
- Container env var keys for runtime configuration are declared in `docker-compose.yml`, including `Meetings_MeetingsConnectionString` for the backend and `ASPNETCORE_MyMeetings_IntegrationTests_ConnectionString` for the migrator.
- Environment-specific settings files exist for Development and Production: `src/API/CompanyName.MyMeetings.API/appsettings.Development.json` and `src/API/CompanyName.MyMeetings.API/appsettings.Production.json`.
- The API loads config from `appsettings.json`, `appsettings.{Environment}.json`, user secrets, and environment variables with the `Meetings_` prefix in `src/API/CompanyName.MyMeetings.API/Startup.cs` (ConfigurationBuilder setup).
