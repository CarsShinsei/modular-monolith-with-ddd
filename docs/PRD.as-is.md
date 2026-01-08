# Product Requirements Document (AS-IS)

## Purpose and Scope
This repository contains a modular monolith system with a thin HTTP API and five internal modules: Administration, Meetings, Payments, Registrations, and UserAccess. The scope is defined by these modules and the API host.
Sources: `docs/01-architecture-map.md`, `docs/02-module-catalog.md`, `AGENTS.md`.
Unknowns: The business purpose and user-facing goals are not specified in the source docs.

## Users and Stakeholders
Unknowns: No stakeholder or user personas are described in the source docs.
Sources: `docs/00-repo-inventory.md`, `docs/01-architecture-map.md`, `docs/02-module-catalog.md`, `AGENTS.md`.

## System Interfaces
The system exposes an HTTP API implemented in `src/API/CompanyName.MyMeetings.API`. There is a containerized runtime topology defined by `docker-compose.yml`.
Sources: `docs/01-architecture-map.md`, `docs/00-repo-inventory.md`.
Unknowns: Supported endpoints, request/response contracts, and external integrations are not documented in the source docs.

## Quality Requirements (As Enforced)
Build and test quality gates are defined via NUKE targets and CI workflows, with analyzers enabled and warnings treated as errors.
Sources: `docs/04-quality-gates.md`, `AGENTS.md`.
Unknowns: Explicit performance, reliability, or security requirements are not stated in the source docs.
