# 🐿️ Squirrel

<p align="center">
  <img src="assets/squirrel.png" alt="Squirrel package brand" width="320">
</p>

Reusable infrastructure building blocks for .NET microservices.

Squirrel is a collection of focused NuGet packages for persistence, messaging, authentication, resilience, health checks, logging, tracing, validation, and caching. It gives services shared infrastructure conventions while keeping application code on standard .NET hosting, dependency injection, configuration, and middleware patterns.

Install the packages your service needs and compose their registrations. Squirrel is a toolkit, not an application framework that controls your architecture.

<p>
  <a href="https://www.nuget.org/packages?q=Squirrel"><img src="https://img.shields.io/nuget/v/Squirrel.CoreKit.svg?style=flat" alt="NuGet version" height="28"></a>
  <a href="https://github.com/mehdihadeli/squirrel"><img src="https://img.shields.io/badge/.NET-10.0-512BD4?style=flat" alt=".NET 10" height="28"></a>
  <a href="docs/"><img src="https://img.shields.io/badge/docs-component_guides-0A7EA4?style=flat" alt="Component guides" height="28"></a>
</p>

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
| `Squirrel.Abstractions` | Shared contracts and models for application boundaries. | <a href="https://www.nuget.org/packages/Squirrel.Abstractions/"><img src="https://img.shields.io/nuget/v/Squirrel.Abstractions.svg?style=flat&color=0078D4" alt="NuGet Squirrel.Abstractions" height="28"></a> |
| `Squirrel.CoreKit` | Domain services, buses, options, exceptions, and core registrations. | <a href="https://www.nuget.org/packages/Squirrel.CoreKit/"><img src="https://img.shields.io/nuget/v/Squirrel.CoreKit.svg?style=flat&color=0078D4" alt="NuGet Squirrel.CoreKit" height="28"></a> |
| `Squirrel.Web` | ASP.NET Core API conventions and endpoint composition. | <a href="https://www.nuget.org/packages/Squirrel.Web/"><img src="https://img.shields.io/nuget/v/Squirrel.Web.svg?style=flat&color=0078D4" alt="NuGet Squirrel.Web" height="28"></a> |
| `Squirrel.Security` | JWT bearer and API-key authentication. | <a href="https://www.nuget.org/packages/Squirrel.Security/"><img src="https://img.shields.io/nuget/v/Squirrel.Security.svg?style=flat&color=0078D4" alt="NuGet Squirrel.Security" height="28"></a> |
| `Squirrel.HealthCheck` | Health-check registration and endpoint conventions. | <a href="https://www.nuget.org/packages/Squirrel.HealthCheck/"><img src="https://img.shields.io/nuget/v/Squirrel.HealthCheck.svg?style=flat&color=0078D4" alt="NuGet Squirrel.HealthCheck" height="28"></a> |
| `Squirrel.Resiliency` | Service discovery, retries, timeouts, and circuit breakers. | <a href="https://www.nuget.org/packages/Squirrel.Resiliency/"><img src="https://img.shields.io/nuget/v/Squirrel.Resiliency.svg?style=flat&color=0078D4" alt="NuGet Squirrel.Resiliency" height="28"></a> |
| `Squirrel.Validation` | FluentValidation discovery and dependency injection. | <a href="https://www.nuget.org/packages/Squirrel.Validation/"><img src="https://img.shields.io/nuget/v/Squirrel.Validation.svg?style=flat&color=0078D4" alt="NuGet Squirrel.Validation" height="28"></a> |
| `Squirrel.Email` | Email delivery abstraction and provider integrations. | <a href="https://www.nuget.org/packages/Squirrel.Email/"><img src="https://img.shields.io/nuget/v/Squirrel.Email.svg?style=flat&color=0078D4" alt="NuGet Squirrel.Email" height="28"></a> |
| `Squirrel.Caching` | Cache abstractions, hybrid caching, and invalidation. | <a href="https://www.nuget.org/packages/Squirrel.Caching/"><img src="https://img.shields.io/nuget/v/Squirrel.Caching.svg?style=flat&color=0078D4" alt="NuGet Squirrel.Caching" height="28"></a> |
| `Squirrel.Caching.AzureRedis` | Azure Redis hosting and client integration. | <a href="https://www.nuget.org/packages/Squirrel.Caching.AzureRedis/"><img src="https://img.shields.io/nuget/v/Squirrel.Caching.AzureRedis.svg?style=flat&color=0078D4" alt="NuGet Squirrel.Caching.AzureRedis" height="28"></a> |
| `Squirrel.Serialization` | MemoryPack serializer registration. | <a href="https://www.nuget.org/packages/Squirrel.Serialization/"><img src="https://img.shields.io/nuget/v/Squirrel.Serialization.svg?style=flat&color=0078D4" alt="NuGet Squirrel.Serialization" height="28"></a> |
| `Squirrel.Integration.Wolverine` | Wolverine messaging and RabbitMQ integration. | <a href="https://www.nuget.org/packages/Squirrel.Integration.Wolverine/"><img src="https://img.shields.io/nuget/v/Squirrel.Integration.Wolverine.svg?style=flat&color=0078D4" alt="NuGet Squirrel.Integration.Wolverine" height="28"></a> |
| `Squirrel.Integration.Wolverine.InMemory` | In-process Wolverine bus and durable local queues. | <a href="https://www.nuget.org/packages/Squirrel.Integration.Wolverine.InMemory/"><img src="https://img.shields.io/nuget/v/Squirrel.Integration.Wolverine.InMemory.svg?style=flat&color=0078D4" alt="NuGet Squirrel.Integration.Wolverine.InMemory" height="28"></a> |
| `Squirrel.Persistence.EfCore.Postgres` | PostgreSQL EF Core persistence and repositories. | <a href="https://www.nuget.org/packages/Squirrel.Persistence.EfCore.Postgres/"><img src="https://img.shields.io/nuget/v/Squirrel.Persistence.EfCore.Postgres.svg?style=flat&color=0078D4" alt="NuGet Squirrel.Persistence.EfCore.Postgres" height="28"></a> |
| `Squirrel.Persistence.EfCore.AzurePostgres` | Reserved Azure PostgreSQL package placeholder. | <a href="https://www.nuget.org/packages/Squirrel.Persistence.EfCore.AzurePostgres/"><img src="https://img.shields.io/nuget/v/Squirrel.Persistence.EfCore.AzurePostgres.svg?style=flat&color=0078D4" alt="NuGet Squirrel.Persistence.EfCore.AzurePostgres" height="28"></a> |
| `Squirrel.Persistence.EfCore.AzureCosmosDB` | Reserved Azure Cosmos DB package placeholder. | <a href="https://www.nuget.org/packages/Squirrel.Persistence.EfCore.AzureCosmosDB/"><img src="https://img.shields.io/nuget/v/Squirrel.Persistence.EfCore.AzureCosmosDB.svg?style=flat&color=0078D4" alt="NuGet Squirrel.Persistence.EfCore.AzureCosmosDB" height="28"></a> |
| `Squirrel.Persistence.Mongo` | MongoDB context, repositories, and health checks. | <a href="https://www.nuget.org/packages/Squirrel.Persistence.Mongo/"><img src="https://img.shields.io/nuget/v/Squirrel.Persistence.Mongo.svg?style=flat&color=0078D4" alt="NuGet Squirrel.Persistence.Mongo" height="28"></a> |
| `Squirrel.Persistence.Marten` | Marten documents, event streams, and projections. | <a href="https://www.nuget.org/packages/Squirrel.Persistence.Marten/"><img src="https://img.shields.io/nuget/v/Squirrel.Persistence.Marten.svg?style=flat&color=0078D4" alt="NuGet Squirrel.Persistence.Marten" height="28"></a> |
| `Squirrel.Persistence.EventStoreDB` | EventStoreDB client, subscriptions, and checkpoints. | <a href="https://www.nuget.org/packages/Squirrel.Persistence.EventStoreDB/"><img src="https://img.shields.io/nuget/v/Squirrel.Persistence.EventStoreDB.svg?style=flat&color=0078D4" alt="NuGet Squirrel.Persistence.EventStoreDB" height="28"></a> |
| `Squirrel.OpenTelemetry` | OpenTelemetry tracing, metrics, logging, and exporters. | <a href="https://www.nuget.org/packages/Squirrel.OpenTelemetry/"><img src="https://img.shields.io/nuget/v/Squirrel.OpenTelemetry.svg?style=flat&color=0078D4" alt="NuGet Squirrel.OpenTelemetry" height="28"></a> |
| `Squirrel.OpenApi` | Swagger, Scalar, API versioning, and AsyncAPI support. | <a href="https://www.nuget.org/packages/Squirrel.OpenApi/"><img src="https://img.shields.io/nuget/v/Squirrel.OpenApi.svg?style=flat&color=0078D4" alt="NuGet Squirrel.OpenApi" height="28"></a> |
| `Squirrel.SerilogLogging` | Serilog setup, enrichment, correlation, and sinks. | <a href="https://www.nuget.org/packages/Squirrel.SerilogLogging/"><img src="https://img.shields.io/nuget/v/Squirrel.SerilogLogging.svg?style=flat&color=0078D4" alt="NuGet Squirrel.SerilogLogging" height="28"></a> |
| `Squirrel.AspireIntegrations` | Aspire resources for infrastructure and observability tools. | <a href="https://www.nuget.org/packages/Squirrel.AspireIntegrations/"><img src="https://img.shields.io/nuget/v/Squirrel.AspireIntegrations.svg?style=flat&color=0078D4" alt="NuGet Squirrel.AspireIntegrations" height="28"></a> |

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
