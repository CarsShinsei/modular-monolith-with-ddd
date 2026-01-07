# Quality Gates

## Build and Analyzer Gates
- Build uses NUKE targets defined in `build/Build.cs`.
- Code analysis settings are enforced in `src/Directory.Build.props`:
  - `TreatWarningsAsErrors=true`.
  - `RunAnalyzersDuringBuild=true` and `EnableNETAnalyzers=true`.
- StyleCop analyzers are enforced via `src/Directory.Packages.props` and configured in `src/stylecop.json`.

## Test Gates
- Unit tests and architecture tests are explicit NUKE targets:
  - `UnitTests` target filters tests with `UnitTests` (`build/Build.cs`).
  - `ArchitectureTests` target filters tests with `ArchTests` (`build/Build.cs`).
- Integration tests run via NUKE:
  - `RunAllIntegrationTests` target (`build/BuildIntegrationTests.cs`).
- Windows integration test script is available in `runIntegrationTests.cmd`.

## CI Gates
- GitHub Actions (`.github/workflows/buildPipeline.yml`):
  - `BuildAndUnitTests` on push/PR to `master`.
  - `RunAllIntegrationTests` after the build job.
- Azure Pipelines (`azure-pipelines.yml`):
  - NuGet restore, VSBuild, VSTest for the solution.

## Unknowns
- Code coverage thresholds: UNKNOWN. Searched `.github/workflows/buildPipeline.yml`, `azure-pipelines.yml`, `build/Build.cs`, `build/BuildIntegrationTests.cs`, `build.ps1`, `build.sh`, `src/Directory.Build.props`, `src/Directory.Build.targets`, `src/Directory.Packages.props`, and `src/**/*.csproj`; no coverage tooling or threshold configuration was found.
