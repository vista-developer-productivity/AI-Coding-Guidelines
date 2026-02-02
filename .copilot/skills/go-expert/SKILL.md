---
name: go-expert
description: Expert in Go language features, idioms, standard library, and Go patterns. Use when working with Go-specific language features, not for web services or architecture (use backend-expert for that).
---

# Go Language Expert

You are an Expert in Go programming language, its idioms, standard library, and Go-specific patterns.

## Core Expertise

### Go Language Features

- **Interfaces**: Implicit implementation, empty interface, type assertions
- **Struct Patterns**: Embedding, composition over inheritance, struct tags
- **Error Handling**: error interface, errors.Is/As, custom errors, wrapping
- **Pointers**: Value vs pointer receivers, nil pointers, pointer semantics
- **Slices & Maps**: append, make, cap, map initialization, zero values
- **Defer**: Resource cleanup, panic recovery, defer order
- **Type System**: Type aliases, type definitions, type conversions

### Go Concurrency Basics (Language Level)

- **Goroutines**: go keyword, goroutine lifecycle
- **Channels**: Buffered vs unbuffered, send/receive, close
- **Select**: Multiplexing channels, default cases, timeouts
- **Sync Primitives**: Mutex, RWMutex, WaitGroup, Once

**Note**: For concurrency patterns, worker pools, and distributed systems, use `backend-expert` skill.

### Standard Library

- **strings**: Builder, manipulation, case conversion
- **fmt**: Formatting, Sprintf, Errorf
- **io**: Reader, Writer, Copy, pipes
- **encoding/json**: Marshal, Unmarshal, struct tags
- **time**: Duration, Ticker, Timer, time zones
- **regexp**: Regular expressions, FindString, ReplaceAll
- **flag**: Command-line argument parsing
- **os**: File operations, environment variables

### Testing Basics (Language Level)

- **testing package**: Test functions, table-driven tests
- **testify assertions**: require vs assert
- **Test helpers**: t.Helper(), t.Cleanup()
- **Subtests**: t.Run for grouped tests

**Note**: For testing strategies, patterns, and architecture, use `testing-expert` skill.

## Approach

When writing Go code:

1. **Simple and explicit**: Avoid clever code, prioritize clarity
2. **Error handling**: Check errors explicitly, don't ignore them
3. **Zero values**: Leverage useful zero values for clean initialization
4. **Composition**: Prefer composition and interfaces over inheritance
5. **Standard library**: Use stdlib before adding dependencies
6. **gofmt always**: Format code with gofmt/goimports

Write idiomatic Go that embraces simplicity and explicitness.

---

## Interfaces and Type System

### Interface Patterns

```go
// Small, focused interfaces
type Reader interface {
    Read(p []byte) (n int, err error)
}

type Writer interface {
    Write(p []byte) (n int, err error)
}

// Interface composition
type ReadWriter interface {
    Reader
    Writer
}

// Empty interface (any in Go 1.18+)
func PrintAny(v interface{}) {
    fmt.Println(v)
}

// Type assertion
func processValue(v interface{}) {
    if str, ok := v.(string); ok {
        fmt.Println("String:", str)
    }
}

// Type switch
func identify(v interface{}) string {
    switch t := v.(type) {
    case string:
        return "string"
    case int:
        return "int"
    default:
        return fmt.Sprintf("unknown: %T", t)
    }
}
```

### Struct Embedding

```go
// Embedding for composition
type User struct {
    ID   int
    Name string
}

func (u User) Display() string {
    return fmt.Sprintf("%s (ID: %d)", u.Name, u.ID)
}

// Admin embeds User
type Admin struct {
    User        // Embedded field
    Permissions []string
}

// Usage - promoted methods
admin := Admin{
    User: User{ID: 1, Name: "Alice"},
    Permissions: []string{"read", "write"},
}
fmt.Println(admin.Display()) // Calls User.Display()
fmt.Println(admin.Name)      // Accesses User.Name directly
```

### Struct Tags

```go
type Person struct {
    Name  string `json:"name" validate:"required"`
    Email string `json:"email" validate:"required,email"`
    Age   int    `json:"age,omitempty" validate:"min=0,max=150"`
}

// Custom tag parsing
import "reflect"

func getTag(f reflect.StructField, key string) string {
    return f.Tag.Get(key)
}
```

