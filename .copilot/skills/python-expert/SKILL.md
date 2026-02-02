---
name: python-expert
description: Expert in Python language features, idioms, standard library, and pythonic patterns. Use when working with Python-specific language features, not for web frameworks or architecture (use backend-expert for that).
---

# Python Language Expert

You are an Expert in Python programming language, its idioms, standard library, and pythonic patterns.

## Core Expertise

### Python Language Features

- **Type Hints**: Static typing with mypy, typing module, generics, Protocol
- **Decorators**: Function/class decorators, functools, property decorators
- **Context Managers**: `with` statement, `__enter__`/`__exit__`, contextlib
- **Generators & Iterators**: yield, generator expressions, itertools
- **Data Classes**: dataclasses, attrs, pydantic for data validation
- **Magic Methods**: `__init__`, `__str__`, `__repr__`, `__eq__`, operators

### Pythonic Patterns

- **List/Dict Comprehensions**: Concise data transformations
- **Unpacking**: `*args`, `**kwargs`, extended unpacking
- **Slicing**: Advanced slice operations, step slicing
- **Multiple Assignment**: Tuple unpacking, walrus operator (`:=`)
- **EAFP vs LBYL**: Exception handling philosophy
- **Duck Typing**: Protocol-based programming

### Standard Library

- **Collections**: defaultdict, Counter, deque, namedtuple, ChainMap
- **Itertools**: chain, groupby, combinations, permutations
- **Functools**: partial, lru_cache, singledispatch, reduce
- **Pathlib**: Modern file path handling
- **Datetime**: timezone-aware datetime handling
- **Logging**: Structured logging, handlers, formatters
- **JSON/CSV**: Data serialization and parsing

### Testing Basics (Language Level)

- **pytest fundamentals**: fixtures, parametrize, markers
- **unittest basics**: TestCase, setUp, tearDown
- **Mock/patch**: unittest.mock for testing
- **Assertions**: pytest assertions vs unittest assertions

**Note**: For testing strategies, patterns, and architecture, use `testing-expert` skill.

## Approach

When working with Python:

1. **Pythonic first**: Use idiomatic Python patterns over verbose alternatives
2. **Type safety**: Add type hints for clarity and tooling support
3. **Standard library**: Leverage built-in modules before adding dependencies
4. **Readability counts**: Follow PEP 8, write self-documenting code
5. **Explicit is better**: Avoid magic, make intentions clear
6. **Test locally**: Use pytest for language-level unit tests

Write clean, idiomatic Python that leverages language features effectively.

---

## Type Hints and mypy

### Basic Type Annotations

```python
from typing import List, Dict, Optional, Union, Tuple, Set

def process_users(
    user_ids: List[int],
    metadata: Optional[Dict[str, str]] = None
) -> List[str]:
    """Process user IDs and return usernames."""
    results: List[str] = []
    for user_id in user_ids:
        results.append(f"user_{user_id}")
    return results

# Type aliases for clarity
UserId = int
UserData = Dict[str, Union[str, int]]

def get_user(user_id: UserId) -> Optional[UserData]:
    """Retrieve user data by ID."""
    return {"id": user_id, "name": "Alice"}
```

### Advanced Type Hints

```python
from typing import Protocol, TypeVar, Generic, Callable

# Protocol for duck typing
class Drawable(Protocol):
    def draw(self) -> None: ...

def render(item: Drawable) -> None:
    item.draw()

# Generics
T = TypeVar('T')

class Stack(Generic[T]):
    def __init__(self) -> None:
        self._items: List[T] = []
    
    def push(self, item: T) -> None:
        self._items.append(item)
    
    def pop(self) -> T:
        return self._items.pop()

# Callable types
Validator = Callable[[str], bool]

def validate_email(email: str) -> bool:
    return '@' in email

def process_with_validator(data: str, validator: Validator) -> bool:
    return validator(data)
```

### Type Narrowing and Type Guards

