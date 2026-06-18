# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is a course project ("Curso Erudio") for learning REST API development with ASP.NET Core 10. Each top-level numbered folder (e.g., `04_RestWithASPNET10Erudio_ScaffoldViaTerminal/`) represents a lesson or stage. New stages will be added as the course progresses.

## Project Layout

Each lesson folder contains a self-contained solution:

```
04_RestWithASPNET10Erudio_ScaffoldViaTerminal/
└── RestWithASPNET10Erudio/
    ├── RestWithASPNET10Erudio.slnx   # solution file
    └── RestWithASPNET10Erudio/       # main project
        ├── Program.cs
        ├── RestWithASPNET10Erudio.csproj
        └── appsettings.json
```

When working on a specific lesson, `cd` into its inner project folder (the one with the `.csproj`) before running dotnet commands.

## Commands

All commands must be run from inside the project directory containing the `.csproj`:

```bash
# Build
dotnet build

# Run (HTTP on :5268, HTTPS on :7206)
dotnet run

# Run with a specific launch profile
dotnet run --launch-profile https

# Restore NuGet packages
dotnet restore

# Add a NuGet package
dotnet add package <PackageName>
```

There are currently no test projects. When test projects are added, run them with:

```bash
dotnet test
# Run a single test by name filter
dotnet test --filter "FullyQualifiedName~MyTestMethod"
```

OpenAPI schema is available at `http://localhost:5268/openapi/v1.json` when running in Development mode.

The `.http` file in the project root (`RestWithASPNET10Erudio.http`) can be used with the VS Code REST Client or JetBrains HTTP Client to exercise endpoints manually.

## Architecture

### Current Pattern: ASP.NET Core Minimal APIs

Endpoints are registered directly in `Program.cs` using `app.MapGet/Post/Put/Delete`. There are no MVC controllers. All service registration goes through `builder.Services` before `builder.Build()`.

```csharp
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddOpenApi();
var app = builder.Build();

app.MapGet("/resource", () => { /* handler */ }).WithName("EndpointName");

app.Run();
```

### Data Models

Immutable data objects use C# `record` types:

```csharp
record MyModel(DateOnly Date, int Value, string? Label);
```

### Key Project Settings

- `<TargetFramework>net10.0</TargetFramework>`
- `<Nullable>enable</Nullable>` — all reference types are non-nullable by default; use `string?` to opt into nullability
- `<ImplicitUsings>enable</ImplicitUsings>` — common namespaces (`System`, `System.Linq`, etc.) are available without explicit `using` statements

### Adding New Lessons

New course stages should follow the existing naming convention: `NN_<ProjectName>_<Topic>/` at the repository root, each containing a fresh `.slnx` + project folder created with `dotnet new webapi`.
