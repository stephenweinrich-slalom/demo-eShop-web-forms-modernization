# eShop Modernized Web Forms

A demo ASP.NET Web Forms application used as a starting point for .NET modernization exercises. It is based on the Microsoft `eShopModernizing` sample and implements a product catalog (browse, create, edit, delete) backed by Entity Framework 6 and SQL Server, with optional Azure integrations.

> This repository is intended for demos and experimentation with modernization tooling (e.g. GitHub Copilot app modernization, containerization, and migration to .NET).

## Tech stack

| Area | Technology |
| --- | --- |
| Framework | .NET Framework 4.7.2, ASP.NET Web Forms |
| Data access | Entity Framework 6, SQL Server |
| DI | Autofac |
| Logging | log4net (file + optional Azure appenders) |
| Telemetry | Azure Application Insights |
| Storage | Local `Pics` folder or Azure Blob Storage |
| Auth | Optional Azure Active Directory (OpenID Connect via OWIN) |
| Config | `Microsoft.Configuration.ConfigurationBuilders` (environment, user secrets, Key Vault) |
| Containers | Windows containers (`mcr.microsoft.com/dotnet/framework/aspnet:4.7.2`) |

## Repository layout

```
eShopModernizedWebForms.sln        Visual Studio solution
docker-compose*.yml                Windows container composition (app + SQL Server)
docker-compose.dcproj              Docker Compose project for Visual Studio
.env                               Compose variable defaults (placeholders only)
src/eShopModernizedWebForms/
  App_Start/                       Bundles, routes, auth, catalog + Key Vault configuration
  Catalog/                         Catalog pages (Create, Edit, Details, Delete) + image upload service
  Content/ Scripts/ fonts/ images/ Static assets (Bootstrap, jQuery, site CSS)
  Middleware/                      OWIN authentication middleware
  Models/                          CatalogItem, CatalogBrand, CatalogType, EF DbContext, seeding
  Modules/                         Autofac module registrations
  Services/                        Catalog + image services (real and mock implementations)
  ViewModel/                       Page view models
  Pics/                            Sample catalog images
  Web.config                       Application settings and connection strings
```

## Prerequisites

- Windows 10/11 or Windows Server
- Visual Studio 2019 or later with the **ASP.NET and web development** workload
- .NET Framework 4.7.2 Developer Pack
- SQL Server (LocalDB, Express, or a container) — only if not using mock data
- Docker Desktop configured for **Windows containers** (optional, for the Compose workflow)

## Getting started

### Run in Visual Studio (IIS Express)

1. Open `eShopModernizedWebForms.sln`.
2. Restore NuGet packages (`Tools > NuGet Package Manager > Restore`).
3. Set `eShopModernizedWebForms` as the startup project and press <kbd>F5</kbd>.

By default the app can run against in-memory mock data, so no database is required for a first run. Switch data sources in `src/eShopModernizedWebForms/Web.config`:

```xml
<add key="UseMockData" value="true" />
<add key="UseCustomizationData" value="false" />
```

Set `UseMockData` to `false` and point `CatalogDBContext` at a reachable SQL Server instance to use a real database. The schema and seed data are created automatically on first run.

### Run with Docker Compose (Windows containers)

```powershell
docker-compose build
docker-compose up
```

The app is published to <http://localhost:5114> and SQL Server is exposed on port `5433`.

To run prebuilt images without rebuilding:

```powershell
docker-compose -f docker-compose.yml -f docker-compose.nobuild.yml up
```

## Configuration

Key settings live in `Web.config` (`appSettings`) and can be overridden by environment variables through the configuration builders, which is how `docker-compose.override.yml` supplies values.

| Setting | Description |
| --- | --- |
| `CatalogDBContext` (connection string) | SQL Server connection string for the catalog database |
| `UseMockData` | Use in-memory catalog data instead of SQL Server |
| `UseCustomizationData` | Load customized seed data from CSV files in `Setup/` |
| `UseAzureStorage` | Serve catalog images from Azure Blob Storage instead of local files |
| `StorageConnectionString` | Azure Storage connection string used when `UseAzureStorage` is `true` |
| `AppInsightsInstrumentationKey` | Application Insights instrumentation key |
| `UseAzureActiveDirectory` | Enable Azure AD sign-in |
| `AzureActiveDirectoryClientId` / `AzureActiveDirectoryTenant` | Azure AD application registration details |

Do not commit real connection strings, keys, or secrets. Supply them through environment variables, user secrets, or Azure Key Vault (see `App_Start/OptionalKeyVaultConfigurationBuilder.cs`).

## Credits

Derived from the [dotnet-architecture/eShopModernizing](https://github.com/dotnet-architecture/eShopModernizing) reference sample from Microsoft.
