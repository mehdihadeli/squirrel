# Packages

Squirrel publishes the foundation package `Squirrel.CoreKit` and a coordinated `Squirrel.*` NuGet family. Install only the packages needed by each service; package registrations are designed to compose through `IHostApplicationBuilder` and `IServiceCollection`.

## Foundations

| Package | Purpose | Details |
| --- | --- | --- |
| `Squirrel.Abstractions` | Shared contracts for commands, queries, events, persistence, paging, and modules. | [Read the component guide](../components/abstractions) |
| `Squirrel.CoreKit` | Core buses, domain services, options, exceptions, and common extensions. | [Read the component guide](../components/core) |

## Platform

| Package                      | Purpose                                                                          | Details                                                |
| ---------------------------- | -------------------------------------------------------------------------------- | ------------------------------------------------------ |
| `Squirrel.Web`         | CORS, compression, versioning, rate limiting, problem details, and minimal APIs. | [Read the component guide](../components/web)          |
| `Squirrel.Security`    | JWT and API-key authentication helpers.                                          | [Read the component guide](../components/security)     |
| `Squirrel.HealthCheck` | Default liveness and readiness health checks.                                    | [Read the component guide](../components/health-check) |
| `Squirrel.Resiliency`  | Service discovery and HTTP resilience defaults.                                  | [Read the component guide](../components/resiliency)   |
| `Squirrel.Email`       | Email delivery abstraction and provider integration.                             | [Read the component guide](../components/email)        |

## Messaging and persistence

| Package                                           | Purpose                                                                  | Details                                                                     |
| ------------------------------------------------- | ------------------------------------------------------------------------ | --------------------------------------------------------------------------- |
| `Squirrel.Integration.Wolverine`            | Wolverine and RabbitMQ event bus integration.                            | [Read the component guide](../components/wolverine)                         |
| `Squirrel.Integration.Wolverine.InMemory`   | In-process Wolverine event bus and local queues, with optional PostgreSQL durability. | [Read the component guide](../components/wolverine)                         |
| `Squirrel.Persistence.EfCore.Postgres`      | PostgreSQL EF Core context, repositories, unit of work, and migrations.  | [Read the component guide](../components/persistence-efcore-postgres)       |
| `Squirrel.Persistence.EfCore.AzurePostgres` | Azure PostgreSQL package area.                                           | [Read the component guide](../components/persistence-efcore-azure-postgres) |
| `Squirrel.Persistence.EfCore.AzureCosmosDB` | Azure Cosmos DB persistence package area.                                | [Read the component guide](../components/persistence-efcore-azure-cosmosdb) |
| `Squirrel.Persistence.Mongo`                | MongoDB context, repositories, unit of work, tracing, and health checks. | [Read the component guide](../components/persistence-mongo)                 |
| `Squirrel.Persistence.Marten`               | Marten document and event-sourcing integration.                          | [Read the component guide](../components/persistence-marten)                |
| `Squirrel.Persistence.EventStoreDB`         | EventStoreDB client and event-sourcing integration.                      | [Read the component guide](../components/persistence-eventstoredb)          |

## Observability and supporting infrastructure

| Package                             | Purpose                                                   | Details                                                       |
| ----------------------------------- | --------------------------------------------------------- | ------------------------------------------------------------- |
| `Squirrel.OpenTelemetry`      | OpenTelemetry traces, metrics, logs, and instrumentation. | [Read the component guide](../components/opentelemetry)       |
| `Squirrel.OpenApi`            | Swagger, ASP.NET OpenAPI, API versioning, and AsyncAPI.   | [Read the component guide](../components/openapi)             |
| `Squirrel.SerilogLogging`     | Structured logging and request enrichment.                | [Read the component guide](../components/serilog-logging)     |
| `Squirrel.Caching`            | Cache abstractions and cache registration.                | [Read the component guide](../components/caching)             |
| `Squirrel.Caching.AzureRedis` | Azure Redis cache integration.                            | [Read the component guide](../components/caching-azure-redis) |
| `Squirrel.Serialization`      | MemoryPack serialization registration.                    | [Read the component guide](../components/serialization)       |
| `Squirrel.Validation`         | FluentValidation assembly scanning.                       | [Read the component guide](../components/validation)          |
| `Squirrel.AspireIntegrations` | .NET Aspire resources for local infrastructure.           | [Read the component guide](../components/aspire-integrations) |

All packages target `net10.0` and are versioned together with Nerdbank.GitVersioning.