---

## Error Handling

### Standard Error Patterns

```go
import (
    "errors"
    "fmt"
)

// Creating errors
err := errors.New("something went wrong")
err := fmt.Errorf("failed to process user %d", userID)

// Error checking
if err != nil {
    return fmt.Errorf("operation failed: %w", err) // Wrap with %w
}

// Error wrapping and unwrapping (Go 1.13+)
baseErr := errors.New("base error")
wrappedErr := fmt.Errorf("wrapped: %w", baseErr)

// Check if error is specific type
if errors.Is(wrappedErr, baseErr) {
    fmt.Println("Found base error")
}

// Extract error type
var pathErr *os.PathError
if errors.As(err, &pathErr) {
    fmt.Println("Path error:", pathErr.Path)
}
```

### Custom Errors

```go
// Custom error type
type ValidationError struct {
    Field string
    Issue string
}

func (e *ValidationError) Error() string {
    return fmt.Sprintf("validation error on %s: %s", e.Field, e.Issue)
}

// Sentinel errors
var (
    ErrNotFound     = errors.New("not found")
    ErrUnauthorized = errors.New("unauthorized")
    ErrInvalidInput = errors.New("invalid input")
)

// Usage
func GetUser(id int) (*User, error) {
    if id <= 0 {
        return nil, ErrInvalidInput
    }
    // ... lookup logic
    return nil, ErrNotFound
}
```

---

## Slices, Maps, and Collections

### Slice Operations

```go
// Slice initialization
s1 := []int{1, 2, 3}
s2 := make([]int, 5)      // length 5, capacity 5
s3 := make([]int, 0, 10)  // length 0, capacity 10

// Append
s := []int{1, 2, 3}
s = append(s, 4, 5)
s = append(s, []int{6, 7}...) // Spread operator

// Slicing
s := []int{1, 2, 3, 4, 5}
sub := s[1:3]   // [2, 3]
sub := s[:3]    // [1, 2, 3]
sub := s[2:]    // [3, 4, 5]

// Capacity and length
fmt.Println(len(s), cap(s))

// Copy
src := []int{1, 2, 3}
dst := make([]int, len(src))
copy(dst, src)

// Remove element (order preserved)
func remove(slice []int, i int) []int {
    return append(slice[:i], slice[i+1:]...)
}

// Remove element (order not preserved, faster)
func removeFast(slice []int, i int) []int {
    slice[i] = slice[len(slice)-1]
    return slice[:len(slice)-1]
}
```

### Map Operations

```go
// Map initialization
m1 := map[string]int{"a": 1, "b": 2}
m2 := make(map[string]int)
m3 := make(map[string]int, 100) // With capacity hint

// Access with existence check
value, ok := m["key"]
if !ok {
    // Key doesn't exist
}

// Delete
delete(m, "key")

// Iterate (order not guaranteed)
for key, value := range m {
    fmt.Println(key, value)
}

// Iterate keys only
for key := range m {
    fmt.Println(key)
}

// Iterate values only
for _, value := range m {
    fmt.Println(value)
}
```

---

## Defer, Panic, and Recover

### Defer Patterns

```go
// Basic defer - executes in LIFO order
func processFile(filename string) error {
    f, err := os.Open(filename)
    if err != nil {
        return err
    }
    defer f.Close() // Executed when function returns
    
    // Process file
    return nil
}

// Multiple defers (LIFO)
func example() {
    defer fmt.Println("Third")
    defer fmt.Println("Second")
    defer fmt.Println("First")
}
// Prints: First, Second, Third

// Defer with cleanup
func transaction() error {
    tx := beginTransaction()
    defer func() {
        if err := recover(); err != nil {
            tx.Rollback()
        }
    }()
    
    // Perform operations
    return tx.Commit()
}
```

### Panic and Recover

```go
// Panic for unrecoverable errors
func mustConnect(url string) *Connection {
    conn, err := connect(url)
    if err != nil {
        panic(fmt.Sprintf("failed to connect: %v", err))
    }
    return conn
}

// Recover from panic
func safeExecute(fn func()) (err error) {
    defer func() {
        if r := recover(); r != nil {
            err = fmt.Errorf("panic recovered: %v", r)
        }
    }()
    
    fn()
    return nil
}
```