```python
from typing import TypeGuard

def is_str_list(val: List[object]) -> TypeGuard[List[str]]:
    """Check if all items in list are strings."""
    return all(isinstance(x, str) for x in val)

def process_items(items: List[object]) -> None:
    if is_str_list(items):
        # Type narrowed to List[str]
        joined = ", ".join(items)
```

---

## Decorators

### Function Decorators

```python
from functools import wraps
import time
from typing import Any, Callable

def timer(func: Callable) -> Callable:
    """Decorator to time function execution."""
    @wraps(func)
    def wrapper(*args: Any, **kwargs: Any) -> Any:
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(f"{func.__name__} took {end - start:.2f}s")
        return result
    return wrapper

@timer
def slow_function() -> None:
    time.sleep(1)

# Decorator with arguments
def retry(times: int = 3):
    def decorator(func: Callable) -> Callable:
        @wraps(func)
        def wrapper(*args: Any, **kwargs: Any) -> Any:
            for i in range(times):
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    if i == times - 1:
                        raise
                    print(f"Retry {i + 1}/{times}")
        return wrapper
    return decorator

@retry(times=3)
def flaky_function() -> str:
    return "success"
```

### Property Decorators

```python
class User:
    def __init__(self, first_name: str, last_name: str):
        self._first_name = first_name
        self._last_name = last_name
        self._email: Optional[str] = None
    
    @property
    def full_name(self) -> str:
        """Computed property."""
        return f"{self._first_name} {self._last_name}"
    
    @property
    def email(self) -> Optional[str]:
        return self._email
    
    @email.setter
    def email(self, value: str) -> None:
        if '@' not in value:
            raise ValueError("Invalid email")
        self._email = value
```

---

## Context Managers

### Using Context Managers

```python
from contextlib import contextmanager
from typing import Generator, TextIO

# Built-in context managers
with open('file.txt', 'r') as f:
    content = f.read()

# Custom context manager (class-based)
class DatabaseConnection:
    def __enter__(self) -> 'DatabaseConnection':
        self.connect()
        return self
    
    def __exit__(self, exc_type, exc_val, exc_tb) -> None:
        self.disconnect()
    
    def connect(self) -> None:
        print("Connected")
    
    def disconnect(self) -> None:
        print("Disconnected")

# Custom context manager (decorator-based)
@contextmanager
def temporary_setting(key: str, value: str) -> Generator[None, None, None]:
    """Temporarily set a configuration value."""
    old_value = get_setting(key)
    set_setting(key, value)
    try:
        yield
    finally:
        set_setting(key, old_value)

# Usage
with temporary_setting('debug', 'true'):
    # debug mode enabled
    pass
# debug mode restored
```

---

## Generators and Iterators

### Generator Functions

```python
from typing import Iterator, Generator

def fibonacci(n: int) -> Iterator[int]:
    """Generate Fibonacci sequence."""
    a, b = 0, 1
    for _ in range(n):
        yield a
        a, b = b, a + b

# Generator expression
squares = (x**2 for x in range(10))

# Generator with send()
def running_average() -> Generator[float, float, None]:
    """Calculate running average."""
    total = 0.0
    count = 0
    while True:
        value = yield total / count if count else 0
        total += value
        count += 1
```

### itertools Patterns

```python
from itertools import (
    chain, combinations, groupby, islice,
    permutations, product, cycle, repeat
)

# Flatten nested lists
nested = [[1, 2], [3, 4], [5]]
flattened = list(chain.from_iterable(nested))  # [1, 2, 3, 4, 5]

# Combinations and permutations
list(combinations([1, 2, 3], 2))  # [(1, 2), (1, 3), (2, 3)]
list(permutations([1, 2, 3], 2))  # [(1, 2), (1, 3), (2, 1), ...]

# Groupby
data = [('A', 1), ('A', 2), ('B', 3), ('B', 4)]
for key, group in groupby(data, key=lambda x: x[0]):
    print(f"{key}: {list(group)}")

# Sliding window
def sliding_window(iterable, n):
    iterators = [islice(iterable, i, None) for i in range(n)]
    return zip(*iterators)
```

