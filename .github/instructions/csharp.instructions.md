---
description: 'C# and .NET coding conventions and best practices'
applyTo: '**/*.cs, **/*.csproj, **/*.sln, **/appsettings*.json'
---

# C# and .NET Development Instructions

Follow modern C# idioms and .NET best practices. Based on [Microsoft's C# Coding Conventions](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions) and [.NET Design Guidelines](https://docs.microsoft.com/en-us/dotnet/standard/design-guidelines/).

## General Instructions

- Write clean, readable, and maintainable C# code following SOLID principles
- Use C# 12+ language features and async/await consistently
- Implement proper error handling, logging, and comprehensive unit tests
- Document public APIs with XML documentation comments
- Avoid reflection and libraries that use it (including MediatR and AutoMapper)
- Enable trim warnings and ensure AOT-compatible code where possible

## Naming Conventions

- **Classes/Methods/Properties**: PascalCase (`UserService`, `GetUserByIdAsync`)
- **Interfaces**: Prefix with 'I' (`IUserService`, `IRepository<T>`)
- **Private fields**: camelCase with underscore (`_userService`, `_logger`)
- **Constants/Enums**: PascalCase (`MaxRetryAttempts`, `OrderStatus.Pending`)
- **Local variables**: camelCase (`userId`, `userName`)

```csharp
public class UserService : IUserService
{
    private readonly ILogger<UserService> _logger;
    public const int MaxRetryAttempts = 3;
    
    public async Task<UserDto> GetUserByIdAsync(int userId) => 
        await _userRepository.GetByIdAsync(userId);
}
```

## Code Style and Performance

