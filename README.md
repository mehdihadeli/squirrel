# 🐿️ Squirrel

<p align="center">
  <img src="assets/squirrel.png" alt="Squirrel package brand" width="320">
</p>

Reusable infrastructure building blocks for .NET microservices.

Squirrel is a collection of focused NuGet packages for persistence, messaging, authentication, resilience, health checks, logging, tracing, validation, and caching. It gives services shared infrastructure conventions while keeping application code on standard .NET hosting, dependency injection, configuration, and middleware patterns.

Install the packages your service needs and compose their registrations. Squirrel is a toolkit, not an application framework that controls your architecture.

[![build-test-publish](https://img.shields.io/github/actions/workflow/status/mehdihadeli/squirrel/build-and-publish.yml?branch=main&style=for-the-badge&label=build-test-publish)](https://github.com/mehdihadeli/squirrel/actions/workflows/build-and-publish.yml)
[![License](https://img.shields.io/github/license/mehdihadeli/squirrel?style=for-the-badge)](LICENSE)
[![.NET 10](https://img.shields.io/badge/.NET%2010-512bd4?style=for-the-badge)](https://dotnet.microsoft.com/platform/dotnet)
[![NuGet](https://img.shields.io/nuget/v/Squirrel.CoreKit?style=for-the-badge&color=brightgreen)](https://www.nuget.org/packages/Squirrel.CoreKit)
[![Documentation](https://img.shields.io/badge/docs-component_guides-0A7EA4?style=for-the-badge)](docs/)

[Quick Start](#quick-start) · [Features](#features) · [Packages](#packages) · [Architecture](#architecture) · [Documentation](#documentation)

> [!IMPORTANT]
> All packages target `net10.0`. Use the .NET 10 SDK and install only the packages required by your service.

## 🔄 How It Works

1. **Choose** packages for your service's infrastructure needs.
2. **Configure** connection strings and options through standard .NET configuration providers.
3. **Register** services with the package's hosting or dependency-injection extensions.
4. **Run** the application with consistent persistence, messaging, HTTP, and observability conventions.

## Quick Start

From an ASP.NET Core project, install the core, web, observability, health-check, resilience, and validation packages:

```bash
dotnet add package Squirrel.CoreKit
dotnet add package Squirrel.Web
dotnet add package Squirrel.OpenTelemetry
dotnet add package Squirrel.HealthCheck
dotnet add package Squirrel.Resiliency
dotnet add package Squirrel.Validation
```

Add the registrations to your application's startup code, using the namespaces shown in the [quickstart guide](docs/guide/quickstart.md):

```csharp
var builder = WebApplication.CreateBuilder(args);

builder.AddCoreServices();
builder.AddCustomResiliency();
builder.AddDefaultOpenTelemetry();
builder.AddDefaultHealthChecks();
builder.AddCustomProblemDetails(new[] { typeof(Program).Assembly });
builder.Services.AddCustomValidators(typeof(Program).Assembly);

var app = builder.Build();
app.MapDefaultHealthChecks();
app.Run();
```

> [!TIP]
> Start with the capabilities your service uses. Add persistence, messaging, caching, or authentication separately as needed.

## Package Selection

Use the smallest package set that matches your service. Packages are independently installable and share conventions through `Squirrel.Abstractions`.

| Service need | Start with | Add when needed |
| --- | --- | --- |
| HTTP API | `Squirrel.Web`, `Squirrel.Validation`, `Squirrel.HealthCheck` | `Squirrel.Security`, `Squirrel.OpenApi` |
| Application and domain logic | `Squirrel.Abstractions`, `Squirrel.CoreKit` | `Squirrel.Validation` |
| Database-backed service | `Squirrel.Persistence.EfCore.Postgres` | `Squirrel.Persistence.Marten`, `Squirrel.Persistence.Mongo`, or `Squirrel.Persistence.EventStoreDB` |
| Messaging | `Squirrel.Integration.Wolverine.InMemory` | `Squirrel.Integration.Wolverine` for RabbitMQ |
| Production operations | `Squirrel.OpenTelemetry`, `Squirrel.SerilogLogging`, `Squirrel.Resiliency` | `Squirrel.AspireIntegrations` for local orchestration |

Install only the rows your service uses. Each package's component guide documents its required configuration, registrations, and optional integrations.

## 🧰 Common Usage

### PostgreSQL and RabbitMQ

Add PostgreSQL persistence and RabbitMQ messaging:

```bash
dotnet add package Squirrel.Persistence.EfCore.Postgres
dotnet add package Squirrel.Integration.Wolverine
```

Register your database context and message handlers:

```csharp
builder.AddPostgresDbContext<AppDbContext>("catalogdb");
builder.AddWolverineEventBus(
  assemblies: [typeof(OrderPlacedHandler).Assembly]);
```

`AppDbContext` and `OrderPlacedHandler` are application-defined types. See the [PostgreSQL guide](docs/components/persistence-efcore-postgres.md) and [Wolverine guide](docs/components/wolverine.md) for complete setup.

## Features

- ✅ **CQRS and domain building blocks** with command/query buses, aggregates, value objects, business rules, domain events, and request pipelines.
- ✅ **Database persistence and event sourcing** with PostgreSQL EF Core, MongoDB, Marten, EventStoreDB, repositories, unit of work, and persistence interceptors.
- ✅ **Durable and in-process messaging** through Wolverine and RabbitMQ, with message topology, envelopes, and optional PostgreSQL-backed local queues.
- ✅ **Consistent HTTP APIs** with minimal endpoints, module composition, CORS, compression, versioning, rate limiting, and Problem Details.
- ✅ **Authentication and request validation** with JWT, API keys, claim/role policies, and FluentValidation discovery and pipeline behaviors.
- ✅ **API and message documentation** through Swagger, ASP.NET OpenAPI, Scalar, versioned API documentation, and AsyncAPI helpers.
- ✅ **Hybrid caching and Redis coordination** with query caching, invalidation, publish/subscribe, distributed locking, and MessagePack cache serialization.
- ✅ **Resilience, health, and observability** with service discovery, retries, circuit breakers, health endpoints, OpenTelemetry, and enriched Serilog logs.
- ✅ **Serialization and email delivery** with MemoryPack binary serializers, core JSON serializers, and MailKit or SendGrid email providers.
- ✅ **Aspire development infrastructure** for databases, caches, messaging, search, dashboards, and tracing, metrics, and logging tools.

## Packages

Each package can be installed independently. Use the short descriptions to find the right capability, then select the blue NuGet badge for the latest stable version and package page.

| Package | Best for | NuGet |
| --- | --- | --- |
| `Squirrel.Abstractions` | Shared contracts and models for application boundaries. | [![Squirrel.Abstractions](https://img.shields.io/nuget/v/Squirrel.Abstractions?style=for-the-badge&label=Squirrel.Abstractions&color=brightgreen)](https://www.nuget.org/packages/Squirrel.Abstractions) |
| `Squirrel.CoreKit` | Domain services, buses, options, exceptions, and core registrations. | [![Squirrel.CoreKit](https://img.shields.io/nuget/v/Squirrel.CoreKit?style=for-the-badge&label=Squirrel.CoreKit&color=brightgreen)](https://www.nuget.org/packages/Squirrel.CoreKit) |
| `Squirrel.Web` | ASP.NET Core API conventions and endpoint composition. | [![Squirrel.Web](https://img.shields.io/nuget/v/Squirrel.Web?style=for-the-badge&label=Squirrel.Web&color=brightgreen)](https://www.nuget.org/packages/Squirrel.Web) |
| `Squirrel.Security` | JWT bearer and API-key authentication. | [![Squirrel.Security](https://img.shields.io/nuget/v/Squirrel.Security?style=for-the-badge&label=Squirrel.Security&color=brightgreen)](https://www.nuget.org/packages/Squirrel.Security) |
| `Squirrel.HealthCheck` | Health-check registration and endpoint conventions. | [![Squirrel.HealthCheck](https://img.shields.io/nuget/v/Squirrel.HealthCheck?style=for-the-badge&label=Squirrel.HealthCheck&color=brightgreen)](https://www.nuget.org/packages/Squirrel.HealthCheck) |
| `Squirrel.Resiliency` | Service discovery, retries, timeouts, and circuit breakers. | [![Squirrel.Resiliency](https://img.shields.io/nuget/v/Squirrel.Resiliency?style=for-the-badge&label=Squirrel.Resiliency&color=brightgreen)](https://www.nuget.org/packages/Squirrel.Resiliency) |
| `Squirrel.Validation` | FluentValidation discovery and dependency injection. | [![Squirrel.Validation](https://img.shields.io/nuget/v/Squirrel.Validation?style=for-the-badge&label=Squirrel.Validation&color=brightgreen)](https://www.nuget.org/packages/Squirrel.Validation) |
| `Squirrel.Email` | Email delivery abstraction and provider integrations. | [![Squirrel.Email](https://img.shields.io/nuget/v/Squirrel.Email?style=for-the-badge&label=Squirrel.Email&color=brightgreen)](https://www.nuget.org/packages/Squirrel.Email) |
| `Squirrel.Caching` | Cache abstractions, hybrid caching, and invalidation. | [![Squirrel.Caching](https://img.shields.io/nuget/v/Squirrel.Caching?style=for-the-badge&label=Squirrel.Caching&color=brightgreen)](https://www.nuget.org/packages/Squirrel.Caching) |
| `Squirrel.Caching.AzureRedis` | Azure Redis hosting and client integration. | [![Squirrel.Caching.AzureRedis](https://img.shields.io/nuget/v/Squirrel.Caching.AzureRedis?style=for-the-badge&label=Squirrel.Caching.AzureRedis&color=brightgreen)](https://www.nuget.org/packages/Squirrel.Caching.AzureRedis) |
| `Squirrel.Serialization` | MemoryPack serializer registration. | [![Squirrel.Serialization](https://img.shields.io/nuget/v/Squirrel.Serialization?style=for-the-badge&label=Squirrel.Serialization&color=brightgreen)](https://www.nuget.org/packages/Squirrel.Serialization) |
| `Squirrel.Integration.Wolverine` | Wolverine messaging and RabbitMQ integration. | [![Squirrel.Integration.Wolverine](https://img.shields.io/nuget/v/Squirrel.Integration.Wolverine?style=for-the-badge&label=Squirrel.Integration.Wolverine&color=brightgreen)](https://www.nuget.org/packages/Squirrel.Integration.Wolverine) |
| `Squirrel.Integration.Wolverine.InMemory` | In-process Wolverine bus and durable local queues. | [![Squirrel.Integration.Wolverine.InMemory](https://img.shields.io/nuget/v/Squirrel.Integration.Wolverine.InMemory?style=for-the-badge&label=Squirrel.Integration.Wolverine.InMemory&color=brightgreen)](https://www.nuget.org/packages/Squirrel.Integration.Wolverine.InMemory) |
| `Squirrel.Persistence.EfCore.Postgres` | PostgreSQL EF Core persistence and repositories. | [![Squirrel.Persistence.EfCore.Postgres](https://img.shields.io/nuget/v/Squirrel.Persistence.EfCore.Postgres?style=for-the-badge&label=Squirrel.Persistence.EfCore.Postgres&color=brightgreen)](https://www.nuget.org/packages/Squirrel.Persistence.EfCore.Postgres) |
| `Squirrel.Persistence.EfCore.AzurePostgres` | Reserved Azure PostgreSQL package placeholder. | [![Squirrel.Persistence.EfCore.AzurePostgres](https://img.shields.io/nuget/v/Squirrel.Persistence.EfCore.AzurePostgres?style=for-the-badge&label=Squirrel.Persistence.EfCore.AzurePostgres&color=brightgreen)](https://www.nuget.org/packages/Squirrel.Persistence.EfCore.AzurePostgres) |
| `Squirrel.Persistence.EfCore.AzureCosmosDB` | Reserved Azure Cosmos DB package placeholder. | [![Squirrel.Persistence.EfCore.AzureCosmosDB](https://img.shields.io/nuget/v/Squirrel.Persistence.EfCore.AzureCosmosDB?style=for-the-badge&label=Squirrel.Persistence.EfCore.AzureCosmosDB&color=brightgreen)](https://www.nuget.org/packages/Squirrel.Persistence.EfCore.AzureCosmosDB) |
| `Squirrel.Persistence.Mongo` | MongoDB context, repositories, and health checks. | [![Squirrel.Persistence.Mongo](https://img.shields.io/nuget/v/Squirrel.Persistence.Mongo?style=for-the-badge&label=Squirrel.Persistence.Mongo&color=brightgreen)](https://www.nuget.org/packages/Squirrel.Persistence.Mongo) |
| `Squirrel.Persistence.Marten` | Marten documents, event streams, and projections. | [![Squirrel.Persistence.Marten](https://img.shields.io/nuget/v/Squirrel.Persistence.Marten?style=for-the-badge&label=Squirrel.Persistence.Marten&color=brightgreen)](https://www.nuget.org/packages/Squirrel.Persistence.Marten) |
| `Squirrel.Persistence.EventStoreDB` | EventStoreDB client, subscriptions, and checkpoints. | [![Squirrel.Persistence.EventStoreDB](https://img.shields.io/nuget/v/Squirrel.Persistence.EventStoreDB?style=for-the-badge&label=Squirrel.Persistence.EventStoreDB&color=brightgreen)](https://www.nuget.org/packages/Squirrel.Persistence.EventStoreDB) |
| `Squirrel.OpenTelemetry` | OpenTelemetry tracing, metrics, logging, and exporters. | [![Squirrel.OpenTelemetry](https://img.shields.io/nuget/v/Squirrel.OpenTelemetry?style=for-the-badge&label=Squirrel.OpenTelemetry&color=brightgreen)](https://www.nuget.org/packages/Squirrel.OpenTelemetry) |
| `Squirrel.OpenApi` | Swagger, Scalar, API versioning, and AsyncAPI support. | [![Squirrel.OpenApi](https://img.shields.io/nuget/v/Squirrel.OpenApi?style=for-the-badge&label=Squirrel.OpenApi&color=brightgreen)](https://www.nuget.org/packages/Squirrel.OpenApi) |
| `Squirrel.SerilogLogging` | Serilog setup, enrichment, correlation, and sinks. | [![Squirrel.SerilogLogging](https://img.shields.io/nuget/v/Squirrel.SerilogLogging?style=for-the-badge&label=Squirrel.SerilogLogging&color=brightgreen)](https://www.nuget.org/packages/Squirrel.SerilogLogging) |
| `Squirrel.AspireIntegrations` | Aspire resources for infrastructure and observability tools. | [![Squirrel.AspireIntegrations](https://img.shields.io/nuget/v/Squirrel.AspireIntegrations?style=for-the-badge&label=Squirrel.AspireIntegrations&color=brightgreen)](https://www.nuget.org/packages/Squirrel.AspireIntegrations) |

See the [component documentation](docs/components/) and [package reference](docs/reference/packages.md) for detailed setup instructions. Package versions are released together and managed with Nerdbank.GitVersioning.

## Architecture

The package family separates contracts, reusable application code, and technology-specific integrations. Your service owns its domain, endpoints, handlers, and choice of infrastructure.

| Area | Responsibility |
| --- | --- |
| Contracts | `Squirrel.Abstractions` defines shared interfaces and models without requiring infrastructure implementations. |
| Application building blocks | `Squirrel` supplies domain types, buses, event handling, pipelines, persistence helpers, and shared services. |
| HTTP and platform behavior | Web, security, validation, health-check, and resilience packages add service-facing conventions. |
| Infrastructure adapters | Persistence, Wolverine, caching, email, and serialization packages connect application code to selected technologies. |
| Observability | OpenTelemetry and Serilog packages register instrumentation, exporters, logging, and context enrichment. |
| Development orchestration | `Squirrel.AspireIntegrations` supplies AppHost resources for dependencies and observability tools. |

The [architecture guide](docs/guide/architecture.md) explains the project structure and dependency boundaries.

## Documentation

| Guide | What you will find |
| --- | --- |
| [Quickstart](docs/guide/quickstart.md) | Package installation and registrations for a typical service. |
| [Architecture](docs/guide/architecture.md) | Project structure, shared contracts, and infrastructure boundaries. |
| [Package Reference](docs/reference/packages.md) | Package responsibilities and links to component guides. |
| [Component Guides](docs/components/) | Setup examples and provider-specific integration details. |

## License

Squirrel is licensed under the [MIT License](LICENSE).