---

## Goroutines and Channels (Basics)

### Basic Goroutines

```go
// Launch goroutine
go func() {
    fmt.Println("Running in goroutine")
}()

// Wait for completion
var wg sync.WaitGroup
wg.Add(1)
go func() {
    defer wg.Done()
    fmt.Println("Work done")
}()
wg.Wait()
```

### Channel Basics

```go
// Unbuffered channel (synchronous)
ch := make(chan int)

// Buffered channel
ch := make(chan int, 10)

// Send and receive
ch <- 42        // Send
value := <-ch   // Receive
value, ok := <-ch // Receive with closed check

// Close channel
close(ch)

// Range over channel
for value := range ch {
    fmt.Println(value)
}

// Select for multiplexing
select {
case msg := <-ch1:
    fmt.Println("Received from ch1:", msg)
case msg := <-ch2:
    fmt.Println("Received from ch2:", msg)
case <-time.After(1 * time.Second):
    fmt.Println("Timeout")
default:
    fmt.Println("No messages")
}
```

### Sync Primitives

```go
import "sync"

// Mutex for exclusive access
var mu sync.Mutex
mu.Lock()
// Critical section
mu.Unlock()

// RWMutex for read/write separation
var rwmu sync.RWMutex
rwmu.RLock()   // Multiple readers
// Read operation
rwmu.RUnlock()

rwmu.Lock()    // Single writer
// Write operation
rwmu.Unlock()

// Once for one-time initialization
var once sync.Once
once.Do(func() {
    // Initialization code (runs only once)
})

// WaitGroup for goroutine coordination
var wg sync.WaitGroup
for i := 0; i < 5; i++ {
    wg.Add(1)
    go func(id int) {
        defer wg.Done()
        // Work
    }(i)
}
wg.Wait()
```

**Note**: For advanced concurrency patterns (worker pools, fan-out/fan-in, context cancellation), use the `backend-expert` skill.

---

## Standard Library Usage

### strings Package

```go
import "strings"

// Common operations
strings.Contains("hello", "ell")     // true
strings.HasPrefix("hello", "he")    // true
strings.HasSuffix("hello", "lo")    // true
strings.Split("a,b,c", ",")         // ["a", "b", "c"]
strings.Join([]string{"a", "b"}, "-") // "a-b"
strings.ToUpper("hello")            // "HELLO"
strings.ToLower("HELLO")            // "hello"
strings.TrimSpace("  hello  ")      // "hello"
strings.Replace("hello", "l", "L", 1) // "heLlo"

// Builder for efficient string concatenation
var builder strings.Builder
for i := 0; i < 100; i++ {
    builder.WriteString("item ")
}
result := builder.String()
```

### io Package

```go
import (
    "io"
    "os"
    "strings"
)

// Copy between readers and writers
src, _ := os.Open("source.txt")
dst, _ := os.Create("dest.txt")
defer src.Close()
defer dst.Close()
io.Copy(dst, src)

// Read all
data, err := io.ReadAll(reader)

// String reader
reader := strings.NewReader("hello world")

// Pipe for connecting reader and writer
pr, pw := io.Pipe()
```

### encoding/json

```go
import "encoding/json"

type User struct {
    ID    int    `json:"id"`
    Name  string `json:"name"`
    Email string `json:"email,omitempty"`
}

// Marshal (struct to JSON)
user := User{ID: 1, Name: "Alice", Email: "alice@example.com"}
jsonBytes, err := json.Marshal(user)
jsonString := string(jsonBytes)

// MarshalIndent for pretty printing
jsonBytes, err := json.MarshalIndent(user, "", "  ")

// Unmarshal (JSON to struct)
jsonStr := `{"id":1,"name":"Alice"}`
var user User
err := json.Unmarshal([]byte(jsonStr), &user)

// Decoder for streaming
decoder := json.NewDecoder(reader)
var user User
err := decoder.Decode(&user)

// Encoder for streaming
encoder := json.NewEncoder(writer)
err := encoder.Encode(user)
```

### time Package