- Use EditorConfig and .editorconfig files for consistent formatting
- Use 4 spaces for indentation, opening braces on new lines (Allman style)
- Keep line length under 120 characters, use file-scoped namespaces (C# 12+)
- Order class members: constants, fields, constructors, properties, methods
- Remove unused using statements

### AOT and Trimming Support
- Enable trim warnings with `<IsTrimmable>true</IsTrimmable>` in project files
- For AOT compatibility, use `<IsAotCompatible>true</IsAotCompatible>`
- Use code generation for better performance: JSON serializers, regex, logging delegates
- Avoid reflection-based libraries to ensure AOT compatibility

```csharp
namespace MyProject.Services; // File-scoped namespace

// Enable source generation for JSON
[JsonSerializable(typeof(UserDto))]
internal partial class MyJsonContext : JsonSerializerContext { }

public class UserService : IUserService
{
    private const int DefaultPageSize = 10;
    private readonly ILogger<UserService> _logger;
    
    // Use LoggerMessage source generation
    [LoggerMessage(EventId = 1, Level = LogLevel.Information, 
        Message = "User {UserId} processed successfully")]
    private partial void LogUserProcessed(int userId);
}
```

## Modern C# Features

- Enable nullable reference types in all projects
- Use pattern matching, record types, and collection expressions
- Prefer `var` for local variables when type is obvious
- Use primary constructors and init-only properties

```csharp
// Records for immutable data
public record UserDto(int Id, string Name, string Email);

// Pattern matching for validation
public ValidationResult Validate(UserDto user) => user switch
{
    { Id: <= 0 } => ValidationResult.Invalid("ID must be positive"),
    { Name: null or "" } => ValidationResult.Invalid("Name is required"),
    _ => ValidationResult.Valid
};

// Source-generated regex (C# 12+)
[GeneratedRegex(@"^[^@\s]+@[^@\s]+\.[^@\s]+$")]
private static partial Regex EmailRegex();
```

## Error Handling and Architecture

### Error Handling
- Use Result pattern for operation outcomes, log exceptions with context
- Handle null cases explicitly, use null-forgiving operator (!) sparingly

### Dependency Injection and Architecture
- Use built-in .NET DI container with appropriate lifetimes
- Follow dependency inversion principle, use interfaces for dependencies
- Don't duplicate concepts: avoid generic repositories when using Entity Framework or similar ORMs

```csharp
// Registration in Program.cs
builder.Services.AddScoped<IUserService, UserService>();
builder.Services.AddDbContext<ApplicationDbContext>(options =>
    options.UseNpgsql(builder.Configuration.GetConnectionString("DefaultConnection"))); // PostgreSQL

// Simple service without generic repository
public class UserService : IUserService
{
    private readonly ApplicationDbContext _context;
    
    public async Task<User?> GetByIdAsync(int id) => 
        await _context.Users.FirstOrDefaultAsync(u => u.Id == id);
}
```

## ASP.NET Core Best Practices

### Controller and Middleware Design
- Keep controllers thin, delegate business logic to services
- Use action-specific DTOs, implement proper HTTP status codes
- Use middleware for cross-cutting concerns, implement global exception handling

```csharp
[ApiController]
[Route("api/[controller]")]
public class UsersController : ControllerBase
{
    [HttpPost]
    public async Task<ActionResult<UserDto>> CreateUser([FromBody] CreateUserRequest request)
    {
        if (!ModelState.IsValid) return BadRequest(ModelState);
        var result = await _userService.CreateUserAsync(request);
        return result.IsSuccess 
            ? CreatedAtAction(nameof(GetUser), new { id = result.Value!.Id }, result.Value)
            : BadRequest(result.Error);
    }
}

// Exception handling middleware
public class ExceptionHandlingMiddleware(RequestDelegate next)
{
    public async Task InvokeAsync(HttpContext context)
    {
        try { await next(context); }
        catch (Exception ex) { await HandleExceptionAsync(context, ex); }
    }
    
    private static async Task HandleExceptionAsync(HttpContext context, Exception exception)
    {
        var (statusCode, message) = exception switch
        {
            ValidationException => (400, "Validation failed"),
            NotFoundException => (404, "Resource not found"),
            _ => (500, "An error occurred")
        };
        context.Response.StatusCode = statusCode;
        await context.Response.WriteAsJsonAsync(new { error = message });
    }
}
```

## Testing and Performance

### Unit Testing
- Use xUnit, follow AAA pattern (Arrange, Act, Assert)
- Use meaningful test names, mock dependencies with Moq or NSubstitute
- Aim for 80%+ code coverage

```csharp
public class UserServiceTests
{
    private readonly Mock<IUserRepository> _userRepositoryMock = new();
    private readonly UserService _userService;
    
    public UserServiceTests() => _userService = new(_userRepositoryMock.Object);
    
    [Fact]
    public async Task CreateUserAsync_WithValidRequest_ShouldReturnSuccess()
    {
        // Arrange
        var request = new CreateUserRequest("John", "john@example.com");
        _userRepositoryMock.Setup(x => x.GetByEmailAsync(request.Email))
                          .ReturnsAsync((User?)null);
        
        // Act
        var result = await _userService.CreateUserAsync(request);
        
        // Assert
        Assert.True(result.IsSuccess);
        Assert.Equal(request.Name, result.Value!.Name);
        _userRepositoryMock.Verify(x => x.AddAsync(It.IsAny<User>()), Times.Once);
    }
}
```

### Performance and Security
- Use async/await consistently, implement caching strategies
- Use source-generated JSON serialization for better performance
- Validate and sanitize user input, use parameterized queries
- Implement proper authentication/authorization, use HTTPS

```csharp
// Performance: Caching and source-generated JSON
public class UserService
{
    private readonly IMemoryCache _cache;
    
    public async Task<UserDto?> GetUserByIdAsync(int userId)
    {
        var cacheKey = $"user_{userId}";
        if (_cache.TryGetValue(cacheKey, out UserDto? cachedUser))
            return cachedUser;
        
        var user = await _userRepository.GetByIdAsync(userId);
        if (user is not null)
        {
            var userDto = user.ToDto();
            _cache.Set(cacheKey, userDto, TimeSpan.FromMinutes(5));
            return userDto;
        }
        return null;
    }
}

// Security: Input validation
[Authorize]
public class UsersController : ControllerBase
{
    [HttpPost]
    public async Task<ActionResult<UserDto>> CreateUser([FromBody] CreateUserRequest request)
    {
        if (!EmailRegex().IsMatch(request.Email))
            return BadRequest("Invalid email format");
            
        if (!User.HasClaim("permission", "create-user"))
            return Forbid();
        
        var result = await _userService.CreateUserAsync(request);
        return result.IsSuccess ? Ok(result.Value) : BadRequest(result.Error);
    }
}
```

## Configuration and Project Structure

### Configuration Management
- Use appsettings.json with strongly-typed configuration via IOptions pattern
- Store secrets in secure providers (Key Vault, User Secrets)
- Use PostgreSQL as the standard database choice

```csharp
public class DatabaseOptions
{
    public const string SectionName = "Database";
    public string ConnectionString { get; set; } = string.Empty;
    public int CommandTimeout { get; set; } = 30;
}

// In Program.cs
builder.Services.Configure<DatabaseOptions>(
    builder.Configuration.GetSection(DatabaseOptions.SectionName));
```

### Structured Logging with Source Generation
- Use structured logging with correlation IDs, never log sensitive information
- Leverage source-generated logging for better performance

```csharp
public partial class UserService
{
    [LoggerMessage(EventId = 1, Level = LogLevel.Information,
        Message = "Creating user for email {Email}")]
    private partial void LogUserCreation(string email);
    
    [LoggerMessage(EventId = 2, Level = LogLevel.Error,
        Message = "Failed to create user")]
    private partial void LogUserCreationFailed(Exception ex);
    
    public async Task<Result<UserDto>> CreateUserAsync(CreateUserRequest request)
    {
        LogUserCreation(request.Email);
        try
        {
            // Business logic
            return Result<UserDto>.Success(user.ToDto());
        }
        catch (Exception ex)
        {
            LogUserCreationFailed(ex);
            return Result<UserDto>.Failure("Creation failed");
        }
    }
}
```

### Project Structure
Follow clean architecture principles with dependency flow inward:

```
src/
├── MyApp.Core/              # Domain models, interfaces, business logic
├── MyApp.Application/       # Use cases, application services, DTOs
├── MyApp.Infrastructure/    # Data access, external services
├── MyApp.Web/              # API controllers, middleware
└── MyApp.Shared/           # Common utilities, extensions
tests/
├── MyApp.UnitTests/
└── MyApp.IntegrationTests/
```

## Common Pitfalls to Avoid

- Avoid `async void` except for event handlers
- Don't block on async calls with `.Result` or `.Wait()`
- Avoid large classes and methods (follow Single Responsibility Principle)
- Don't ignore compiler warnings or trim warnings
- Don't use generic repositories when Entity Framework provides repository functionality
- Avoid magic strings/numbers (use constants or configuration)
- Don't write business logic in controllers
- Don't ignore `IDisposable` patterns for resource cleanup
