# 🐿️ Squirrel

<p align="center">
  <img src="assets/squirrel.png" alt="Squirrel package brand" width="320">
</p>

Reusable infrastructure building blocks for .NET microservices.

Squirrel is a collection of focused NuGet packages for persistence, messaging, authentication, resilience, health checks, logging, tracing, validation, and caching. It gives services shared infrastructure conventions while keeping application code on standard .NET hosting, dependency injection, configuration, and middleware patterns.

Install the packages your service needs and compose their registrations. Squirrel is a toolkit, not an application framework that controls your architecture.

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

Each package can be installed independently. The badges show the latest stable version reported by NuGet; click a badge to open the package page without pinning it to a release.

| Package | Description | Latest NuGet |
| --- | --- | --- |
| `Squirrel.Abstractions` | Shared contracts for commands, queries, events, messages, repositories, paging, persistence, serialization, and web modules. | [![NuGet Squirrel.Abstractions](https://img.shields.io/nuget/v/Squirrel.Abstractions.svg)](https://www.nuget.org/packages/Squirrel.Abstractions/) |
| `Squirrel.CoreKit` | Common application and domain services, command and query support, options, exceptions, validation helpers, paging, and dependency-registration extensions. | [![NuGet Squirrel.CoreKit](https://img.shields.io/nuget/v/Squirrel.CoreKit.svg)](https://www.nuget.org/packages/Squirrel.CoreKit/) |
| `Squirrel.Web` | ASP.NET Core conventions for CORS, compression, API versioning, rate limiting, problem details, minimal APIs, and endpoint mapping. | [![NuGet Squirrel.Web](https://img.shields.io/nuget/v/Squirrel.Web.svg)](https://www.nuget.org/packages/Squirrel.Web/) |
| `Squirrel.Security` | JWT bearer and API-key authentication helpers for HTTP APIs. | [![NuGet Squirrel.Security](https://img.shields.io/nuget/v/Squirrel.Security.svg)](https://www.nuget.org/packages/Squirrel.Security/) |
| `Squirrel.HealthCheck` | Default health-check registration and endpoint conventions for service health monitoring. | [![NuGet Squirrel.HealthCheck](https://img.shields.io/nuget/v/Squirrel.HealthCheck.svg)](https://www.nuget.org/packages/Squirrel.HealthCheck/) |
| `Squirrel.Resiliency` | Service discovery and HTTP resilience with timeouts, retries, and circuit breakers. | [![NuGet Squirrel.Resiliency](https://img.shields.io/nuget/v/Squirrel.Resiliency.svg)](https://www.nuget.org/packages/Squirrel.Resiliency/) |
| `Squirrel.Validation` | FluentValidation assembly scanning and dependency-injection registration for request and command validators. | [![NuGet Squirrel.Validation](https://img.shields.io/nuget/v/Squirrel.Validation.svg)](https://www.nuget.org/packages/Squirrel.Validation/) |
| `Squirrel.Email` | Email delivery abstraction with provider integrations such as SendGrid. | [![NuGet Squirrel.Email](https://img.shields.io/nuget/v/Squirrel.Email.svg)](https://www.nuget.org/packages/Squirrel.Email/) |
| `Squirrel.Caching` | Cache abstractions, hybrid caching support, Redis integration contracts, and cache invalidation patterns. | [![NuGet Squirrel.Caching](https://img.shields.io/nuget/v/Squirrel.Caching.svg)](https://www.nuget.org/packages/Squirrel.Caching/) |
| `Squirrel.Caching.AzureRedis` | Azure Redis hosting and client integration for Azure Redis or Azure Managed Redis. | [![NuGet Squirrel.Caching.AzureRedis](https://img.shields.io/nuget/v/Squirrel.Caching.AzureRedis.svg)](https://www.nuget.org/packages/Squirrel.Caching.AzureRedis/) |
| `Squirrel.Serialization` | MemoryPack serialization registration for binary messages and data payloads. | [![NuGet Squirrel.Serialization](https://img.shields.io/nuget/v/Squirrel.Serialization.svg)](https://www.nuget.org/packages/Squirrel.Serialization/) |
| `Squirrel.Integration.Wolverine` | Wolverine and RabbitMQ integration with handler discovery, message topology, and durable messaging support. | [![NuGet Squirrel.Integration.Wolverine](https://img.shields.io/nuget/v/Squirrel.Integration.Wolverine.svg)](https://www.nuget.org/packages/Squirrel.Integration.Wolverine/) |
| `Squirrel.Integration.Wolverine.InMemory` | Wolverine in-process event bus and local queues, with optional PostgreSQL-backed durability and no RabbitMQ dependency. | [![NuGet Squirrel.Integration.Wolverine.InMemory](https://img.shields.io/nuget/v/Squirrel.Integration.Wolverine.InMemory.svg)](https://www.nuget.org/packages/Squirrel.Integration.Wolverine.InMemory/) |
| `Squirrel.Persistence.EfCore.Postgres` | PostgreSQL EF Core setup, repositories, unit of work, migrations, naming conventions, retries, and persistence interceptors. | [![NuGet Squirrel.Persistence.EfCore.Postgres](https://img.shields.io/nuget/v/Squirrel.Persistence.EfCore.Postgres.svg)](https://www.nuget.org/packages/Squirrel.Persistence.EfCore.Postgres/) |
| `Squirrel.Persistence.EfCore.AzurePostgres` | Reserved package area for Azure PostgreSQL persistence; currently a placeholder without a provider implementation. | [![NuGet Squirrel.Persistence.EfCore.AzurePostgres](https://img.shields.io/nuget/v/Squirrel.Persistence.EfCore.AzurePostgres.svg)](https://www.nuget.org/packages/Squirrel.Persistence.EfCore.AzurePostgres/) |
| `Squirrel.Persistence.EfCore.AzureCosmosDB` | Reserved package area for Azure Cosmos DB persistence; currently a placeholder without a provider implementation. | [![NuGet Squirrel.Persistence.EfCore.AzureCosmosDB](https://img.shields.io/nuget/v/Squirrel.Persistence.EfCore.AzureCosmosDB.svg)](https://www.nuget.org/packages/Squirrel.Persistence.EfCore.AzureCosmosDB/) |
| `Squirrel.Persistence.Mongo` | MongoDB context, repositories, unit of work, health checks, and tracing integration. | [![NuGet Squirrel.Persistence.Mongo](https://img.shields.io/nuget/v/Squirrel.Persistence.Mongo.svg)](https://www.nuget.org/packages/Squirrel.Persistence.Mongo/) |
| `Squirrel.Persistence.Marten` | Marten document storage and event sourcing on PostgreSQL, including event streams, projections, and subscriptions. | [![NuGet Squirrel.Persistence.Marten](https://img.shields.io/nuget/v/Squirrel.Persistence.Marten.svg)](https://www.nuget.org/packages/Squirrel.Persistence.Marten/) |
| `Squirrel.Persistence.EventStoreDB` | EventStoreDB client, event-sourcing integration, subscriptions, checkpoints, tracing, and health checks. | [![NuGet Squirrel.Persistence.EventStoreDB](https://img.shields.io/nuget/v/Squirrel.Persistence.EventStoreDB.svg)](https://www.nuget.org/packages/Squirrel.Persistence.EventStoreDB/) |
| `Squirrel.OpenTelemetry` | OpenTelemetry tracing, metrics, logging, exporters, and instrumentation for common dependencies. | [![NuGet Squirrel.OpenTelemetry](https://img.shields.io/nuget/v/Squirrel.OpenTelemetry.svg)](https://www.nuget.org/packages/Squirrel.OpenTelemetry/) |
| `Squirrel.OpenApi` | Swagger, ASP.NET OpenAPI, API versioning documentation, Scalar, and AsyncAPI support. | [![NuGet Squirrel.OpenApi](https://img.shields.io/nuget/v/Squirrel.OpenApi.svg)](https://www.nuget.org/packages/Squirrel.OpenApi/) |
| `Squirrel.SerilogLogging` | Serilog setup, request enrichment, correlation data, baggage enrichment, and common sinks. | [![NuGet Squirrel.SerilogLogging](https://img.shields.io/nuget/v/Squirrel.SerilogLogging.svg)](https://www.nuget.org/packages/Squirrel.SerilogLogging/) |
| `Squirrel.AspireIntegrations` | .NET Aspire resources for databases, caches, RabbitMQ, EventStoreDB, Elasticsearch, and observability tools such as Grafana, Jaeger, Loki, Prometheus, Tempo, and Zipkin. | [![NuGet Squirrel.AspireIntegrations](https://img.shields.io/nuget/v/Squirrel.AspireIntegrations.svg)](https://www.nuget.org/packages/Squirrel.AspireIntegrations/) |

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