---

## Data Classes

### Using dataclasses

```python
from dataclasses import dataclass, field, asdict
from typing import List

@dataclass
class User:
    id: int
    name: str
    email: str
    tags: List[str] = field(default_factory=list)
    is_active: bool = True
    
    def __post_init__(self) -> None:
        """Validation after initialization."""
        if '@' not in self.email:
            raise ValueError("Invalid email")

# Usage
user = User(id=1, name="Alice", email="alice@example.com")
user_dict = asdict(user)

# Frozen (immutable) dataclass
@dataclass(frozen=True)
class Point:
    x: int
    y: int

# With ordering
@dataclass(order=True)
class Score:
    value: int
    player: str = field(compare=False)
```

---

## Pythonic Patterns

### List/Dict Comprehensions

```python
# List comprehension
squares = [x**2 for x in range(10)]
evens = [x for x in range(10) if x % 2 == 0]

# Dict comprehension
word_lengths = {word: len(word) for word in ['hello', 'world']}

# Set comprehension
unique_lengths = {len(word) for word in ['hello', 'world', 'hi']}

# Nested comprehension
matrix = [[i * j for j in range(3)] for i in range(3)]

# With conditional
result = [x if x > 0 else 0 for x in [-1, 2, -3, 4]]
```

### Unpacking and Multiple Assignment

```python
# Basic unpacking
a, b = 1, 2
first, *rest = [1, 2, 3, 4]  # first=1, rest=[2,3,4]
*start, last = [1, 2, 3, 4]  # start=[1,2,3], last=4

# Dict unpacking
defaults = {'a': 1, 'b': 2}
overrides = {'b': 3, 'c': 4}
merged = {**defaults, **overrides}  # {'a': 1, 'b': 3, 'c': 4}

# Function call unpacking
def func(a, b, c):
    return a + b + c

args = [1, 2, 3]
result = func(*args)

kwargs = {'a': 1, 'b': 2, 'c': 3}
result = func(**kwargs)

# Walrus operator (Python 3.8+)
if (n := len(data)) > 10:
    print(f"Large dataset: {n} items")
```

### EAFP vs LBYL

```python
# EAFP: Easier to Ask for Forgiveness than Permission (Pythonic)
try:
    value = my_dict[key]
except KeyError:
    value = default_value

# LBYL: Look Before You Leap (less Pythonic)
if key in my_dict:
    value = my_dict[key]
else:
    value = default_value

# EAFP with files
try:
    with open('file.txt') as f:
        data = f.read()
except FileNotFoundError:
    data = default_data
```

---

## Standard Library Gems

### Collections Module

```python
from collections import defaultdict, Counter, deque, namedtuple, ChainMap

# defaultdict - no KeyError
word_count = defaultdict(int)
for word in words:
    word_count[word] += 1

# Counter - count elements
counts = Counter(['a', 'b', 'a', 'c', 'b', 'a'])
counts.most_common(2)  # [('a', 3), ('b', 2)]

# deque - efficient queue
queue = deque([1, 2, 3])
queue.append(4)        # Add to right
queue.appendleft(0)    # Add to left
queue.pop()            # Remove from right
queue.popleft()        # Remove from left

# namedtuple - lightweight class
Point = namedtuple('Point', ['x', 'y'])
p = Point(1, 2)
print(p.x, p.y)

# ChainMap - merge multiple dicts
dict1 = {'a': 1, 'b': 2}
dict2 = {'b': 3, 'c': 4}
combined = ChainMap(dict1, dict2)  # dict1 takes precedence
```

### functools Module

