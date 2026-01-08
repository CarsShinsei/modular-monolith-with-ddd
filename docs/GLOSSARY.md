# Glossary

## Modular Monolith
A single system composed of internal modules, with a thin HTTP API and module boundaries defined in the repo.
Sources: `docs/01-architecture-map.md`, `AGENTS.md`.
Unknowns: None stated in sources.

## Module
A bounded code area under `src/Modules/<ModuleName>/` with `Application`, `Domain`, `Infrastructure`, and `IntegrationEvents` projects.
Sources: `docs/02-module-catalog.md`, `docs/01-architecture-map.md`.
Unknowns: None stated in sources.

## IntegrationEvents
A module project that contains integration event contracts and is used for cross-module references.
Sources: `docs/01-architecture-map.md`, `docs/03-dependency-rules.md`.
Unknowns: None stated in sources.

## BuildingBlocks
Shared cross-cutting code under `src/BuildingBlocks/`.
Sources: `docs/00-repo-inventory.md`, `docs/02-module-catalog.md`.
Unknowns: None stated in sources.

## NUKE Build
The build system used for compile and test targets under `build/`.
Sources: `docs/00-repo-inventory.md`, `docs/04-quality-gates.md`.
Unknowns: None stated in sources.

## SUT
System-under-test project under `src/Tests/SUT/`.
Sources: `docs/02-module-catalog.md`.
Unknowns: None stated in sources.
