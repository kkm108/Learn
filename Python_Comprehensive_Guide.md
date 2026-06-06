# The Comprehensive Python Productivity Guide
### Practical, Real-World Examples You Can Run and Modify Today

> **Python version:** 3.10+  
> **Philosophy:** Every example solves a real problem. Adapt the patterns, not just the code.

---

## Table of Contents

1. [Environment Setup & Project Hygiene](#1-environment-setup--project-hygiene)
2. [Core Language — The Parts That Matter Most](#2-core-language--the-parts-that-matter-most)
3. [Data Structures — Choosing the Right Container](#3-data-structures--choosing-the-right-container)
4. [Functions — Writing Code You Want to Call Again](#4-functions--writing-code-you-want-to-call-again)
5. [Classes & Object-Oriented Patterns](#5-classes--object-oriented-patterns)
6. [File & Path Operations](#6-file--path-operations)
7. [Working with Data: CSV, JSON, TOML, YAML](#7-working-with-data-csv-json-toml-yaml)
8. [String Processing & Regular Expressions](#8-string-processing--regular-expressions)
9. [Dates, Times & Scheduling](#9-dates-times--scheduling)
10. [HTTP Requests & APIs](#10-http-requests--apis)
11. [Databases — SQLite & SQLAlchemy](#11-databases--sqlite--sqlalchemy)
12. [Concurrency — Threads, Processes & Async](#12-concurrency--threads-processes--async)
13. [Error Handling & Logging](#13-error-handling--logging)
14. [Testing with pytest](#14-testing-with-pytest)
15. [CLI Tools with argparse & Click](#15-cli-tools-with-argparse--click)
16. [Data Science: pandas, NumPy & Matplotlib](#16-data-science-pandas-numpy--matplotlib)
17. [Automation & System Tasks](#17-automation--system-tasks)
18. [Packaging & Distribution](#18-packaging--distribution)
19. [Performance & Profiling](#19-performance--profiling)
20. [Productivity Patterns & Recipes](#20-productivity-patterns--recipes)

---

## 1. Environment Setup & Project Hygiene

### Virtual environments (built-in)

```python
# Terminal commands — run these before every project
python -m venv .venv                  # create virtual env in .venv/
source .venv/bin/activate             # Linux / macOS
.venv\Scripts\activate                # Windows PowerShell
pip install -r requirements.txt       # install dependencies
pip freeze > requirements.txt         # save current deps
deactivate                            # exit the env
```

### uv — the modern, fast alternative

```bash
pip install uv                        # one-time install
uv venv                               # create .venv (10× faster than venv)
uv pip install requests pandas        # fast installs
uv pip compile requirements.in -o requirements.txt  # lock deps
```

### Recommended project layout

```
my_project/
├── .venv/                  # never commit this
├── src/
│   └── my_project/
│       ├── __init__.py
│       ├── core.py
│       └── utils.py
├── tests/
│   ├── conftest.py
│   └── test_core.py
├── data/
│   └── sample.csv
├── .env                    # secrets — never commit
├── .gitignore
├── pyproject.toml          # modern project config
└── README.md
```

### pyproject.toml — modern project configuration

```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[project]
name = "my-project"
version = "0.1.0"
description = "What this project does"
requires-python = ">=3.10"
dependencies = [
    "requests>=2.31",
    "pandas>=2.0",
]

[project.optional-dependencies]
dev = ["pytest", "ruff", "mypy"]

[tool.ruff]
line-length = 100
select = ["E", "F", "I"]   # errors, pyflakes, isort

[tool.mypy]
strict = true
```

### Loading secrets safely with python-dotenv

```python
# pip install python-dotenv
from dotenv import load_dotenv
import os

load_dotenv()  # reads .env file into environment

DATABASE_URL = os.getenv("DATABASE_URL", "sqlite:///default.db")
API_KEY      = os.environ["API_KEY"]   # raises KeyError if missing — intentional
DEBUG        = os.getenv("DEBUG", "false").lower() == "true"

# .env file (never commit this):
# DATABASE_URL=postgresql://user:pass@localhost/mydb
# API_KEY=sk-abc123
# DEBUG=true
```

---

## 2. Core Language — The Parts That Matter Most

### Unpacking & starred expressions

```python
# Basic unpacking
first, *rest = [1, 2, 3, 4, 5]
# first = 1, rest = [2, 3, 4, 5]

head, *middle, tail = [10, 20, 30, 40, 50]
# head=10, middle=[20,30,40], tail=50

# Swap without a temp variable
a, b = 10, 20
a, b = b, a   # a=20, b=10

# Unpack from a function returning a tuple
def get_bounds(data):
    return min(data), max(data)

low, high = get_bounds([3, 1, 4, 1, 5, 9])
```

### Comprehensions — the right way

```python
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# List comprehension with filter
evens_squared = [x**2 for x in numbers if x % 2 == 0]
# [4, 16, 36, 64, 100]

# Dict comprehension — invert a mapping
original = {"a": 1, "b": 2, "c": 3}
inverted = {v: k for k, v in original.items()}
# {1: "a", 2: "b", 3: "c"}

# Set comprehension — unique first letters
words = ["apple", "avocado", "banana", "apricot"]
first_letters = {w[0] for w in words}
# {"a", "b"}

# Generator expression — lazy, no memory overhead
total = sum(x**2 for x in range(1_000_000))  # no list created

# Nested comprehension — flatten a matrix
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
flat = [cell for row in matrix for cell in row]
# [1, 2, 3, 4, 5, 6, 7, 8, 9]

# Conditional expression (ternary)
label = "even" if 4 % 2 == 0 else "odd"
```

### Walrus operator `:=` (Python 3.8+)

```python
import re

# Avoid computing the same thing twice
data = [1, 5, 2, 8, 3, 7, 4]
if (n := len(data)) > 5:
    print(f"Long list: {n} items")

# Very useful in while loops
import sys
while chunk := sys.stdin.read(8192):
    process(chunk)

# In a comprehension — keep the computed value
results = [y for x in data if (y := x * 2) > 8]
# [10, 16, 14] — y is computed once, used for both filter and result
```

### Structural Pattern Matching (Python 3.10+)

```python
def handle_command(command: dict):
    match command:
        case {"action": "move", "direction": direction}:
            print(f"Moving {direction}")

        case {"action": "attack", "target": target, "weapon": weapon}:
            print(f"Attacking {target} with {weapon}")

        case {"action": "quit"}:
            print("Quitting")

        case _:
            print(f"Unknown command: {command}")


handle_command({"action": "move", "direction": "north"})
handle_command({"action": "attack", "target": "dragon", "weapon": "sword"})


# Pattern matching on types
def describe(value):
    match value:
        case int() | float() as n if n < 0:
            return f"negative number: {n}"
        case str() as s if len(s) > 10:
            return f"long string: {s!r}"
        case [x, y]:
            return f"2-element list: {x}, {y}"
        case {"name": str(name)}:
            return f"dict with name: {name}"
        case None:
            return "nothing"
        case _:
            return f"something else: {value!r}"
```

### f-strings — everything they can do

```python
name = "Alice"
score = 98.5678
items = [1, 2, 3]

# Basic
print(f"Hello, {name}!")

# Format specifiers
print(f"Score: {score:.2f}")          # 98.57
print(f"Score: {score:>10.2f}")       # right-align in 10 chars
print(f"Hex: {255:#010x}")            # 0x000000ff
print(f"Thousands: {1_000_000:,}")    # 1,000,000
print(f"Percent: {0.857:.1%}")        # 85.7%

# Self-documenting expressions (Python 3.8+)
x = 42
print(f"{x = }")          # x = 42
print(f"{x * 2 = }")      # x * 2 = 84

# Expressions inside
print(f"Length: {len(items)}")
print(f"Upper: {name.upper()}")
print(f"Conditional: {'pass' if score >= 60 else 'fail'}")

# Multi-line f-strings
report = (
    f"Student: {name}\n"
    f"Score:   {score:.1f}\n"
    f"Items:   {len(items)}"
)
```

### Type hints — practical usage

```python
from typing import Optional, Union
from collections.abc import Sequence, Callable, Iterator

# Basic function annotations
def greet(name: str, times: int = 1) -> str:
    return (name + "\n") * times

# Optional = can be None
def find_user(user_id: int) -> Optional[dict]:
    ...   # returns dict or None

# Union — multiple types (Python 3.10+: use X | Y)
def process(value: int | str | None) -> str:
    return str(value)

# Collections
def summarise(data: list[float]) -> dict[str, float]:
    return {"mean": sum(data) / len(data), "min": min(data), "max": max(data)}

# Callable
def apply(fn: Callable[[int], int], values: list[int]) -> list[int]:
    return [fn(v) for v in values]

# TypeAlias (Python 3.10+)
type Matrix = list[list[float]]   # Python 3.12+
# or: Matrix = list[list[float]]  # 3.10/3.11
```

---

## 3. Data Structures — Choosing the Right Container

### When to use what

```
list        → ordered, mutable sequence; append/index access
tuple       → ordered, immutable; use as dict key, namedtuple for records
dict        → key-value lookup; insertion-ordered (Python 3.7+)
set         → unique items; fast membership testing; set math
deque       → fast prepend/append on both ends; use as a queue
defaultdict → dict that auto-creates missing keys
Counter     → frequency counting
namedtuple  → lightweight immutable record (like a simple class)
dataclass   → mutable record with methods; better than namedtuple for complex data
heapq       → priority queue
OrderedDict → dict with move_to_end(); mostly superseded by dict
```

### dict — advanced patterns

```python
# Merge dicts (Python 3.9+)
defaults = {"color": "blue", "size": 10, "visible": True}
overrides = {"color": "red", "size": 20}
merged = defaults | overrides
# {"color": "red", "size": 20, "visible": True}

# In-place merge
defaults |= overrides

# .get with a default
user = {"name": "Alice", "age": 30}
email = user.get("email", "no-email@example.com")

# .setdefault — insert only if missing
cache = {}
cache.setdefault("key", []).append("value")  # safe append

# dict.fromkeys — initialise with a default value
keys = ["a", "b", "c"]
counts = dict.fromkeys(keys, 0)  # {"a": 0, "b": 0, "c": 0}

# Destructuring a dict
config = {"host": "localhost", "port": 5432, "db": "mydb"}
host = config["host"]
port, db = config["port"], config["db"]

# Filtering a dict
active_users = {k: v for k, v in users.items() if v["active"]}
```

### defaultdict — clean grouping

```python
from collections import defaultdict

# Group words by first letter
words = ["apple", "avocado", "banana", "cherry", "apricot"]

by_letter = defaultdict(list)
for word in words:
    by_letter[word[0]].append(word)

# {"a": ["apple", "avocado", "apricot"], "b": ["banana"], "c": ["cherry"]}

# Nested defaultdict — adjacency list for a graph
graph = defaultdict(set)
edges = [("A", "B"), ("A", "C"), ("B", "D"), ("C", "D")]
for src, dst in edges:
    graph[src].add(dst)
    graph[dst].add(src)  # undirected
```

### Counter — frequency analysis

```python
from collections import Counter

text = "the quick brown fox jumps over the lazy dog"
word_counts = Counter(text.split())
# Counter({"the": 2, "quick": 1, ...})

# Most common N items
top3 = word_counts.most_common(3)
# [("the", 2), ("quick", 1), ("brown", 1)]

# Arithmetic on Counters
c1 = Counter(["a", "b", "a", "c"])
c2 = Counter(["a", "b", "b", "d"])
combined = c1 + c2   # {"a": 3, "b": 3, "c": 1, "d": 1}
diff      = c1 - c2  # {"a": 1, "c": 1} (only positive results)
```

### deque — fast queues

```python
from collections import deque

# Use as a FIFO queue (list is slow for popleft)
queue = deque()
queue.append("first")
queue.append("second")
queue.append("third")
first = queue.popleft()   # O(1), unlike list.pop(0) which is O(n)

# Bounded deque — keep last N items (rolling window)
log = deque(maxlen=5)
for i in range(10):
    log.append(i)
# deque([5, 6, 7, 8, 9], maxlen=5)

# Rotate
d = deque([1, 2, 3, 4, 5])
d.rotate(2)   # deque([4, 5, 1, 2, 3])
d.rotate(-1)  # deque([5, 1, 2, 3, 4])
```

### dataclasses — modern records

```python
from dataclasses import dataclass, field, asdict, astuple
from datetime import datetime

@dataclass
class Product:
    name:        str
    price:       float
    quantity:    int = 0
    tags:        list[str] = field(default_factory=list)
    created_at:  datetime  = field(default_factory=datetime.now)

    # Computed property
    @property
    def total_value(self) -> float:
        return self.price * self.quantity

    def __post_init__(self):
        # Validation after __init__
        if self.price < 0:
            raise ValueError(f"Price cannot be negative: {self.price}")


p = Product("Widget", 9.99, quantity=100, tags=["sale", "featured"])
print(p.total_value)   # 999.0
print(asdict(p))       # convert to plain dict (great for JSON serialisation)


# Frozen dataclass — immutable, hashable
@dataclass(frozen=True)
class Point:
    x: float
    y: float

    def distance_to(self, other: "Point") -> float:
        return ((self.x - other.x)**2 + (self.y - other.y)**2) ** 0.5


p1, p2 = Point(0, 0), Point(3, 4)
print(p1.distance_to(p2))   # 5.0
points_set = {p1, p2}       # hashable because frozen=True
```

### heapq — priority queue

```python
import heapq

# Min-heap (smallest item always first)
tasks = []
heapq.heappush(tasks, (3, "low priority task"))
heapq.heappush(tasks, (1, "urgent task"))
heapq.heappush(tasks, (2, "normal task"))

while tasks:
    priority, task = heapq.heappop(tasks)
    print(f"[{priority}] {task}")
# [1] urgent task
# [2] normal task
# [3] low priority task

# nlargest / nsmallest — more efficient than sorting for partial results
scores = [42, 87, 13, 95, 61, 78, 55]
top3    = heapq.nlargest(3, scores)   # [95, 87, 78]
bottom3 = heapq.nsmallest(3, scores) # [13, 42, 55]
```

---

## 4. Functions — Writing Code You Want to Call Again

### *args and **kwargs — flexible signatures

```python
# Accept any number of positional args
def mean(*numbers: float) -> float:
    return sum(numbers) / len(numbers)

mean(1, 2, 3)        # 2.0
mean(10, 20, 30, 40) # 25.0

# Accept any keyword args
def create_user(name: str, **attributes) -> dict:
    return {"name": name, **attributes}

create_user("Alice", age=30, role="admin", active=True)
# {"name": "Alice", "age": 30, "role": "admin", "active": True}

# Keyword-only arguments (after *)
def connect(host: str, port: int, *, timeout: int = 30, retries: int = 3):
    ...  # timeout and retries MUST be passed as keywords

connect("localhost", 5432, timeout=10)   # OK
connect("localhost", 5432, 10)           # TypeError — timeout must be keyword

# Positional-only arguments (before /)
def fast_add(x: float, y: float, /) -> float:
    return x + y   # x, y cannot be passed as keywords
```

### Decorators — the practical ones

```python
import functools
import time
import logging

logger = logging.getLogger(__name__)

# 1. Retry decorator
def retry(times: int = 3, delay: float = 1.0, exceptions=(Exception,)):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            last_exc = None
            for attempt in range(1, times + 1):
                try:
                    return func(*args, **kwargs)
                except exceptions as e:
                    last_exc = e
                    logger.warning(f"Attempt {attempt}/{times} failed: {e}")
                    if attempt < times:
                        time.sleep(delay)
            raise last_exc
        return wrapper
    return decorator


@retry(times=3, delay=0.5, exceptions=(ConnectionError, TimeoutError))
def fetch_data(url: str) -> dict:
    ...  # your HTTP call here


# 2. Timer decorator
def timer(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        start = time.perf_counter()
        result = func(*args, **kwargs)
        elapsed = time.perf_counter() - start
        print(f"{func.__name__} took {elapsed:.4f}s")
        return result
    return wrapper


# 3. Memoisation (built-in)
from functools import lru_cache, cache

@cache                         # unbounded cache (Python 3.9+)
def fibonacci(n: int) -> int:
    if n < 2:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)


@lru_cache(maxsize=128)        # bounded LRU cache
def expensive_lookup(key: str) -> str:
    ...  # database query, API call, etc.


# 4. Rate-limiting decorator
def rate_limit(calls_per_second: float):
    min_interval = 1.0 / calls_per_second
    last_called = [0.0]

    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            elapsed = time.monotonic() - last_called[0]
            if elapsed < min_interval:
                time.sleep(min_interval - elapsed)
            last_called[0] = time.monotonic()
            return func(*args, **kwargs)
        return wrapper
    return decorator


@rate_limit(calls_per_second=2)
def call_api(endpoint: str) -> dict:
    ...
```

### Generators — lazy sequences

```python
from typing import Iterator
import os

# Simple generator
def count_up(start: int, stop: int, step: int = 1) -> Iterator[int]:
    current = start
    while current < stop:
        yield current
        current += step

for n in count_up(0, 10, 2):
    print(n)   # 0 2 4 6 8


# Generator for reading large files line-by-line without loading all into memory
def read_lines(filepath: str) -> Iterator[str]:
    with open(filepath, encoding="utf-8") as f:
        for line in f:
            yield line.rstrip("\n")


# Generator pipeline — chain transformations lazily
def parse_log_lines(lines: Iterator[str]) -> Iterator[dict]:
    for line in lines:
        if line.strip() and not line.startswith("#"):
            parts = line.split("|")
            if len(parts) >= 3:
                yield {"timestamp": parts[0].strip(),
                       "level":     parts[1].strip(),
                       "message":   parts[2].strip()}


def filter_errors(records: Iterator[dict]) -> Iterator[dict]:
    return (r for r in records if r["level"] == "ERROR")


# Compose the pipeline — nothing is evaluated until iteration
lines   = read_lines("app.log")
records = parse_log_lines(lines)
errors  = filter_errors(records)

for error in errors:
    print(error)


# Send values into a generator (coroutine-style)
def running_average() -> Iterator[float]:
    total, count = 0.0, 0
    while True:
        value = yield total / count if count else 0.0
        total += value
        count += 1

avg = running_average()
next(avg)          # prime it
avg.send(10)       # 10.0
avg.send(20)       # 15.0
avg.send(30)       # 20.0
```

### Context managers — clean resource handling

```python
from contextlib import contextmanager, suppress
import tempfile

# contextmanager decorator — simplest way to write one
@contextmanager
def timer_cm(label: str = ""):
    start = time.perf_counter()
    try:
        yield
    finally:
        elapsed = time.perf_counter() - start
        print(f"{label}: {elapsed:.4f}s")


with timer_cm("data processing"):
    data = [x**2 for x in range(1_000_000)]


# Suppress specific exceptions cleanly
with suppress(FileNotFoundError):
    os.remove("temp_file_that_may_not_exist.txt")


# Temporary files / directories
with tempfile.NamedTemporaryFile(suffix=".csv", delete=False) as f:
    f.write(b"name,age\nAlice,30\n")
    temp_path = f.name

# temp_path is available here; file is on disk


# Multiple context managers on one line
with open("input.txt") as src, open("output.txt", "w") as dst:
    dst.write(src.read().upper())


# Class-based context manager
class DatabaseTransaction:
    def __init__(self, conn):
        self.conn = conn

    def __enter__(self):
        self.conn.begin()
        return self.conn

    def __exit__(self, exc_type, exc_val, exc_tb):
        if exc_type:
            self.conn.rollback()
            return False  # re-raise the exception
        self.conn.commit()
        return True
```

---

## 5. Classes & Object-Oriented Patterns

### Dunder methods — making classes feel native

```python
class Vector:
    def __init__(self, x: float, y: float):
        self.x = x
        self.y = y

    # String representations
    def __repr__(self) -> str:
        return f"Vector({self.x}, {self.y})"

    def __str__(self) -> str:
        return f"({self.x}, {self.y})"

    # Arithmetic
    def __add__(self, other: "Vector") -> "Vector":
        return Vector(self.x + other.x, self.y + other.y)

    def __sub__(self, other: "Vector") -> "Vector":
        return Vector(self.x - other.x, self.y - other.y)

    def __mul__(self, scalar: float) -> "Vector":
        return Vector(self.x * scalar, self.y * scalar)

    def __rmul__(self, scalar: float) -> "Vector":
        return self.__mul__(scalar)  # makes 3 * v work too

    def __abs__(self) -> float:
        return (self.x**2 + self.y**2) ** 0.5

    # Comparison
    def __eq__(self, other: object) -> bool:
        if not isinstance(other, Vector):
            return NotImplemented
        return self.x == other.x and self.y == other.y

    def __lt__(self, other: "Vector") -> bool:
        return abs(self) < abs(other)

    # Make it iterable
    def __iter__(self):
        yield self.x
        yield self.y

    # Length
    def __len__(self) -> int:
        return 2

    # Index access: v[0], v[1]
    def __getitem__(self, index: int) -> float:
        return (self.x, self.y)[index]


v1 = Vector(3, 4)
v2 = Vector(1, 2)
print(v1 + v2)     # (4, 6)
print(abs(v1))     # 5.0
print(3 * v1)      # (9, 12)
x, y = v1          # iterable unpacking
```

### Properties — controlled attribute access

```python
class Temperature:
    def __init__(self, celsius: float = 0.0):
        self._celsius = celsius    # private backing attribute

    @property
    def celsius(self) -> float:
        return self._celsius

    @celsius.setter
    def celsius(self, value: float):
        if value < -273.15:
            raise ValueError(f"Temperature below absolute zero: {value}")
        self._celsius = value

    @property
    def fahrenheit(self) -> float:
        return self._celsius * 9/5 + 32

    @fahrenheit.setter
    def fahrenheit(self, value: float):
        self.celsius = (value - 32) * 5/9   # uses the celsius setter


t = Temperature(100)
print(t.fahrenheit)    # 212.0
t.fahrenheit = 32
print(t.celsius)       # 0.0
```

### Class methods, static methods & `__slots__`

```python
from datetime import date

class Employee:
    __slots__ = ("name", "department", "salary", "_hire_date")  # memory saving

    def __init__(self, name: str, department: str, salary: float, hire_date: date):
        self.name = name
        self.department = department
        self.salary = salary
        self._hire_date = hire_date

    @classmethod
    def from_dict(cls, data: dict) -> "Employee":
        return cls(
            name=data["name"],
            department=data["department"],
            salary=float(data["salary"]),
            hire_date=date.fromisoformat(data["hire_date"]),
        )

    @classmethod
    def from_csv_row(cls, row: str) -> "Employee":
        name, dept, salary, hired = row.split(",")
        return cls(name.strip(), dept.strip(), float(salary), date.fromisoformat(hired.strip()))

    @staticmethod
    def is_valid_salary(salary: float) -> bool:
        return 0 < salary < 10_000_000

    @property
    def years_of_service(self) -> int:
        return (date.today() - self._hire_date).days // 365

    def __repr__(self):
        return f"Employee({self.name!r}, {self.department!r}, {self.salary:.2f})"


e = Employee.from_dict({
    "name": "Alice", "department": "Engineering",
    "salary": "95000", "hire_date": "2019-06-01"
})
```

### Protocols — structural subtyping (duck typing + type safety)

```python
from typing import Protocol, runtime_checkable

@runtime_checkable
class Serialisable(Protocol):
    def to_dict(self) -> dict: ...
    def to_json(self) -> str: ...


@runtime_checkable
class Closeable(Protocol):
    def close(self) -> None: ...


def save(obj: Serialisable, path: str) -> None:
    import json
    with open(path, "w") as f:
        json.dump(obj.to_dict(), f, indent=2)


# Any class with to_dict() and to_json() satisfies Serialisable
# — no inheritance required
class Config:
    def __init__(self, data: dict):
        self.data = data

    def to_dict(self) -> dict:
        return self.data

    def to_json(self) -> str:
        import json
        return json.dumps(self.data)


save(Config({"debug": True}), "config.json")  # works
```

---

## 6. File & Path Operations

### pathlib — the modern way

```python
from pathlib import Path

# Construction
home    = Path.home()                    # /home/username
cwd     = Path.cwd()                     # current working directory
config  = Path("/etc/myapp/config.toml")
data    = Path("data") / "processed" / "output.csv"  # cross-platform join

# Inspection
print(data.name)       # output.csv
print(data.stem)       # output
print(data.suffix)     # .csv
print(data.parent)     # data/processed
print(data.parts)      # ('data', 'processed', 'output.csv')

# Existence checks
data.exists()
data.is_file()
data.is_dir()
data.is_symlink()

# Reading / writing (small files)
text = Path("notes.txt").read_text(encoding="utf-8")
Path("notes.txt").write_text("Hello\n", encoding="utf-8")
raw  = Path("image.png").read_bytes()
Path("copy.png").write_bytes(raw)

# Directory operations
Path("output/reports").mkdir(parents=True, exist_ok=True)
Path("old_name.txt").rename("new_name.txt")
Path("file.txt").unlink(missing_ok=True)      # delete (no error if absent)

# Glob patterns
py_files   = list(Path(".").rglob("*.py"))           # all .py recursively
test_files = list(Path("tests").glob("test_*.py"))   # direct children

# Iterate directory
for entry in Path("data").iterdir():
    if entry.is_file() and entry.suffix == ".csv":
        print(entry, entry.stat().st_size)

# Safe open (no manual string concatenation)
with (Path("data") / "input.txt").open(encoding="utf-8") as f:
    for line in f:
        ...

# Replace extension
csv_path  = Path("report.xlsx")
json_path = csv_path.with_suffix(".json")   # report.json

# Relative path from a base
base    = Path("/home/alice/projects")
target  = Path("/home/alice/projects/api/src/main.py")
rel     = target.relative_to(base)          # api/src/main.py
```

### Reading large files efficiently

```python
# Line by line — constant memory
with open("huge.log", encoding="utf-8") as f:
    for line in f:
        process(line.rstrip())

# Chunked binary read
CHUNK = 64 * 1024  # 64 KB
with open("large_file.bin", "rb") as f:
    while data := f.read(CHUNK):
        process_chunk(data)

# CSV row by row (see section 7 for full CSV guide)
import csv
with open("data.csv", newline="", encoding="utf-8") as f:
    for row in csv.DictReader(f):
        process_row(row)
```

### File watching (real-time change detection)

```python
# pip install watchdog
from watchdog.observers import Observer
from watchdog.events import FileSystemEventHandler
import time

class ChangeHandler(FileSystemEventHandler):
    def on_modified(self, event):
        if not event.is_directory:
            print(f"Modified: {event.src_path}")

    def on_created(self, event):
        print(f"Created: {event.src_path}")

    def on_deleted(self, event):
        print(f"Deleted: {event.src_path}")


observer = Observer()
observer.schedule(ChangeHandler(), path="./data", recursive=True)
observer.start()

try:
    while True:
        time.sleep(1)
finally:
    observer.stop()
    observer.join()
```

---

## 7. Working with Data: CSV, JSON, TOML, YAML

### CSV — reading and writing

```python
import csv
from pathlib import Path

# Read into list of dicts
def read_csv(path: str | Path) -> list[dict]:
    with open(path, newline="", encoding="utf-8") as f:
        return list(csv.DictReader(f))


# Write from list of dicts
def write_csv(path: str | Path, rows: list[dict], fieldnames: list[str] | None = None):
    if not rows:
        return
    fieldnames = fieldnames or list(rows[0].keys())
    with open(path, "w", newline="", encoding="utf-8") as f:
        writer = csv.DictWriter(f, fieldnames=fieldnames)
        writer.writeheader()
        writer.writerows(rows)


# Transform CSV — filter and reshape
def filter_csv(src: str, dst: str, min_salary: float):
    with open(src, newline="") as fin, open(dst, "w", newline="") as fout:
        reader = csv.DictReader(fin)
        writer = csv.DictWriter(fout, fieldnames=["name", "department", "salary"])
        writer.writeheader()
        for row in reader:
            if float(row["salary"]) >= min_salary:
                writer.writerow({k: row[k] for k in writer.fieldnames})
```

### JSON — reading, writing, custom types

```python
import json
from datetime import datetime, date
from dataclasses import dataclass, asdict
from pathlib import Path

# Basic read / write
config = json.loads('{"host": "localhost", "port": 5432}')
json_str = json.dumps(config, indent=2, sort_keys=True)

# File read / write
def load_json(path: str | Path) -> dict | list:
    with open(path, encoding="utf-8") as f:
        return json.load(f)

def save_json(data: dict | list, path: str | Path, indent: int = 2):
    with open(path, "w", encoding="utf-8") as f:
        json.dump(data, f, indent=indent, ensure_ascii=False)


# Custom encoder — handles types JSON doesn't know about
class AppEncoder(json.JSONEncoder):
    def default(self, obj):
        if isinstance(obj, (datetime, date)):
            return obj.isoformat()
        if isinstance(obj, Path):
            return str(obj)
        if hasattr(obj, "__dataclass_fields__"):
            return asdict(obj)
        return super().default(obj)


data = {"created_at": datetime.now(), "path": Path("/tmp/out")}
json.dumps(data, cls=AppEncoder, indent=2)


# JSONL — one JSON object per line (great for logs, streaming data)
def write_jsonl(records: list[dict], path: str):
    with open(path, "w", encoding="utf-8") as f:
        for record in records:
            f.write(json.dumps(record, cls=AppEncoder) + "\n")


def read_jsonl(path: str):
    with open(path, encoding="utf-8") as f:
        return [json.loads(line) for line in f if line.strip()]
```

### TOML (Python 3.11+ built-in)

```python
import tomllib    # read-only, built-in since 3.11
# pip install tomli for 3.10, pip install tomli-w for writing

# Read
with open("pyproject.toml", "rb") as f:
    config = tomllib.load(f)

project_name = config["project"]["name"]
dependencies = config["project"]["dependencies"]

# Write (requires tomli-w)
import tomli_w

config = {
    "app": {
        "name": "MyApp",
        "version": "1.0.0",
        "debug": False,
    },
    "database": {
        "host": "localhost",
        "port": 5432,
    },
}
with open("config.toml", "wb") as f:
    tomli_w.dump(config, f)
```

### YAML

```python
# pip install pyyaml
import yaml
from pathlib import Path

# Read
def load_yaml(path: str | Path) -> dict:
    with open(path, encoding="utf-8") as f:
        return yaml.safe_load(f)  # always safe_load, never yaml.load()


# Write
def save_yaml(data: dict, path: str | Path):
    with open(path, "w", encoding="utf-8") as f:
        yaml.dump(data, f, default_flow_style=False, allow_unicode=True, indent=2)


# Multi-document YAML
def load_all_yaml(path: str) -> list[dict]:
    with open(path) as f:
        return list(yaml.safe_load_all(f))
```

---

## 8. String Processing & Regular Expressions

### str methods you should know

```python
text = "  Hello, World!  "

# Cleanup
text.strip()           # "Hello, World!"
text.lstrip()          # "Hello, World!  "
text.rstrip()          # "  Hello, World!"
text.removeprefix("  Hello, ")  # "World!  "  (Python 3.9+)
text.removesuffix("!")           # "  Hello, World  "

# Splitting
"a,b,,c".split(",")                   # ["a", "b", "", "c"]
"a,b,,c".split(",", maxsplit=2)       # ["a", "b", ",c"]
"line1\nline2\nline3".splitlines()    # ["line1", "line2", "line3"]

# Joining
", ".join(["Alice", "Bob", "Carol"])  # "Alice, Bob, Carol"
"\n".join(lines)

# Checking
"hello".startswith("he")     # True
"hello".endswith(("lo","y")) # True — accepts tuple
"abc123".isdigit()           # False
"123".isdigit()              # True
"hello world".isalpha()      # False
" \t\n".isspace()            # True

# Replacement
"cat sat mat".replace("at", "ot")       # "cot sot mot"
"cat sat mat".replace("at", "ot", 1)   # "cot sat mat" — max 1 replacement

# Case
"hello WORLD".lower()       # "hello world"
"hello WORLD".upper()       # "HELLO WORLD"
"hello world".title()       # "Hello World"
"hello WORLD".swapcase()    # "HELLO world"
"hello world".capitalize()  # "Hello world"

# Finding
s = "find the needle in the haystack"
s.find("needle")            # 9 (index, or -1 if not found)
s.index("needle")           # 9 (raises ValueError if not found)
s.count("the")              # 2
"needle" in s               # True

# Formatting
"%-20s | %6.2f" % ("Alice", 98.5)         # old style
"{:<20} | {:>6.2f}".format("Alice", 98.5) # .format()
f"{'Alice':<20} | {98.5:>6.2f}"           # f-string
```

### Regular expressions — practical patterns

```python
import re

# Basic search
text = "Call us at 555-1234 or 555-5678 today!"

match = re.search(r'\d{3}-\d{4}', text)
if match:
    print(match.group())   # "555-1234" (first match)

# All matches
all_phones = re.findall(r'\d{3}-\d{4}', text)
# ["555-1234", "555-5678"]

# Named groups — the most readable pattern
LOG_PATTERN = re.compile(
    r'(?P<timestamp>\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2})'
    r'\s+(?P<level>\w+)'
    r'\s+(?P<message>.+)'
)
m = LOG_PATTERN.match("2024-01-15 14:30:00 ERROR Connection refused")
if m:
    print(m.group("timestamp"))  # "2024-01-15 14:30:00"
    print(m.group("level"))      # "ERROR"
    print(m.groupdict())         # full dict

# Substitution with a function
def redact_card(match: re.Match) -> str:
    card = match.group()
    return "*" * (len(card) - 4) + card[-4:]

clean = re.sub(r'\b\d{4}[ -]?\d{4}[ -]?\d{4}[ -]?\d{4}\b',
               redact_card,
               "Card: 4532 1234 5678 9012")
# "Card: ************9012"

# Split on multiple delimiters
parts = re.split(r'[;,\s]+', "one, two;three  four")
# ["one", "two", "three", "four"]

# Compiled patterns — reuse in loops
EMAIL_RE = re.compile(r'[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}')
URL_RE   = re.compile(r'https?://[^\s<>"]+')
IP_RE    = re.compile(r'\b(?:\d{1,3}\.){3}\d{1,3}\b')

# Common patterns library
PATTERNS = {
    "email":    r'[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}',
    "url":      r'https?://[^\s<>"]+',
    "ip":       r'\b(?:\d{1,3}\.){3}\d{1,3}\b',
    "date_iso": r'\d{4}-\d{2}-\d{2}',
    "phone_us": r'\b\d{3}[-.]?\d{3}[-.]?\d{4}\b',
    "hex_color": r'#(?:[0-9a-fA-F]{3}){1,2}\b',
    "slug":     r'^[a-z0-9]+(?:-[a-z0-9]+)*$',
}
```

---

## 9. Dates, Times & Scheduling

### datetime — the essentials

```python
from datetime import datetime, date, timedelta, timezone

# Now
now     = datetime.now()                   # local naive datetime
now_utc = datetime.now(tz=timezone.utc)   # UTC-aware datetime

# Parse from string
dt = datetime.strptime("2024-01-15 14:30:00", "%Y-%m-%d %H:%M:%S")
dt = datetime.fromisoformat("2024-01-15T14:30:00")  # ISO 8601

# Format to string
dt.strftime("%d %B %Y")           # "15 January 2024"
dt.isoformat()                    # "2024-01-15T14:30:00"
dt.strftime("%Y%m%d_%H%M%S")     # "20240115_143000" — good for filenames

# Arithmetic
tomorrow   = date.today() + timedelta(days=1)
next_week  = datetime.now() + timedelta(weeks=1)
two_hours  = datetime.now() + timedelta(hours=2)
diff       = datetime(2025, 1, 1) - datetime.now()  # timedelta
days_left  = diff.days

# Date ranges
def date_range(start: date, end: date, step: int = 1):
    current = start
    while current <= end:
        yield current
        current += timedelta(days=step)

for d in date_range(date(2024, 1, 1), date(2024, 1, 7)):
    print(d)

# Timezone-aware (use zoneinfo, Python 3.9+)
from zoneinfo import ZoneInfo
ny_tz  = ZoneInfo("America/New_York")
ist_tz = ZoneInfo("Asia/Kolkata")

ny_time  = datetime.now(tz=ny_tz)
ist_time = ny_time.astimezone(ist_tz)
print(f"NY:  {ny_time:%Y-%m-%d %H:%M %Z}")
print(f"IST: {ist_time:%Y-%m-%d %H:%M %Z}")
```

### Schedule recurring tasks

```python
# pip install schedule
import schedule
import time

def job_backup():
    print("Running backup...")

def job_report():
    print("Generating daily report...")

def job_cleanup():
    print("Cleaning temp files...")

# Define schedule
schedule.every().hour.do(job_backup)
schedule.every().day.at("02:00").do(job_report)
schedule.every().monday.at("09:00").do(job_cleanup)
schedule.every(30).minutes.do(job_backup)
schedule.every(5).seconds.do(lambda: print("ping"))  # quick check

# Run loop
while True:
    schedule.run_pending()
    time.sleep(1)
```

---

## 10. HTTP Requests & APIs

### requests — the standard library for HTTP

```python
# pip install requests
import requests
from requests.adapters import HTTPAdapter
from urllib3.util.retry import Retry

# Create a session with automatic retries
def make_session(
    retries: int = 3,
    backoff: float = 0.5,
    status_forcelist: tuple = (429, 500, 502, 503, 504),
) -> requests.Session:
    session = requests.Session()
    retry = Retry(
        total=retries,
        backoff_factor=backoff,
        status_forcelist=status_forcelist,
        allowed_methods=["GET", "POST"],
    )
    adapter = HTTPAdapter(max_retries=retry)
    session.mount("http://", adapter)
    session.mount("https://", adapter)
    return session


session = make_session()

# GET with params
resp = session.get(
    "https://api.example.com/users",
    params={"page": 1, "per_page": 50, "active": True},
    headers={"Authorization": f"Bearer {API_KEY}"},
    timeout=10,
)
resp.raise_for_status()   # raises HTTPError for 4xx/5xx
data = resp.json()

# POST JSON
resp = session.post(
    "https://api.example.com/users",
    json={"name": "Alice", "email": "alice@example.com"},
    headers={"Authorization": f"Bearer {API_KEY}"},
    timeout=10,
)
resp.raise_for_status()
new_user = resp.json()

# Download a file with progress
def download_file(url: str, dest: str, chunk_size: int = 8192):
    with session.get(url, stream=True, timeout=30) as resp:
        resp.raise_for_status()
        total = int(resp.headers.get("content-length", 0))
        received = 0
        with open(dest, "wb") as f:
            for chunk in resp.iter_content(chunk_size=chunk_size):
                f.write(chunk)
                received += len(chunk)
                if total:
                    pct = received / total * 100
                    print(f"\r{pct:.1f}%", end="", flush=True)
    print()


# Paginated API helper
def get_all_pages(base_url: str, headers: dict, page_key: str = "page") -> list:
    all_items = []
    page = 1
    while True:
        resp = session.get(base_url, headers=headers, params={page_key: page}, timeout=10)
        resp.raise_for_status()
        data = resp.json()
        items = data.get("items") or data.get("results") or data
        if not items:
            break
        all_items.extend(items)
        if not data.get("next"):  # no next page
            break
        page += 1
    return all_items
```

### httpx — async-capable modern alternative

```python
# pip install httpx
import httpx
import asyncio

async def fetch_many(urls: list[str]) -> list[dict]:
    async with httpx.AsyncClient(timeout=10) as client:
        tasks = [client.get(url) for url in urls]
        responses = await asyncio.gather(*tasks)
        return [r.json() for r in responses]


results = asyncio.run(fetch_many([
    "https://api.example.com/items/1",
    "https://api.example.com/items/2",
    "https://api.example.com/items/3",
]))
```

---

## 11. Databases — SQLite & SQLAlchemy

### sqlite3 — built-in, zero dependencies

```python
import sqlite3
from contextlib import contextmanager
from pathlib import Path

DB_PATH = Path("app.db")

@contextmanager
def get_connection(path: Path = DB_PATH):
    conn = sqlite3.connect(path)
    conn.row_factory = sqlite3.Row   # rows behave like dicts
    conn.execute("PRAGMA journal_mode=WAL")   # better concurrent reads
    conn.execute("PRAGMA foreign_keys=ON")
    try:
        yield conn
        conn.commit()
    except Exception:
        conn.rollback()
        raise
    finally:
        conn.close()


# Initialise schema
SCHEMA = """
CREATE TABLE IF NOT EXISTS users (
    id          INTEGER PRIMARY KEY AUTOINCREMENT,
    name        TEXT    NOT NULL,
    email       TEXT    NOT NULL UNIQUE,
    created_at  TEXT    DEFAULT (datetime('now')),
    active      INTEGER DEFAULT 1
);

CREATE INDEX IF NOT EXISTS idx_users_email ON users(email);
"""

def init_db():
    with get_connection() as conn:
        conn.executescript(SCHEMA)


# CRUD helpers
def create_user(name: str, email: str) -> int:
    with get_connection() as conn:
        cur = conn.execute(
            "INSERT INTO users (name, email) VALUES (?, ?)",
            (name, email)
        )
        return cur.lastrowid


def get_user(user_id: int) -> sqlite3.Row | None:
    with get_connection() as conn:
        return conn.execute(
            "SELECT * FROM users WHERE id = ?", (user_id,)
        ).fetchone()


def list_users(active_only: bool = True) -> list[sqlite3.Row]:
    with get_connection() as conn:
        query = "SELECT * FROM users"
        if active_only:
            query += " WHERE active = 1"
        return conn.execute(query + " ORDER BY name").fetchall()


def update_user(user_id: int, **fields) -> int:
    if not fields:
        return 0
    set_clause = ", ".join(f"{k} = ?" for k in fields)
    values = list(fields.values()) + [user_id]
    with get_connection() as conn:
        cur = conn.execute(
            f"UPDATE users SET {set_clause} WHERE id = ?", values
        )
        return cur.rowcount


def delete_user(user_id: int) -> int:
    with get_connection() as conn:
        cur = conn.execute("DELETE FROM users WHERE id = ?", (user_id,))
        return cur.rowcount


# Bulk insert
def bulk_insert_users(users: list[dict]):
    with get_connection() as conn:
        conn.executemany(
            "INSERT OR IGNORE INTO users (name, email) VALUES (:name, :email)",
            users
        )
```

### SQLAlchemy 2.x ORM

```python
# pip install sqlalchemy
from sqlalchemy import create_engine, String, Integer, Boolean, select, func
from sqlalchemy.orm import DeclarativeBase, Mapped, mapped_column, Session
from datetime import datetime

engine = create_engine("sqlite:///app.db", echo=False)


class Base(DeclarativeBase):
    pass


class User(Base):
    __tablename__ = "users"

    id:         Mapped[int]      = mapped_column(primary_key=True)
    name:       Mapped[str]      = mapped_column(String(100))
    email:      Mapped[str]      = mapped_column(String(255), unique=True)
    active:     Mapped[bool]     = mapped_column(Boolean, default=True)
    created_at: Mapped[datetime] = mapped_column(default=datetime.utcnow)

    def __repr__(self):
        return f"<User {self.email}>"


Base.metadata.create_all(engine)


# CRUD
def add_user(name: str, email: str) -> User:
    with Session(engine) as session:
        user = User(name=name, email=email)
        session.add(user)
        session.commit()
        session.refresh(user)
        return user


def get_active_users() -> list[User]:
    with Session(engine) as session:
        return session.scalars(
            select(User).where(User.active == True).order_by(User.name)
        ).all()


def count_users() -> int:
    with Session(engine) as session:
        return session.scalar(select(func.count()).select_from(User))
```

---

## 12. Concurrency — Threads, Processes & Async

### threading — I/O-bound work

```python
import threading
from concurrent.futures import ThreadPoolExecutor, as_completed
import requests

urls = [f"https://httpbin.org/delay/{i % 3}" for i in range(10)]

# ThreadPoolExecutor — high level, preferred
def fetch(url: str) -> dict:
    resp = requests.get(url, timeout=10)
    return {"url": url, "status": resp.status_code}


with ThreadPoolExecutor(max_workers=5) as executor:
    futures = {executor.submit(fetch, url): url for url in urls}
    for future in as_completed(futures):
        url = futures[future]
        try:
            result = future.result()
            print(result)
        except Exception as e:
            print(f"Failed {url}: {e}")


# Thread-safe counter using a Lock
class SafeCounter:
    def __init__(self):
        self._value = 0
        self._lock  = threading.Lock()

    def increment(self):
        with self._lock:
            self._value += 1

    @property
    def value(self):
        with self._lock:
            return self._value
```

### multiprocessing — CPU-bound work

```python
from concurrent.futures import ProcessPoolExecutor
import math

def is_prime(n: int) -> bool:
    if n < 2: return False
    for i in range(2, int(math.sqrt(n)) + 1):
        if n % i == 0:
            return False
    return True

numbers = range(100_000, 100_200)

with ProcessPoolExecutor() as executor:   # uses all CPU cores
    results = list(executor.map(is_prime, numbers))

primes = [n for n, prime in zip(numbers, results) if prime]
```

### asyncio — many concurrent I/O tasks

```python
import asyncio
import httpx

async def fetch(client: httpx.AsyncClient, url: str) -> dict:
    resp = await client.get(url, timeout=10)
    return {"url": url, "status": resp.status_code, "size": len(resp.content)}


async def main(urls: list[str]) -> list[dict]:
    async with httpx.AsyncClient() as client:
        tasks = [fetch(client, url) for url in urls]
        return await asyncio.gather(*tasks, return_exceptions=True)


# Limit concurrency with a semaphore
async def main_limited(urls: list[str], limit: int = 10) -> list[dict]:
    semaphore = asyncio.Semaphore(limit)

    async def bounded_fetch(client, url):
        async with semaphore:
            return await fetch(client, url)

    async with httpx.AsyncClient() as client:
        tasks = [bounded_fetch(client, url) for url in urls]
        return await asyncio.gather(*tasks, return_exceptions=True)


results = asyncio.run(main_limited(urls, limit=5))


# Async queue — producer / consumer pattern
async def producer(queue: asyncio.Queue, items: list):
    for item in items:
        await queue.put(item)
    await queue.put(None)   # sentinel


async def consumer(queue: asyncio.Queue):
    while (item := await queue.get()) is not None:
        await asyncio.sleep(0.1)  # simulate work
        print(f"Processed: {item}")
        queue.task_done()


async def pipeline():
    queue = asyncio.Queue(maxsize=10)
    await asyncio.gather(
        producer(queue, list(range(20))),
        consumer(queue),
    )
```

---

## 13. Error Handling & Logging

### Exception hierarchy and best practices

```python
# Custom exception hierarchy
class AppError(Exception):
    """Base for all application errors."""

class ConfigError(AppError):
    """Bad or missing configuration."""

class NetworkError(AppError):
    """Network-related failures."""
    def __init__(self, message: str, url: str, status_code: int | None = None):
        super().__init__(message)
        self.url = url
        self.status_code = status_code

class ValidationError(AppError):
    def __init__(self, field: str, value, reason: str):
        super().__init__(f"Validation failed for '{field}': {reason} (got {value!r})")
        self.field  = field
        self.value  = value
        self.reason = reason


# Catching and re-raising with context
def load_config(path: str) -> dict:
    try:
        import tomllib
        with open(path, "rb") as f:
            return tomllib.load(f)
    except FileNotFoundError as e:
        raise ConfigError(f"Config file not found: {path}") from e
    except Exception as e:
        raise ConfigError(f"Failed to parse config: {e}") from e


# Exception groups (Python 3.11+)
async def fetch_all(urls):
    errors = []
    results = []
    for url in urls:
        try:
            results.append(await fetch(url))
        except NetworkError as e:
            errors.append(e)
    if errors:
        raise ExceptionGroup("fetch errors", errors)
    return results
```

### Logging — structured and production-ready

```python
import logging
import logging.handlers
import sys
from pathlib import Path

def setup_logging(
    level: int = logging.INFO,
    log_file: str | None = None,
    json_format: bool = False,
) -> logging.Logger:
    handlers: list[logging.Handler] = []

    # Console handler
    console = logging.StreamHandler(sys.stdout)
    console.setLevel(level)
    handlers.append(console)

    # Rotating file handler
    if log_file:
        file_handler = logging.handlers.RotatingFileHandler(
            log_file, maxBytes=10 * 1024 * 1024, backupCount=5, encoding="utf-8"
        )
        file_handler.setLevel(logging.DEBUG)
        handlers.append(file_handler)

    if json_format:
        # pip install python-json-logger
        from pythonjsonlogger import jsonlogger
        formatter = jsonlogger.JsonFormatter(
            "%(asctime)s %(name)s %(levelname)s %(message)s"
        )
    else:
        formatter = logging.Formatter(
            fmt="%(asctime)s | %(levelname)-8s | %(name)s | %(message)s",
            datefmt="%Y-%m-%d %H:%M:%S",
        )

    for handler in handlers:
        handler.setFormatter(formatter)

    root = logging.getLogger()
    root.setLevel(logging.DEBUG)
    root.handlers.clear()
    for handler in handlers:
        root.addHandler(handler)

    return logging.getLogger("app")


logger = setup_logging(log_file="app.log")

# Usage
logger.debug("Detailed debugging info")
logger.info("User %s logged in", "alice")
logger.warning("Disk usage at %d%%", 85)
logger.error("Failed to connect to %s", "db-host")
logger.exception("Unhandled error")   # includes traceback automatically

# Contextual logging
import logging
class RequestLogger:
    def __init__(self, request_id: str):
        self.logger = logging.LoggerAdapter(
            logging.getLogger("requests"),
            {"request_id": request_id}
        )

    def info(self, msg, *args, **kwargs):
        self.logger.info(msg, *args, **kwargs)
```

---

## 14. Testing with pytest

### Test structure and fixtures

```python
# tests/conftest.py
import pytest
from pathlib import Path
import tempfile
import sqlite3


@pytest.fixture
def tmp_db(tmp_path: Path):
    """Temporary SQLite database, cleaned up after each test."""
    db_path = tmp_path / "test.db"
    conn = sqlite3.connect(db_path)
    conn.execute("CREATE TABLE users (id INTEGER PRIMARY KEY, name TEXT, email TEXT UNIQUE)")
    conn.commit()
    yield db_path
    conn.close()


@pytest.fixture(scope="session")
def sample_data():
    """Loaded once per test session — expensive setup."""
    return [
        {"name": "Alice", "email": "alice@example.com", "score": 95},
        {"name": "Bob",   "email": "bob@example.com",   "score": 78},
        {"name": "Carol", "email": "carol@example.com", "score": 88},
    ]


@pytest.fixture
def mock_api(requests_mock):
    """Mock HTTP responses — pip install requests-mock"""
    requests_mock.get(
        "https://api.example.com/users",
        json={"users": [{"id": 1, "name": "Alice"}]}
    )
    return requests_mock
```

```python
# tests/test_core.py
import pytest
from unittest.mock import patch, MagicMock, call


# Basic test
def test_mean():
    assert mean(1, 2, 3) == 2.0
    assert mean(10) == 10.0


# Parametrised tests
@pytest.mark.parametrize("value, expected", [
    (0, "zero"),
    (1, "positive"),
    (-1, "negative"),
    (1_000_000, "positive"),
])
def test_classify(value, expected):
    assert classify_number(value) == expected


# Exceptions
def test_invalid_salary():
    with pytest.raises(ValueError, match="negative"):
        Employee("Alice", "Eng", salary=-1000, hire_date=date.today())


# Mocking
def test_fetch_user(mock_api):
    result = fetch_user(1)
    assert result["name"] == "Alice"


def test_send_email():
    with patch("myapp.email.smtplib.SMTP") as mock_smtp:
        send_welcome_email("alice@example.com")
        mock_smtp.assert_called_once()
        instance = mock_smtp.return_value.__enter__.return_value
        instance.sendmail.assert_called_once()


# Approx comparisons for floats
def test_distance():
    assert compute_distance(0, 0, 3, 4) == pytest.approx(5.0)
    assert compute_distance(0, 0, 1, 1) == pytest.approx(1.414, rel=1e-3)


# Fixtures with tmp_path (built-in pytest fixture)
def test_write_csv(tmp_path):
    output = tmp_path / "output.csv"
    write_csv(output, [{"name": "Alice", "score": 95}])
    assert output.exists()
    content = output.read_text()
    assert "Alice" in content
    assert "95" in content
```

### Running tests

```bash
pytest                          # run all tests
pytest tests/test_core.py       # specific file
pytest -k "test_user"           # tests matching a keyword
pytest -v                       # verbose — show each test name
pytest -x                       # stop on first failure
pytest --tb=short               # short traceback
pytest --cov=src --cov-report=html   # coverage (pip install pytest-cov)
pytest -n auto                  # parallel tests (pip install pytest-xdist)
```

---

## 15. CLI Tools with argparse & Click

### argparse — built-in

```python
import argparse
import sys

def create_parser() -> argparse.ArgumentParser:
    parser = argparse.ArgumentParser(
        description="Process CSV files and generate reports",
        formatter_class=argparse.RawDescriptionHelpFormatter,
        epilog="""
Examples:
  %(prog)s data.csv --output report.json
  %(prog)s data.csv --format html --min-score 80
        """,
    )
    parser.add_argument("input",  help="Input CSV file path")
    parser.add_argument("-o", "--output", default="output.json",
                        help="Output file (default: %(default)s)")
    parser.add_argument("-f", "--format",
                        choices=["json", "csv", "html"],
                        default="json")
    parser.add_argument("--min-score", type=float, default=0,
                        help="Minimum score threshold")
    parser.add_argument("--verbose", "-v", action="store_true")
    parser.add_argument("--workers", type=int, default=4,
                        metavar="N", help="Number of worker threads")
    return parser


def main(args=None):
    parser = create_parser()
    ns = parser.parse_args(args)

    if ns.verbose:
        logging.basicConfig(level=logging.DEBUG)

    process(ns.input, ns.output, ns.format, ns.min_score, ns.workers)


if __name__ == "__main__":
    main()
```

### Click — more ergonomic CLIs

```python
# pip install click
import click

@click.group()
@click.option("--verbose", "-v", is_flag=True)
@click.pass_context
def cli(ctx, verbose):
    ctx.ensure_object(dict)
    ctx.obj["verbose"] = verbose


@cli.command()
@click.argument("input_file", type=click.Path(exists=True))
@click.option("--output", "-o", default="output.json", show_default=True)
@click.option("--format", "fmt",
              type=click.Choice(["json", "csv", "html"]),
              default="json")
@click.option("--min-score", type=float, default=0.0)
@click.pass_context
def process(ctx, input_file, output, fmt, min_score):
    """Process an input file and write results."""
    verbose = ctx.obj["verbose"]
    if verbose:
        click.echo(f"Processing {input_file}")
    # ... your logic here
    click.echo(click.style(f"Done! Output: {output}", fg="green"))


@cli.command()
@click.argument("directory", type=click.Path(file_okay=False))
@click.option("--recursive", "-r", is_flag=True)
def scan(directory, recursive):
    """Scan a directory for processable files."""
    glob_fn = Path(directory).rglob if recursive else Path(directory).glob
    files = list(glob_fn("*.csv"))
    click.echo(f"Found {len(files)} files")
    for f in files:
        click.echo(f"  {f}")


if __name__ == "__main__":
    cli()
```

---

## 16. Data Science: pandas, NumPy & Matplotlib

### pandas — data manipulation

```python
# pip install pandas
import pandas as pd

# Load data
df = pd.read_csv("sales.csv", parse_dates=["date"])
df = pd.read_excel("data.xlsx", sheet_name="Sheet1")
df = pd.read_json("data.json")

# Quick inspection
df.head()
df.info()
df.describe()
df.dtypes
df.shape          # (rows, cols)

# Selection
df["name"]                             # single column → Series
df[["name", "salary"]]                 # multiple columns → DataFrame
df[df["salary"] > 50_000]             # filter rows
df.loc[0:5, "name":"salary"]          # label-based slice
df.iloc[0:5, 0:3]                     # position-based slice
df.query("salary > 50000 and active == True")  # SQL-like

# Cleaning
df = df.dropna(subset=["email"])              # drop rows with null email
df["salary"] = df["salary"].fillna(df["salary"].median())
df["name"]   = df["name"].str.strip().str.title()
df = df.drop_duplicates(subset=["email"])
df["joined"] = pd.to_datetime(df["joined"], errors="coerce")

# Transformation
df["full_name"] = df["first"] + " " + df["last"]
df["bonus"]     = df["salary"] * 0.10
df["grade"]     = pd.cut(df["score"],
                         bins=[0, 60, 75, 90, 100],
                         labels=["F", "C", "B", "A"])

# GroupBy
summary = (
    df.groupby("department")
      .agg(
          headcount=("id", "count"),
          avg_salary=("salary", "mean"),
          total_bonus=("bonus", "sum"),
      )
      .round(2)
      .sort_values("avg_salary", ascending=False)
)

# Pivot table
pivot = df.pivot_table(
    values="sales", index="region", columns="quarter", aggfunc="sum", fill_value=0
)

# Merge / join
result = pd.merge(employees, departments, on="dept_id", how="left")

# Export
df.to_csv("output.csv", index=False)
df.to_excel("output.xlsx", index=False, sheet_name="Results")
df.to_json("output.json", orient="records", indent=2)
```

### NumPy — array operations

```python
import numpy as np

# Create
a = np.array([1, 2, 3, 4, 5], dtype=np.float64)
m = np.zeros((3, 3))
i = np.eye(4)
r = np.random.default_rng(42).normal(loc=0, scale=1, size=(100, 10))

# Vectorised operations (no loops needed)
b = a * 2 + 1
c = np.sqrt(a)
d = np.sin(a)

# Boolean indexing
mask = a > 3
filtered = a[mask]   # [4. 5.]

# Aggregations
a.mean(), a.std(), a.min(), a.max(), a.sum()
np.percentile(a, [25, 50, 75])

# Matrix operations
A = np.random.rand(3, 3)
B = np.random.rand(3, 3)
C = A @ B           # matrix multiplication
np.linalg.det(A)    # determinant
np.linalg.inv(A)    # inverse
```

### Matplotlib — plotting

```python
import matplotlib.pyplot as plt
import matplotlib.ticker as ticker
import numpy as np

fig, axes = plt.subplots(2, 2, figsize=(12, 8))
fig.suptitle("Sales Dashboard", fontsize=14, fontweight="bold")

# Line chart
ax = axes[0, 0]
months = range(1, 13)
revenue = np.random.randint(80, 150, 12)
ax.plot(months, revenue, marker="o", color="#2196F3", linewidth=2)
ax.set_title("Monthly Revenue")
ax.set_xlabel("Month")
ax.set_ylabel("$K")
ax.yaxis.set_major_formatter(ticker.FuncFormatter(lambda x, _: f"${x:.0f}K"))
ax.grid(True, alpha=0.3)

# Bar chart
ax = axes[0, 1]
departments = ["Eng", "Sales", "Marketing", "Support"]
headcount   = [45, 30, 20, 25]
bars = ax.bar(departments, headcount, color=["#4CAF50", "#2196F3", "#FF9800", "#9C27B0"])
ax.set_title("Headcount by Department")
for bar, val in zip(bars, headcount):
    ax.text(bar.get_x() + bar.get_width() / 2, bar.get_height() + 0.5,
            str(val), ha="center", fontweight="bold")

# Scatter plot
ax = axes[1, 0]
x = np.random.randn(200)
y = x * 1.5 + np.random.randn(200) * 0.5
ax.scatter(x, y, alpha=0.5, c="#E91E63", s=20)
ax.set_title("Score vs. Performance")
m, b = np.polyfit(x, y, 1)
ax.plot(x, m * x + b, color="black", linewidth=1, linestyle="--", label="Trend")
ax.legend()

# Histogram
ax = axes[1, 1]
data = np.random.normal(75, 10, 500)
ax.hist(data, bins=30, color="#FF5722", edgecolor="white", alpha=0.8)
ax.axvline(data.mean(), color="black", linestyle="--", label=f"Mean: {data.mean():.1f}")
ax.set_title("Score Distribution")
ax.legend()

plt.tight_layout()
plt.savefig("dashboard.png", dpi=150, bbox_inches="tight")
plt.show()
```

---

## 17. Automation & System Tasks

### subprocess — run external commands

```python
import subprocess
import shutil

# Simple command — capture output
result = subprocess.run(
    ["git", "log", "--oneline", "-10"],
    capture_output=True,
    text=True,
    check=True,          # raises CalledProcessError on non-zero exit
    timeout=30,
)
print(result.stdout)

# Shell command (use carefully — avoid with user input)
result = subprocess.run(
    "find . -name '*.py' | wc -l",
    shell=True, capture_output=True, text=True
)

# Streaming output — print as it arrives
with subprocess.Popen(
    ["python", "long_script.py"],
    stdout=subprocess.PIPE, text=True
) as proc:
    for line in proc.stdout:
        print(line, end="")

# Check if a program is installed
def command_exists(cmd: str) -> bool:
    return shutil.which(cmd) is not None

if not command_exists("rg"):
    print("ripgrep not found — install with: brew install ripgrep")
```

### Email sending

```python
import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart
from email.mime.base import MIMEBase
from email import encoders
from pathlib import Path

def send_email(
    to: str | list[str],
    subject: str,
    body_html: str,
    attachments: list[Path] | None = None,
    smtp_host: str = "smtp.gmail.com",
    smtp_port: int = 587,
    username: str = "",
    password: str = "",
):
    if isinstance(to, str):
        to = [to]

    msg = MIMEMultipart("alternative")
    msg["Subject"] = subject
    msg["From"]    = username
    msg["To"]      = ", ".join(to)
    msg.attach(MIMEText(body_html, "html", "utf-8"))

    for path in (attachments or []):
        part = MIMEBase("application", "octet-stream")
        part.set_payload(path.read_bytes())
        encoders.encode_base64(part)
        part.add_header("Content-Disposition", f'attachment; filename="{path.name}"')
        msg.attach(part)

    with smtplib.SMTP(smtp_host, smtp_port) as server:
        server.ehlo()
        server.starttls()
        server.login(username, password)
        server.sendmail(username, to, msg.as_string())
```

### Batch file processing

```python
from pathlib import Path
from concurrent.futures import ThreadPoolExecutor, as_completed
import shutil

def process_file(src: Path, dst_dir: Path) -> tuple[Path, bool]:
    try:
        content = src.read_text(encoding="utf-8")
        processed = content.upper()   # <-- your transformation here
        dst = dst_dir / src.name
        dst.write_text(processed, encoding="utf-8")
        return src, True
    except Exception as e:
        print(f"Error processing {src}: {e}")
        return src, False


def batch_process(src_dir: Path, dst_dir: Path, pattern: str = "*.txt", workers: int = 8):
    dst_dir.mkdir(parents=True, exist_ok=True)
    files = list(src_dir.rglob(pattern))
    print(f"Processing {len(files)} files with {workers} workers...")

    ok = fail = 0
    with ThreadPoolExecutor(max_workers=workers) as executor:
        futures = {executor.submit(process_file, f, dst_dir): f for f in files}
        for future in as_completed(futures):
            _, success = future.result()
            if success: ok += 1
            else:       fail += 1

    print(f"Done: {ok} succeeded, {fail} failed")
```

---

## 18. Packaging & Distribution

### pyproject.toml — complete example

```toml
[build-system]
requires = ["hatchling>=1.18"]
build-backend = "hatchling.build"

[project]
name = "my-tool"
version = "1.2.3"
description = "A useful command-line tool"
readme = "README.md"
license = {text = "MIT"}
requires-python = ">=3.10"
authors = [{name = "Alice", email = "alice@example.com"}]
keywords = ["tool", "productivity"]
classifiers = [
    "Programming Language :: Python :: 3",
    "License :: OSI Approved :: MIT License",
    "Operating System :: OS Independent",
]
dependencies = [
    "click>=8.0",
    "requests>=2.31",
    "pydantic>=2.0",
]

[project.optional-dependencies]
dev = ["pytest>=7", "ruff", "mypy", "pytest-cov"]

[project.scripts]
my-tool = "my_tool.cli:main"   # installs `my-tool` command

[project.urls]
Homepage = "https://github.com/alice/my-tool"
Issues   = "https://github.com/alice/my-tool/issues"

[tool.hatch.build.targets.wheel]
packages = ["src/my_tool"]
```

### Building and publishing

```bash
# Build
pip install build
python -m build       # creates dist/*.whl and dist/*.tar.gz

# Upload to PyPI
pip install twine
twine upload dist/*

# Or use uv (modern, fast)
uv build
uv publish
```

---

## 19. Performance & Profiling

### Measure before optimising

```python
import cProfile
import pstats
import io
import timeit
from functools import lru_cache

# timeit — micro-benchmarks
t = timeit.timeit(
    stmt="[x**2 for x in range(1000)]",
    number=10_000
)
print(f"{t:.4f}s for 10,000 runs")

# Compare two approaches
import timeit
results = timeit.repeat(
    "[x**2 for x in range(1000)]",
    repeat=5, number=10_000
)
print(f"Best: {min(results):.4f}s  Mean: {sum(results)/len(results):.4f}s")

# cProfile — full program profiling
def profile(func, *args, **kwargs):
    pr = cProfile.Profile()
    pr.enable()
    result = func(*args, **kwargs)
    pr.disable()
    stream = io.StringIO()
    ps = pstats.Stats(pr, stream=stream).sort_stats("cumulative")
    ps.print_stats(20)
    print(stream.getvalue())
    return result

# line_profiler for line-by-line (pip install line_profiler)
# Add @profile decorator to your function, then run:
# kernprof -l -v script.py
```

### Common speedups

```python
# 1. Use sets for membership testing
names_list = ["Alice", "Bob", "Carol"] * 10_000
names_set  = set(names_list)

# O(n) vs O(1)
"Alice" in names_list   # slow
"Alice" in names_set    # fast

# 2. Avoid repeated attribute lookups in hot loops
import math
sqrt = math.sqrt       # cache the reference
results = [sqrt(x) for x in range(1_000_000)]

# 3. Use local variables in tight loops (slightly faster)
def process(data):
    _append = results.append   # local reference
    results = []
    for item in data:
        _append(item * 2)
    return results

# 4. String concatenation — use join, not +=
parts = ["a", "b", "c", "d"]
bad  = ""
for p in parts:
    bad += p         # O(n²) — creates new string each time
good = "".join(parts)  # O(n)

# 5. slots for memory-efficient objects
@dataclass
class Point:
    __slots__ = ("x", "y")
    x: float
    y: float
# Uses ~50% less memory than a regular class per instance

# 6. numpy for numerical loops
# Bad:
total = sum(x**2 for x in data)   # pure Python loop
# Good:
import numpy as np
arr = np.array(data)
total = (arr ** 2).sum()           # vectorised C code
```

### Memory profiling

```python
# pip install memory-profiler
from memory_profiler import memory_usage
import os

# Check peak memory of a function
mem = memory_usage((my_function, (arg1, arg2)), interval=0.1)
print(f"Peak memory: {max(mem):.1f} MiB")

# Quick process memory
import resource
def get_memory_mb() -> float:
    return resource.getrusage(resource.RUSAGE_SELF).ru_maxrss / 1024

# tracemalloc — find where memory is allocated
import tracemalloc
tracemalloc.start()
do_work()
snapshot = tracemalloc.take_snapshot()
top_stats = snapshot.statistics("lineno")
for stat in top_stats[:10]:
    print(stat)
```

---

## 20. Productivity Patterns & Recipes

### Singleton — one instance per process

```python
class Config:
    _instance: "Config | None" = None

    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
            cls._instance._loaded = False
        return cls._instance

    def load(self, path: str):
        if not self._loaded:
            import tomllib
            with open(path, "rb") as f:
                self._data = tomllib.load(f)
            self._loaded = True

    def __getitem__(self, key):
        return self._data[key]


cfg = Config()
cfg.load("config.toml")
# Config() anywhere else returns the same instance
```

### Observer / event system

```python
from collections import defaultdict
from typing import Callable

class EventBus:
    def __init__(self):
        self._listeners: dict[str, list[Callable]] = defaultdict(list)

    def on(self, event: str, callback: Callable):
        self._listeners[event].append(callback)
        return callback   # allow use as decorator

    def emit(self, event: str, *args, **kwargs):
        for cb in self._listeners[event]:
            cb(*args, **kwargs)

    def off(self, event: str, callback: Callable):
        self._listeners[event].remove(callback)


bus = EventBus()

@bus.on("user.created")
def send_welcome(user: dict):
    print(f"Welcome email to {user['email']}")

@bus.on("user.created")
def log_creation(user: dict):
    print(f"Audit log: user {user['id']} created")

bus.emit("user.created", {"id": 42, "email": "alice@example.com"})
```

### Pipeline builder

```python
from typing import TypeVar, Callable

T = TypeVar("T")

class Pipeline:
    def __init__(self, value):
        self._value = value

    def pipe(self, fn: Callable, *args, **kwargs) -> "Pipeline":
        self._value = fn(self._value, *args, **kwargs)
        return self

    def result(self):
        return self._value


result = (
    Pipeline("  Hello, World!  ")
    .pipe(str.strip)
    .pipe(str.lower)
    .pipe(str.replace, "world", "python")
    .pipe(str.title)
    .result()
)
# "Hello, Python!"
```

### Retry with exponential backoff

```python
import time
import random
import logging
from functools import wraps

def exponential_backoff(
    max_retries: int = 5,
    base_delay: float = 1.0,
    max_delay: float = 60.0,
    jitter: bool = True,
    exceptions: tuple = (Exception,),
):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            for attempt in range(max_retries):
                try:
                    return func(*args, **kwargs)
                except exceptions as e:
                    if attempt == max_retries - 1:
                        raise
                    delay = min(base_delay * (2 ** attempt), max_delay)
                    if jitter:
                        delay *= (0.5 + random.random() * 0.5)
                    logging.warning(
                        f"{func.__name__} failed (attempt {attempt+1}/{max_retries}), "
                        f"retrying in {delay:.2f}s: {e}"
                    )
                    time.sleep(delay)
        return wrapper
    return decorator


@exponential_backoff(max_retries=5, base_delay=1.0, exceptions=(ConnectionError, TimeoutError))
def call_external_api(url: str) -> dict:
    ...
```

### Useful one-liners & recipes

```python
from itertools import islice, chain, groupby, zip_longest, batched
from functools import reduce, partial
import operator

# Chunk a list into batches (Python 3.12+)
def chunk(lst, n):
    it = iter(lst)
    while batch := list(islice(it, n)):
        yield batch

# Or with itertools.batched (3.12+)
for batch in batched(range(20), 5):
    print(list(batch))

# Flatten one level
nested = [[1, 2], [3, 4], [5]]
flat = list(chain.from_iterable(nested))  # [1, 2, 3, 4, 5]

# Transpose a matrix
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
transposed = list(zip(*matrix))   # [(1, 4, 7), (2, 5, 8), (3, 6, 9)]

# Groupby (requires sorted input)
data = sorted([
    {"dept": "Eng", "name": "Alice"},
    {"dept": "HR",  "name": "Bob"},
    {"dept": "Eng", "name": "Carol"},
], key=lambda x: x["dept"])

for dept, members in groupby(data, key=lambda x: x["dept"]):
    print(dept, [m["name"] for m in members])

# Product of a list
reduce(operator.mul, [1, 2, 3, 4, 5])   # 120

# Deep-merge two dicts
def deep_merge(base: dict, override: dict) -> dict:
    result = base.copy()
    for key, val in override.items():
        if key in result and isinstance(result[key], dict) and isinstance(val, dict):
            result[key] = deep_merge(result[key], val)
        else:
            result[key] = val
    return result

# Safely get nested dict value
def deep_get(d: dict, *keys, default=None):
    for key in keys:
        if not isinstance(d, dict):
            return default
        d = d.get(key, default)
    return d

user = {"profile": {"address": {"city": "Delhi"}}}
city = deep_get(user, "profile", "address", "city")   # "Delhi"
missing = deep_get(user, "profile", "phone", default="N/A")  # "N/A"

# Memoisation without lru_cache (when args aren't hashable)
def memoize(func):
    cache = {}
    @wraps(func)
    def wrapper(*args):
        if args not in cache:
            cache[args] = func(*args)
        return cache[args]
    return wrapper

# Time a block of code inline
import time
start = time.perf_counter()
# ... your code ...
elapsed = time.perf_counter() - start
print(f"Elapsed: {elapsed * 1000:.2f}ms")
```

---

## Appendix: Cheat Sheet

### Built-in functions worth knowing

```python
# Iteration
enumerate(iterable, start=0)     # (index, value) pairs
zip(a, b, c)                     # tuple of items from each
zip_longest(a, b, fillvalue=None)# zip, but pads shorter iterables
map(fn, iterable)                # lazy transform
filter(fn, iterable)             # lazy filter
reversed(seq)                    # reverse iterator
sorted(iterable, key=fn)         # new sorted list
any(iterable)                    # True if at least one truthy
all(iterable)                    # True if all truthy

# Numbers
abs(x), round(x, n), divmod(a, b), pow(base, exp, mod)

# Type conversion
int("42"), float("3.14"), str(42), bool(0), list("abc"), tuple([1,2,3])

# Introspection
type(x), isinstance(x, T), issubclass(A, B)
dir(x), vars(x), hasattr(x, "attr"), getattr(x, "attr", default)
callable(x), id(x), hash(x)

# I/O
print(*args, sep=" ", end="\n", file=sys.stdout, flush=False)
input(prompt="")
open(file, mode="r", encoding=None, newline=None)
```

### Common standard library modules

| Module | Purpose |
|---|---|
| `pathlib` | File system paths |
| `os`, `os.path` | OS interface, path utilities |
| `sys` | Interpreter state, `argv`, `exit` |
| `re` | Regular expressions |
| `json` | JSON encode/decode |
| `csv` | CSV read/write |
| `tomllib` | TOML read (3.11+) |
| `datetime`, `zoneinfo` | Dates, times, timezones |
| `collections` | `deque`, `defaultdict`, `Counter`, `namedtuple` |
| `itertools` | `chain`, `islice`, `groupby`, `product`, `combinations` |
| `functools` | `lru_cache`, `partial`, `reduce`, `wraps` |
| `contextlib` | `contextmanager`, `suppress`, `ExitStack` |
| `threading` | Threads, Locks, Events |
| `multiprocessing` | Process-based parallelism |
| `asyncio` | Async/await event loop |
| `concurrent.futures` | `ThreadPoolExecutor`, `ProcessPoolExecutor` |
| `subprocess` | Run external commands |
| `shutil` | High-level file ops (copy, move, rmtree) |
| `tempfile` | Temporary files and directories |
| `hashlib` | MD5, SHA-256, etc. |
| `secrets` | Cryptographically secure random |
| `logging` | Structured logging |
| `unittest` | Test framework (use pytest instead) |
| `argparse` | CLI argument parsing |
| `sqlite3` | Built-in SQLite database |
| `http.server` | Quick static file server |
| `urllib.parse` | URL parsing and encoding |
| `dataclasses` | `@dataclass`, `field`, `asdict` |
| `typing` | Type hints |
| `abc` | Abstract base classes |
| `enum` | Enumerations |
| `decimal` | Precise decimal arithmetic |
| `statistics` | `mean`, `median`, `stdev` |
| `textwrap` | Word wrapping, `dedent` |
| `pprint` | Pretty-print data structures |

---

*Sources: [Python Docs](https://docs.python.org/3/) · [Real Python](https://realpython.com) · [Python Cookbook](https://www.oreilly.com/library/view/python-cookbook-3rd/9781449357337/) · community best practices*
