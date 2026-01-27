---
description: 'C# and .NET coding conventions and best practices'
applyTo: '**/*.cs, **/*.csproj, **/*.sln, **/appsettings*.json'
---

# C# and .NET Development Instructions [v1.0]

Minimal standards for C# and .NET development. For comprehensive guidance, invoke the `dotnet-expert` skill.

Follow modern C# idioms and .NET best practices based on [Microsoft's C# Coding Conventions](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions).

## Naming Conventions

- **Classes/Methods/Properties**: PascalCase (`UserService`, `GetUserByIdAsync`)
- **Interfaces**: Prefix with 'I' (`IUserService`, `IRepository<T>`)
- **Private fields**: camelCase with underscore (`_userService`, `_logger`)
- **Constants/Enums**: PascalCase (`MaxRetryAttempts`, `OrderStatus.Pending`)
- **Local variables**: camelCase (`userId`, `userName`)
- **Async methods**: Suffix with 'Async' (`GetUserAsync`)

## Code Style

- Use file-scoped namespaces (C# 10+): `namespace MyApp.Services;`
- 4 spaces for indentation, Allman style (braces on new lines)
- Enable nullable reference types in all projects
- Use `var` for local variables when type is obvious
- Line length under 120 characters
- Order members: constants, fields, constructors, properties, methods

## Essential Patterns

- Use async/await consistently (never `async void` except event handlers)
- Use records for immutable data: `public record UserDto(int Id, string Name);`
- Use pattern matching for validation and switch expressions
- Leverage source generators (JSON, regex, logging) for performance
- Enable nullable reference types and handle null explicitly
- Use dependency injection with appropriate lifetimes

## Performance & Security

- Use source-generated JSON serialization for AOT compatibility
- Enable trim warnings: `<IsTrimmable>true</IsTrimmable>`
- Avoid reflection-based libraries (MediatR, AutoMapper)
- Validate and sanitize user input
- Use parameterized queries, never string concatenation
- Implement proper authentication and authorization

## Testing

- Use xUnit with AAA pattern (Arrange, Act, Assert)
- Mock dependencies with Moq or NSubstitute
- Aim for 80%+ code coverage
- Use meaningful test names describing scenario and outcome

## Common Pitfalls

- Don't block on async with `.Result` or `.Wait()` (causes deadlocks)
- Don't use generic repositories when using Entity Framework
- Don't write business logic in controllers
- Don't ignore compiler or trim warnings

## Comprehensive Guidance

For detailed .NET patterns, ASP.NET Core development, EF Core optimization, and advanced features, invoke the **`dotnet-expert` skill**.
