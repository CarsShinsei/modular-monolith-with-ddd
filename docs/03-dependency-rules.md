# Dependency Rules

## Shared MSBuild Rules
These are applied via `src/Directory.Build.targets`:
- API projects reference all module Infrastructure projects.
  - Rule: `$(MSBuildProjectName.EndsWith('API'))` -> `..\..\Modules\**\Infrastructure\*.csproj`.
- Module layers:
  - `Domain` projects reference `BuildingBlocks.Domain`.
  - `IntegrationEvents` projects reference `BuildingBlocks.Infrastructure`.
  - `Application` projects reference their own `Domain` and `IntegrationEvents`.
  - `Infrastructure` projects reference their own `Application`.
- Building blocks:
  - `BuildingBlocks.Application` references `BuildingBlocks.Domain`.
  - `BuildingBlocks.Infrastructure` and `BuildingBlocks.*.UnitTests` reference `BuildingBlocks.Application`.
- Test projects:
  - `src/Tests/**` references API and `BuildingBlocks.Tests.IntegrationTests`.
  - `src/Modules/*/Tests/**` references the module Infrastructure and `BuildingBlocks.Tests.IntegrationTests`.

## Explicit Cross-Module References
These references are declared directly in project files:
- Administration Application depends on Meetings, Registrations, and UserAccess IntegrationEvents.
  - `src/Modules/Administration/Application/CompanyName.MyMeetings.Modules.Administration.Application.csproj`.
- Meetings Application depends on Administration, Payments, Registrations, and UserAccess IntegrationEvents.
  - `src/Modules/Meetings/Application/CompanyName.MyMeetings.Modules.Meetings.Application.csproj`.
- Payments Application depends on Administration, Meetings, Registrations, and UserAccess IntegrationEvents.
  - `src/Modules/Payments/Application/CompanyName.MyMeetings.Modules.Payments.Application.csproj`.
- UserAccess Application depends on Meetings IntegrationEvents.
  - `src/Modules/UserAccess/Application/CompanyName.MyMeetings.Modules.UserAccess.Application.csproj`.
- Registrations Infrastructure depends on UserAccess Application and Infrastructure.
  - `src/Modules/Registrations/Infrastructure/CompanyName.MyMeetings.Modules.Registrations.Infrastructure.csproj`.

## Runtime Cross-Module Calls Outside IntegrationEvents
- Registrations Infrastructure directly calls the UserAccess module via `IUserAccessModule.ExecuteCommandAsync` to create users in `src/Modules/Registrations/Infrastructure/Users/UserAccessGateway.cs` (method `Create`, uses `CreateUserCommand`).