```go
import "time"

// Current time
now := time.Now()

// Parse time
t, err := time.Parse("2006-01-02", "2024-01-15")
t, err := time.Parse(time.RFC3339, "2024-01-15T10:30:00Z")

// Format time
formatted := now.Format("2006-01-02 15:04:05")
formatted := now.Format(time.RFC3339)

// Duration
duration := 5 * time.Second
time.Sleep(duration)

// Timer
timer := time.NewTimer(5 * time.Second)
<-timer.C // Wait for timer

// Ticker
ticker := time.NewTicker(1 * time.Second)
defer ticker.Stop()
for t := range ticker.C {
    fmt.Println("Tick at", t)
}

// Add/Sub
future := now.Add(24 * time.Hour)
diff := future.Sub(now)
```

---

## Testing Basics

### Table-Driven Tests

```go
import "testing"

func TestAdd(t *testing.T) {
    tests := []struct {
        name     string
        a, b     int
        expected int
    }{
        {"positive numbers", 2, 3, 5},
        {"negative numbers", -2, -3, -5},
        {"mixed", 5, -3, 2},
        {"zero", 0, 5, 5},
    }
    
    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            result := Add(tt.a, tt.b)
            if result != tt.expected {
                t.Errorf("Add(%d, %d) = %d; want %d",
                    tt.a, tt.b, result, tt.expected)
            }
        })
    }
}
```

### Test Helpers

```go
// Helper function
func setup(t *testing.T) *Database {
    t.Helper() // Mark as helper
    
    db := &Database{}
    t.Cleanup(func() {
        db.Close() // Cleanup after test
    })
    
    return db
}

func TestDatabase(t *testing.T) {
    db := setup(t)
    // Test code
}
```

### testify Assertions

```go
import (
    "testing"
    "github.com/stretchr/testify/assert"
    "github.com/stretchr/testify/require"
)

func TestUser(t *testing.T) {
    user := &User{Name: "Alice"}
    
    // assert - continues on failure
    assert.NotNil(t, user)
    assert.Equal(t, "Alice", user.Name)
    
    // require - stops on failure
    require.NotNil(t, user)
    require.Equal(t, "Alice", user.Name)
    
    // Collections
    assert.Contains(t, []int{1, 2, 3}, 2)
    assert.Len(t, []int{1, 2, 3}, 3)
    
    // Errors
    err := doSomething()
    assert.NoError(t, err)
    assert.Error(t, err)
}
```

**Note**: For comprehensive testing strategies, mocking patterns, and test architecture, use the `testing-expert` skill.

---

## Go Idioms and Best Practices

### Common Patterns

```go
// Accept interfaces, return structs
func ProcessData(r io.Reader) (*Result, error) {
    // Implementation
}

// Pointers for large structs or mutation
type LargeStruct struct {
    // Many fields
}

func (l *LargeStruct) Update() {
    // Modify struct
}

// Values for small, immutable data
type Point struct {
    X, Y int
}

func (p Point) Distance() float64 {
    // No mutation
}

// Nil slice vs empty slice
var nilSlice []int            // nil, len=0, cap=0
emptySlice := []int{}         // not nil, len=0, cap=0
emptySlice := make([]int, 0)  // not nil, len=0, cap=0

// Zero value initialization
type Config struct {
    Timeout time.Duration // Defaults to 0
    Retries int           // Defaults to 0
    Enabled bool          // Defaults to false
}

// Make zero value useful
type Buffer struct {
    buf []byte // nil slice is valid
}

func (b *Buffer) Write(p []byte) (n int, err error) {
    b.buf = append(b.buf, p...) // Works even if buf is nil
    return len(p), nil
}
```

### Error Handling Idioms

```go
// Check errors immediately
result, err := doSomething()
if err != nil {
    return fmt.Errorf("failed to do something: %w", err)
}

// Early return on error
func process() error {
    if err := step1(); err != nil {
        return err
    }
    if err := step2(); err != nil {
        return err
    }
    return step3()
}

// Named return for error handling
func divide(a, b int) (result int, err error) {
    if b == 0 {
        err = errors.New("division by zero")
        return
    }
    result = a / b
    return
}
```

---

For HTTP servers, REST APIs, microservices, and advanced concurrency patterns, use the **`backend-expert` skill**.

For testing strategies, test architecture, and comprehensive testing patterns, use the **`testing-expert` skill**.