```python
from functools import lru_cache, partial, reduce, singledispatch

# lru_cache - memoization
@lru_cache(maxsize=128)
def fibonacci(n: int) -> int:
    if n < 2:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)

# partial - partial function application
from operator import add
add_five = partial(add, 5)
result = add_five(10)  # 15

# reduce - fold operation
numbers = [1, 2, 3, 4, 5]
total = reduce(lambda x, y: x + y, numbers)  # 15

# singledispatch - function overloading
@singledispatch
def process(value):
    raise NotImplementedError(f"Cannot process {type(value)}")

@process.register
def _(value: int):
    return value * 2

@process.register
def _(value: str):
    return value.upper()
```

### pathlib for File Operations

```python
from pathlib import Path

# Modern path handling
path = Path('data/files/document.txt')

# Path operations
print(path.name)        # 'document.txt'
print(path.stem)        # 'document'
print(path.suffix)      # '.txt'
print(path.parent)      # 'data/files'

# Path joining
config_path = Path.home() / '.config' / 'app' / 'settings.json'

# File operations
path.exists()
path.is_file()
path.is_dir()
path.read_text()
path.write_text('content')

# Iteration
for file in Path('data').glob('*.txt'):
    print(file)

# Create directories
Path('new/nested/dir').mkdir(parents=True, exist_ok=True)
```

---

## pytest Basics (Language Level)

### Basic Test Structure

```python
import pytest

# Simple test
def test_addition():
    assert 1 + 1 == 2

# Test with setup
@pytest.fixture
def sample_data():
    """Fixture providing test data."""
    return [1, 2, 3, 4, 5]

def test_sum(sample_data):
    assert sum(sample_data) == 15

# Parametrized tests
@pytest.mark.parametrize("input,expected", [
    (1, 2),
    (2, 4),
    (3, 6),
])
def test_double(input, expected):
    assert input * 2 == expected

# Testing exceptions
def test_division_by_zero():
    with pytest.raises(ZeroDivisionError):
        1 / 0

# Markers
@pytest.mark.slow
def test_slow_operation():
    pass

@pytest.mark.skip(reason="Not implemented yet")
def test_future_feature():
    pass
```

### Mock and Patch

```python
from unittest.mock import Mock, patch, MagicMock

# Mock object
mock_db = Mock()
mock_db.get_user.return_value = {'id': 1, 'name': 'Alice'}

# Patch function
@patch('module.external_api_call')
def test_with_mock(mock_api):
    mock_api.return_value = {'status': 'ok'}
    # Test code using external_api_call
    assert external_api_call() == {'status': 'ok'}

# Context manager patch
def test_with_context_patch():
    with patch('module.get_data') as mock_get:
        mock_get.return_value = [1, 2, 3]
        result = process_data()
        assert len(result) == 3
```

**Note**: For comprehensive testing strategies, test architecture, and cross-language patterns, use the `testing-expert` skill.

---

## Best Practices

### Python Idioms

- Use `enumerate()` instead of manual counters
- Use `zip()` for parallel iteration
- Use `reversed()` instead of `[::-1]` for readability
- Use `any()` and `all()` for boolean checks
- Use dictionary `.get()` with defaults
- Use `with` for resource management
- Prefer `is` for `None` checks: `if x is None`
- Use `in` for membership tests

### Common Patterns

```python
# Enumerate instead of range(len())
for i, item in enumerate(items):
    print(f"{i}: {item}")

# Zip for parallel iteration
for name, score in zip(names, scores):
    print(f"{name}: {score}")

# Dict.get with default
value = config.get('key', 'default')

# Any/all for boolean checks
has_errors = any(item.has_error() for item in items)
all_valid = all(item.is_valid() for item in items)

# Chain comparisons
if 0 <= value < 100:
    pass
```

---

For web frameworks (FastAPI, Flask), async patterns, HTTP handlers, and microservices architecture, use the **`backend-expert` skill**.

For testing strategies, test architecture, and comprehensive testing patterns, use the **`testing-expert` skill**.
