# Python In-Depth Notes

---

## Knowledge Graph (Topic Map)

```
Python
├── Core Language
│   ├── Data Types & Variables
│   ├── Operators
│   ├── Control Flow (if/for/while)
│   ├── Functions
│   │   ├── args / kwargs
│   │   ├── Closures
│   │   ├── Decorators
│   │   └── Generators
│   └── Comprehensions
│
├── Object-Oriented Programming (OOP)
│   ├── Classes & Objects
│   ├── Inheritance (Single/Multiple/MRO)
│   ├── Encapsulation & Abstraction
│   ├── Polymorphism
│   ├── Dunder / Magic Methods
│   └── Descriptors & Properties
│
├── Data Structures
│   ├── list / tuple / set / dict
│   ├── collections module
│   └── heapq / bisect
│
├── Functional Programming
│   ├── map / filter / reduce
│   ├── lambda
│   └── itertools / functools
│
├── Memory & Internals
│   ├── Mutable vs Immutable
│   ├── Reference Counting & GC
│   ├── Interning
│   ├── GIL (Global Interpreter Lock)
│   └── Memory Model
│
├── Concurrency & Parallelism
│   ├── Threading
│   ├── Multiprocessing
│   └── asyncio / async-await
│
├── Error Handling
│   ├── try / except / finally / else
│   ├── Custom Exceptions
│   └── Context Managers (with)
│
├── Modules & Packages
│   ├── import system
│   ├── __init__.py
│   └── __all__
│
├── File I/O
│   ├── open() / read / write
│   ├── pathlib
│   └── json / csv / pickle
│
├── Standard Library Highlights
│   ├── os / sys / subprocess
│   ├── datetime / time
│   ├── re (Regex)
│   ├── typing
│   └── dataclasses
│
├── Advanced Topics
│   ├── Metaclasses
│   ├── Abstract Base Classes (ABC)
│   ├── __slots__
│   ├── Descriptors
│   ├── Type Hints & Protocols
│   └── Performance & Profiling
│
├── Strings & Formatting
│   ├── String Methods
│   ├── f-strings / format()
│   ├── encode / decode (bytes ↔ str)
│   └── Template strings
│
├── Standard Library (Extended)
│   ├── os / sys / subprocess
│   ├── datetime / time / timedelta
│   ├── pathlib
│   ├── logging
│   ├── enum
│   ├── re (Regex)
│   ├── typing / TypedDict
│   └── dataclasses / namedtuple
│
├── Testing
│   ├── unittest (assert*, setUp, tearDown)
│   ├── pytest (fixtures, parametrize, markers)
│   └── Mock & Patch (unittest.mock)
│
├── Design Patterns (Pythonic)
│   ├── Singleton
│   ├── Factory
│   ├── Observer
│   ├── Strategy
│   └── Decorator pattern
│
├── Python Execution Model
│   ├── Source → Bytecode (.pyc)
│   ├── dis module (disassembler)
│   ├── CPython vs PyPy vs Jython
│   └── REPL & __name__
│
└── Packaging & Environment
    ├── venv / virtualenv
    ├── pip & requirements.txt
    ├── pyproject.toml / setup.py
    └── poetry / pipenv
```

---

## 1. Data Types & Variables

> **Definition:** In Python, every value is an **object** with a type, an identity (`id()`), and a value. Variables are **references** (name labels) pointing to objects in memory — not containers holding the value itself. Python is **dynamically typed**: the type is attached to the object, not the variable name.

### Primitive Types
| Type    | Example            | Mutable |
|---------|--------------------|---------|
| int     | `x = 42`           | No      |
| float   | `x = 3.14`         | No      |
| bool    | `x = True`         | No      |
| str     | `x = "hello"`      | No      |
| bytes   | `x = b"data"`      | No      |
| NoneType| `x = None`         | No      |

### Container Types
| Type  | Syntax           | Mutable | Ordered | Duplicates |
|-------|------------------|---------|---------|------------|
| list  | `[1, 2, 3]`      | Yes     | Yes     | Yes        |
| tuple | `(1, 2, 3)`      | No      | Yes     | Yes        |
| set   | `{1, 2, 3}`      | Yes     | No      | No         |
| dict  | `{"a": 1}`       | Yes     | Yes*    | Keys: No   |

*dicts maintain insertion order since Python 3.7

### Type Checking
```python
type(x)          # exact type
isinstance(x, int)  # also True for subclasses
```

### Variable Internals
```python
# Variables are references (labels), not boxes
a = [1, 2, 3]
b = a           # b points to the SAME list
b.append(4)
print(a)        # [1, 2, 3, 4]  <-- a is also changed

# To copy:
b = a.copy()        # shallow copy
import copy
b = copy.deepcopy(a)  # deep copy
```

---

## 2. Operators

> **Definition:** Operators are special symbols or keywords that perform operations on one or more **operands**. Python operators follow precedence rules (PEMDAS-like order) and can be overloaded via dunder methods (e.g. `__add__`, `__eq__`).

```python
# Arithmetic
+  -  *  /   //  %  **
# / → float div,  // → floor div,  ** → power

# Comparison
==  !=  >  <  >=  <=

# Identity vs Equality
x is y       # same object (memory address)
x == y       # same value
# Use `is` only for None, True, False

# Membership
x in collection
x not in collection

# Logical
and  or  not

# Bitwise
&  |  ^  ~  <<  >>

# Walrus (Python 3.8+)
if n := len(data):
    print(f"Length is {n}")
```

---

## 3. Control Flow

> **Definition:** Control flow determines the **order in which statements execute**. Python uses indentation (not braces) to define blocks. The three main structures are **conditional branching** (`if/elif/else`), **definite iteration** (`for`), and **indefinite iteration** (`while`). Python's `for-else`/`while-else` clause is unique — the `else` runs only when the loop exits normally (no `break`).

```python
# if-elif-else
if x > 0:
    ...
elif x == 0:
    ...
else:
    ...

# Ternary
result = "yes" if condition else "no"

# for loop
for i in range(10):        # 0..9
for i, v in enumerate(lst):
for k, v in dct.items():

# while
while condition:
    if exit_early: break
    if skip_iter: continue

# for-else / while-else
# else block runs when loop completes WITHOUT break
for item in items:
    if found(item):
        break
else:
    print("not found")
```

---

## 4. Functions (Core Interview Topic)

> **Definition:** A function is a **named, reusable block of code** that takes inputs (parameters) and optionally returns a value. In Python, functions are **first-class objects** — they can be assigned to variables, passed as arguments, returned from other functions, and stored in data structures. Python supports default args, keyword args, positional-only (`/`) and keyword-only (`*`) parameter constraints.

```python
def greet(name, greeting="Hello"):  # default arg
    return f"{greeting}, {name}"

# *args → tuple of positional extras
# **kwargs → dict of keyword extras
def func(*args, **kwargs):
    print(args, kwargs)

# Keyword-only arguments (after *)
def func(a, *, b, c):  # b and c must be passed as keywords
    ...

# Positional-only (Python 3.8+, before /)
def func(a, b, /, c):  # a, b must be positional
    ...

# Unpacking on call
args = (1, 2); kwargs = {"c": 3}
func(*args, **kwargs)
```

### Scope: LEGB Rule
```
L → Local
E → Enclosing (closure)
G → Global
B → Built-in
```
```python
x = "global"

def outer():
    x = "enclosing"
    def inner():
        nonlocal x          # modify enclosing scope
        x = "inner"
    inner()
    print(x)   # "inner"

outer()
print(x)       # "global"
```

---

## 5. Closures & Decorators

### Closure
A function that **remembers** the variables from its enclosing scope even after the outer function has returned.

```python
def make_counter():
    count = 0
    def counter():
        nonlocal count
        count += 1
        return count
    return counter

c = make_counter()
c()  # 1
c()  # 2
```

### Decorators
A decorator is a **higher-order function** that wraps another function to extend behavior.

```python
import functools

def logger(func):
    @functools.wraps(func)   # preserves __name__, __doc__
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__}")
        result = func(*args, **kwargs)
        print(f"Done")
        return result
    return wrapper

@logger
def add(a, b):
    return a + b

# @logger is syntactic sugar for: add = logger(add)
```

### Decorator with Arguments
```python
def repeat(n):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            for _ in range(n):
                func(*args, **kwargs)
        return wrapper
    return decorator

@repeat(3)
def say_hello():
    print("Hello")
```

---

## 6. Generators & Iterators (Top Interview Topic)

> **Definition:** An **iterator** is any object that implements the iterator protocol: `__iter__()` returns `self` and `__next__()` returns the next value or raises `StopIteration`. A **generator** is a special function that uses `yield` to produce values one at a time, pausing execution between each. Generators implement the iterator protocol automatically. They are the foundation of **lazy evaluation** — values are computed on demand rather than all at once, making them memory-efficient for large or infinite sequences.

### Iterator Protocol
Any object with `__iter__()` and `__next__()` methods.

```python
class CountUp:
    def __init__(self, limit):
        self.limit = limit
        self.current = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self.current >= self.limit:
            raise StopIteration
        self.current += 1
        return self.current
```

### Generator Function
Uses `yield` — pauses execution, resumes on `next()`. **Lazy evaluation** = memory efficient.

```python
def fibonacci():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b

gen = fibonacci()
[next(gen) for _ in range(8)]  # [0, 1, 1, 2, 3, 5, 8, 13]
```

### Generator Expression
```python
squares = (x**2 for x in range(10))   # lazy
# vs list comprehension: [x**2 for x in range(10)]  # eager
```

### `yield from`
```python
def chain(*iterables):
    for it in iterables:
        yield from it
```

---

## 7. Comprehensions

> **Definition:** Comprehensions are **concise, readable syntax** for building collections from iterables in a single expression. They run at C speed inside the interpreter (faster than equivalent `for` loops). Python supports list, dict, set, and generator comprehensions. Prefer them over `map`/`filter` when the logic fits in one readable expression.

```python
# List comprehension
[x**2 for x in range(10) if x % 2 == 0]

# Dict comprehension
{k: v for k, v in pairs}
{k: v for k, v in d.items() if v > 0}

# Set comprehension
{x % 3 for x in range(9)}

# Nested comprehension
matrix = [[1,2,3],[4,5,6],[7,8,9]]
flat = [n for row in matrix for n in row]
```

---

## 8. Object-Oriented Programming

> **Definition:** OOP is a paradigm that models real-world entities as **objects** — bundles of data (attributes) and behavior (methods). Python is a fully OOP language where everything, including functions, classes, and modules, is an object. The four pillars are **Encapsulation** (hiding internal state), **Inheritance** (reusing and extending behavior), **Polymorphism** (same interface, different implementations), and **Abstraction** (exposing only what is necessary).

### Class Basics
```python
class Animal:
    species = "Unknown"       # class attribute

    def __init__(self, name): # instance method
        self.name = name      # instance attribute

    def speak(self):
        return f"{self.name} speaks"

    @classmethod
    def create(cls, name):    # cls → the class itself
        return cls(name)

    @staticmethod
    def describe():           # no self or cls
        return "I am an animal"
```

### Inheritance & MRO
```python
class Dog(Animal):
    def speak(self):
        return f"{self.name} barks"   # overrides parent

class Cat(Animal):
    def speak(self):
        return f"{self.name} meows"

# Multiple Inheritance
class Hybrid(Dog, Cat):
    pass

# MRO — Method Resolution Order (C3 Linearization)
Hybrid.__mro__   # (Hybrid, Dog, Cat, Animal, object)
# Python searches left → right, depth first, no repeats
```

### Dunder / Magic Methods (Most Asked in Interviews)
```python
class Vector:
    def __init__(self, x, y):
        self.x, self.y = x, y

    def __repr__(self):          # developer string
        return f"Vector({self.x}, {self.y})"

    def __str__(self):           # user-friendly string
        return f"({self.x}, {self.y})"

    def __add__(self, other):    # v1 + v2
        return Vector(self.x + other.x, self.y + other.y)

    def __len__(self):           # len(v)
        return 2

    def __eq__(self, other):     # v1 == v2
        return self.x == other.x and self.y == other.y

    def __hash__(self):          # needed if __eq__ defined
        return hash((self.x, self.y))

    def __getitem__(self, i):    # v[0]
        return (self.x, self.y)[i]

    def __iter__(self):          # for val in v
        yield self.x
        yield self.y

    def __contains__(self, val): # val in v
        return val in (self.x, self.y)

    def __bool__(self):          # bool(v)
        return bool(self.x or self.y)

    def __call__(self):          # v()
        return self.x ** 2 + self.y ** 2
```

### Properties & Descriptors
```python
class Circle:
    def __init__(self, radius):
        self._radius = radius

    @property
    def radius(self):
        return self._radius

    @radius.setter
    def radius(self, value):
        if value < 0:
            raise ValueError("Radius cannot be negative")
        self._radius = value

    @property
    def area(self):
        import math
        return math.pi * self._radius ** 2
```

### `__slots__`
Avoids per-instance `__dict__`, saves memory.
```python
class Point:
    __slots__ = ["x", "y"]   # only these attributes allowed
    def __init__(self, x, y):
        self.x, self.y = x, y
```

### Abstract Base Classes
```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self): ...

    @abstractmethod
    def perimeter(self): ...

class Rectangle(Shape):
    def __init__(self, w, h):
        self.w, self.h = w, h
    def area(self): return self.w * self.h
    def perimeter(self): return 2 * (self.w + self.h)
```

---

## 9. Memory & Python Internals (Top Interview Topic)

> **Definition:** Understanding Python's memory model is essential for writing efficient, correct code. CPython (the reference implementation) manages memory via **reference counting** — each object tracks how many names point to it and is freed when the count reaches zero. A separate **cyclic garbage collector** handles circular references. Objects are stored in a private heap; CPython's `pymalloc` uses memory pools for small allocations to avoid OS overhead.

### Mutable vs Immutable
| Immutable        | Mutable          |
|------------------|------------------|
| int, float, bool | list, dict, set  |
| str, bytes       | bytearray        |
| tuple, frozenset | custom objects   |

```python
# Immutable objects can be cached/interned
a = "hello"
b = "hello"
a is b  # True  (interned)

a = [1,2]
b = [1,2]
a is b  # False (different objects)
```

### id() and Memory
```python
x = 300
y = 300
x is y   # False (not interned — large ints)

x = 5
y = 5
x is y   # True (CPython caches -5 to 256)
```

### Shallow vs Deep Copy
```python
import copy
original = [[1, 2], [3, 4]]

shallow = copy.copy(original)      # new list, same inner lists
deep    = copy.deepcopy(original)  # fully independent

shallow[0].append(99)
# original[0] → [1, 2, 99]  (affected!)
# deep[0]     → [1, 2]      (not affected)
```

### Reference Counting + GC
```python
import sys
x = [1, 2, 3]
sys.getrefcount(x)    # shows reference count (always +1 for the call itself)

# Circular references are handled by the cyclic garbage collector (gc module)
import gc
gc.collect()
```

### Global Interpreter Lock (GIL)
- Only **one thread** executes Python bytecode at a time.
- Prevents true CPU parallelism in threads.
- I/O-bound → threads are fine (GIL released during I/O).
- CPU-bound → use `multiprocessing` or `concurrent.futures.ProcessPoolExecutor`.

---

## 10. Error Handling

> **Definition:** Python uses **exceptions** — objects that represent errors or unexpected events — to interrupt normal flow and transfer control to a handler. Unlike return-code-based error handling, exceptions carry a full traceback and type hierarchy. The `try/except/else/finally` construct gives fine-grained control: `except` catches errors, `else` runs when no error occurred, and `finally` always runs for cleanup. Context managers (`with`) automate resource cleanup via the `__enter__`/`__exit__` protocol.

```python
try:
    result = 10 / 0
except ZeroDivisionError as e:
    print(f"Error: {e}")
except (TypeError, ValueError) as e:
    print(f"Type/Value error: {e}")
except Exception as e:
    print(f"Unexpected: {e}")
    raise               # re-raise
else:
    print("No exception occurred")   # runs only if no exception
finally:
    print("Always runs")             # cleanup

# Custom Exception
class InsufficientFunds(Exception):
    def __init__(self, amount, balance):
        self.amount = amount
        self.balance = balance
        super().__init__(f"Cannot withdraw {amount}, balance is {balance}")
```

### Context Managers
```python
# Using with statement
with open("file.txt") as f:
    data = f.read()
# file is closed automatically

# Custom context manager (class)
class ManagedResource:
    def __enter__(self):
        print("Acquiring")
        return self

    def __exit__(self, exc_type, exc_val, exc_tb):
        print("Releasing")
        return False   # False → don't suppress exceptions

# Custom context manager (function)
from contextlib import contextmanager

@contextmanager
def managed():
    print("Acquiring")
    try:
        yield "resource"
    finally:
        print("Releasing")
```

---

## 11. Data Structures In-Depth

> **Definition:** Python's built-in data structures — `list`, `tuple`, `dict`, `set` — are the foundation of almost every program. Each has distinct **time complexities**, **memory characteristics**, and ideal use cases. The `collections` module extends these with specialised types (`deque`, `Counter`, `defaultdict`), and `heapq`/`bisect` provide efficient heap and binary-search operations.

### list
```python
lst = [3, 1, 4, 1, 5]
lst.append(9)         # O(1) amortized
lst.insert(0, 0)      # O(n)
lst.pop()             # O(1)  — from end
lst.pop(0)            # O(n)  — from front (use deque for this)
lst.sort()            # Timsort, O(n log n), in-place, stable
sorted(lst)           # returns new sorted list
lst.index(4)          # first occurrence, O(n)
lst.count(1)          # O(n)
```

### dict
```python
d = {"a": 1, "b": 2}
d["c"] = 3
d.get("x", 0)         # safe access with default
d.setdefault("y", []).append(1)
d.update({"d": 4})
del d["a"]
"a" in d              # O(1) average
for k, v in d.items(): ...

# dict is ordered by insertion (Python 3.7+)
# Implemented as hash table → O(1) average for get/set/del
```

### collections module
```python
from collections import (
    defaultdict,
    Counter,
    deque,
    OrderedDict,
    namedtuple,
    ChainMap
)

# defaultdict — auto-initializes missing keys
dd = defaultdict(list)
dd["key"].append(1)   # no KeyError

# Counter — frequency mapping
c = Counter("abracadabra")
c.most_common(2)      # [('a', 5), ('b', 2)]
c1 + c2               # union of counts

# deque — O(1) append/pop from both ends
dq = deque([1, 2, 3], maxlen=5)
dq.appendleft(0)
dq.popleft()

# namedtuple — lightweight immutable record
Point = namedtuple("Point", ["x", "y"])
p = Point(1, 2)
p.x, p.y    # access by name
```

### heapq (Min-Heap)
```python
import heapq
h = [5, 1, 3]
heapq.heapify(h)              # in-place, O(n)
heapq.heappush(h, 2)          # O(log n)
smallest = heapq.heappop(h)   # O(log n)
heapq.nlargest(3, data)       # top 3 largest
heapq.nsmallest(3, data)      # top 3 smallest

# Max-heap trick: negate values
heapq.heappush(h, -val)
max_val = -heapq.heappop(h)
```

---

## 12. Functional Programming

> **Definition:** Functional programming (FP) treats computation as the evaluation of **pure functions** — functions with no side effects that always return the same output for the same input. Python supports FP via `map`, `filter`, `reduce`, `lambda`, closures, and the `itertools`/`functools` modules. FP tools are best paired with generators for **lazy, memory-efficient pipelines**.

```python
from functools import reduce, partial, lru_cache

# map / filter / reduce
nums = [1, 2, 3, 4, 5]
list(map(lambda x: x**2, nums))           # [1, 4, 9, 16, 25]
list(filter(lambda x: x % 2 == 0, nums)) # [2, 4]
reduce(lambda acc, x: acc + x, nums, 0)  # 15

# partial — fix some arguments
def power(base, exp): return base ** exp
square = partial(power, exp=2)
square(5)  # 25

# lru_cache — memoize expensive calls
@lru_cache(maxsize=None)
def fib(n):
    if n < 2: return n
    return fib(n-1) + fib(n-2)

# functools.cache (Python 3.9+, unlimited)
from functools import cache
@cache
def fib(n): ...
```

### itertools
```python
import itertools as it

it.count(1)                  # 1, 2, 3, ...  (infinite)
it.cycle([1,2,3])            # 1, 2, 3, 1, 2, ...  (infinite)
it.repeat(x, n)              # x repeated n times
it.chain([1,2], [3,4])       # 1, 2, 3, 4
it.chain.from_iterable(...)  # flatten one level
it.islice(gen, 5)            # first 5 from any iterable
it.combinations([1,2,3], 2)  # (1,2), (1,3), (2,3)
it.permutations([1,2,3], 2)  # all ordered pairs
it.product([1,2], [3,4])     # cartesian product
it.groupby(data, key=fn)     # consecutive group by key
it.accumulate([1,2,3,4])     # running totals: 1,3,6,10
```

---

## 13. Concurrency & Parallelism

> **Definition:**
> - **Concurrency** — multiple tasks make progress by interleaving execution (one CPU, tasks take turns). Useful for **I/O-bound** work (network, disk).
> - **Parallelism** — multiple tasks run simultaneously on multiple CPUs. Useful for **CPU-bound** work (computation, image processing).
>
> Python provides three models: `threading` (concurrent, limited by GIL for CPU work), `multiprocessing` (true parallelism, bypasses GIL), and `asyncio` (cooperative concurrency via an event loop — single thread, no GIL issue).

### Threading (I/O-bound)
```python
import threading

def task(n):
    print(f"Task {n}")

threads = [threading.Thread(target=task, args=(i,)) for i in range(5)]
for t in threads: t.start()
for t in threads: t.join()

# Thread-safe sharing with Lock
lock = threading.Lock()
with lock:
    shared_resource += 1
```

### Multiprocessing (CPU-bound)
```python
from multiprocessing import Pool

def compute(x):
    return x ** 2

with Pool(4) as p:
    results = p.map(compute, range(10))
```

### asyncio (Concurrency, not parallelism)
```python
import asyncio

async def fetch(url):
    await asyncio.sleep(1)   # non-blocking wait
    return f"data from {url}"

async def main():
    # Run concurrently
    results = await asyncio.gather(
        fetch("url1"),
        fetch("url2"),
        fetch("url3"),
    )
    print(results)

asyncio.run(main())
```

### concurrent.futures (High-Level)
```python
from concurrent.futures import ThreadPoolExecutor, ProcessPoolExecutor

# ThreadPool for I/O-bound
with ThreadPoolExecutor(max_workers=5) as ex:
    futures = [ex.submit(task, i) for i in range(10)]
    results = [f.result() for f in futures]

# ProcessPool for CPU-bound
with ProcessPoolExecutor() as ex:
    results = list(ex.map(cpu_task, data))
```

---

## 14. Type Hints (Python 3.5+)

> **Definition:** Type hints are **optional annotations** that declare the expected types of variables, function parameters, and return values. They are not enforced at runtime — they serve as documentation and are checked by static analysis tools like `mypy` and `pyright`. Python 3.9+ allows using built-in types directly (`list[int]` instead of `List[int]`). Type hints make large codebases safer and easier to refactor.

```python
from typing import Optional, Union, List, Dict, Tuple, Any, Callable
from typing import TypeVar, Generic, Protocol

def greet(name: str) -> str:
    return f"Hello {name}"

def process(items: List[int]) -> Dict[str, int]:
    return {"sum": sum(items)}

# Optional = Union[X, None]
def find(key: str) -> Optional[int]:
    ...

# TypeVar for generics
T = TypeVar("T")
def first(items: list[T]) -> T:
    return items[0]

# Protocol (structural subtyping — duck typing)
class Drawable(Protocol):
    def draw(self) -> None: ...

# Python 3.10+ union syntax
def func(x: int | str) -> int | None: ...

# dataclass with types
from dataclasses import dataclass, field

@dataclass
class Config:
    host: str = "localhost"
    port: int = 8080
    tags: list[str] = field(default_factory=list)
```

---

## 15. Metaclasses

> **Definition:** A **metaclass** is the class of a class — it controls **how classes themselves are created**. Just as `int` creates integers and `str` creates strings, `type` (the default metaclass) creates classes. By defining a custom metaclass you can intercept and modify class creation: auto-register subclasses, enforce coding conventions, add methods, or implement the Singleton pattern. Metaclasses are powerful but rarely needed — prefer class decorators or `__init_subclass__` for simpler use cases.

A **metaclass** is the class of a class — it controls how classes are created.

```python
# type is the default metaclass of all classes
type("Dog", (Animal,), {"speak": lambda self: "Woof"})
# creates a class dynamically

class SingletonMeta(type):
    _instances = {}

    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            cls._instances[cls] = super().__call__(*args, **kwargs)
        return cls._instances[cls]

class Database(metaclass=SingletonMeta):
    def __init__(self):
        self.connection = "connected"

db1 = Database()
db2 = Database()
db1 is db2   # True
```

---

## 16. Modules, Packages & Imports

> **Definition:** A **module** is any `.py` file. A **package** is a directory containing an `__init__.py` file (Python 3.3+ also supports namespace packages without it). The `import` system resolves names by searching `sys.path` in order: built-ins → frozen modules → `sys.path` directories. Modules are executed once on first import and cached in `sys.modules`; subsequent imports reuse the cached object.

```python
# Import variants
import os
from os import path
from os.path import join, exists
import os as operating_system

# __name__ guard
if __name__ == "__main__":
    main()   # only runs when executed directly, not imported

# __all__ — controls what `from module import *` exports
__all__ = ["PublicClass", "public_func"]

# Relative imports (inside a package)
from . import sibling_module
from ..parent import something
```

---

## 17. File I/O

> **Definition:** File I/O in Python is handled through the built-in `open()` function which returns a **file object** (a stream). Python abstracts over OS-level file handles and provides text mode (decoding bytes to `str`) and binary mode. The `with` statement (context manager) ensures files are always closed even if an exception occurs. `pathlib.Path` (Python 3.4+) provides an OOP interface superior to `os.path`.

```python
# Reading
with open("file.txt", "r", encoding="utf-8") as f:
    content = f.read()         # entire file as string
    lines   = f.readlines()    # list of lines
    for line in f:             # iterate lines (memory efficient)
        ...

# Writing
with open("out.txt", "w") as f:
    f.write("hello\n")
    f.writelines(["a\n", "b\n"])

# Modes: r, w, a, rb, wb, r+, x (create, fail if exists)

# pathlib (modern, OOP)
from pathlib import Path

p = Path("data") / "file.txt"
p.exists()
p.read_text()
p.write_text("hello")
list(Path(".").glob("**/*.py"))   # recursive glob

# JSON
import json
data = json.loads('{"key": 1}')  # str → dict
text = json.dumps(data, indent=2)  # dict → str
with open("f.json") as f: data = json.load(f)

# Pickle (Python objects — not cross-language)
import pickle
pickle.dumps(obj)   # serialize to bytes
pickle.loads(b)     # deserialize
```

---

## 18. Regex (re module)

> **Definition:** Regular expressions (regex) are **patterns** for matching, searching, and transforming text. Python's `re` module implements Perl-compatible regex syntax. Key distinction: `re.match()` anchors to the start; `re.search()` scans the full string. Always use **raw strings** (`r"..."`) for patterns to avoid double-escaping backslashes. Compile patterns with `re.compile()` when reusing them in loops.

```python
import re

pattern = r"\d{3}-\d{4}"
text = "Call 123-4567 or 987-6543"

re.match(pattern, text)      # match at start only
re.search(pattern, text)     # first match anywhere
re.findall(pattern, text)    # list of all matches
re.finditer(pattern, text)   # iterator of match objects
re.sub(pattern, "XXX", text) # replace
re.split(r"\s+", text)       # split by regex

# Groups
m = re.search(r"(\d{3})-(\d{4})", text)
m.group(0)   # full match
m.group(1)   # first group
m.groups()   # all groups

# Compile for reuse
pat = re.compile(r"\d+", re.IGNORECASE)
pat.findall(text)

# Common patterns
r"\d+"     # one or more digits
r"\w+"     # word characters (letters, digits, _)
r"\s+"     # whitespace
r"^start"  # start of string
r"end$"    # end of string
r"[aeiou]" # character class
r"(cat|dog)" # alternation
```

---

## 19. Sorting, Key Functions & bisect

> **Definition:** Python's `sorted()` (returns new list) and `list.sort()` (in-place) both use **Timsort** — a stable, adaptive hybrid sort. The `key` parameter accepts a function applied to each element before comparison, enabling sorting by any derived criterion without modifying the data. `bisect` provides O(log n) binary search and insertion into **already-sorted** lists.

```python
# sorted with key
students = [("Alice", 90), ("Bob", 85), ("Charlie", 92)]
sorted(students, key=lambda s: s[1], reverse=True)

# sort by multiple criteria
data.sort(key=lambda x: (x.age, x.name))

# operator.attrgetter / itemgetter
from operator import itemgetter, attrgetter
sorted(students, key=itemgetter(1))
sorted(objects, key=attrgetter("age"))

# bisect — binary search in sorted list
import bisect
a = [1, 3, 5, 7, 9]
bisect.bisect_left(a, 5)   # index where 5 would be inserted (left)
bisect.bisect_right(a, 5)  # index to the right of 5
bisect.insort(a, 4)        # insert 4 in-place maintaining order
```

---

## 20. Top Interview Questions & Answers

### Q1: What is the difference between `==` and `is`?
- `==` compares **values** using `__eq__`.
- `is` compares **identity** (memory address, `id()`).
- Always use `is` for `None`, `True`, `False`. Never for integers/strings.

### Q2: Explain Python's GIL.
The Global Interpreter Lock is a mutex that ensures only one thread executes Python bytecode at a time. It simplifies CPython's memory management (reference counting) but prevents true CPU parallelism. Workarounds: use `multiprocessing`, `concurrent.futures.ProcessPoolExecutor`, or C extensions that release the GIL.

### Q3: Mutable default argument trap
```python
# WRONG — list is created once at function definition
def append_to(val, lst=[]):
    lst.append(val)
    return lst

append_to(1)  # [1]
append_to(2)  # [1, 2]  ← bug!

# CORRECT
def append_to(val, lst=None):
    if lst is None:
        lst = []
    lst.append(val)
    return lst
```

### Q4: How does Python manage memory?
1. **Reference counting** — objects freed when ref count hits 0.
2. **Cyclic GC** — handles circular references (objects that refer to each other).
3. **Memory pools** — CPython uses `pymalloc` for small objects (<= 512 bytes) to reduce OS overhead.

### Q5: What is a decorator and how does it work?
A decorator is a callable that takes a function and returns a new function. Implemented via closures. `@decorator` is syntactic sugar for `func = decorator(func)`.

### Q6: Generator vs List Comprehension
| | List Comprehension | Generator Expression |
|---|---|---|
| Syntax | `[x for x in ...]` | `(x for x in ...)` |
| Evaluation | Eager (all at once) | Lazy (one at a time) |
| Memory | O(n) | O(1) |
| Reusable | Yes | No (exhausted once) |

### Q7: Explain MRO and diamond inheritance
```python
class A: pass
class B(A): pass
class C(A): pass
class D(B, C): pass

D.__mro__  # (D, B, C, A, object)
# C3 linearization ensures consistent, predictable lookup order
```

### Q8: `__new__` vs `__init__`
- `__new__`: creates the instance (called first, returns the object).
- `__init__`: initializes the instance (called after, receives the object).
- Override `__new__` for Singleton, immutable types, or metaclass control.

```python
class Singleton:
    _instance = None
    def __new__(cls):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
        return cls._instance
```

### Q9: List vs Tuple — when to use which?
- **List**: mutable, homogeneous collection, same type items.
- **Tuple**: immutable, heterogeneous record (name, age, score), can be dict key, slight performance advantage.
- Rule of thumb: if data shouldn't change → tuple; if it grows/changes → list.

### Q10: How does `dict` work internally?
Python dict uses a **hash table**. Keys are hashed → bucket index. Collision handled via open addressing (probing). O(1) average for get/set/del. Dict preserves insertion order (CPython 3.6+, guaranteed 3.7+).

### Q11: What is `*args` and `**kwargs`?
- `*args` collects extra positional arguments into a **tuple**.
- `**kwargs` collects extra keyword arguments into a **dict**.
- Order in signature: `def f(a, b, *args, key=val, **kwargs)`

### Q12: Explain `@staticmethod` vs `@classmethod` vs instance method
| | Receives | Use case |
|---|---|---|
| Instance method | `self` (instance) | access/modify instance state |
| `@classmethod` | `cls` (class) | factory methods, alternative constructors |
| `@staticmethod` | nothing | utility functions logically grouped in class |

### Q13: What are Python's data model protocols?
Python uses **dunder methods** to hook into language constructs:
- `__len__` → `len(obj)`
- `__getitem__` → `obj[key]`, enables `for` loop
- `__iter__` / `__next__` → iteration
- `__enter__` / `__exit__` → `with` statement
- `__add__` etc → operator overloading
- `__call__` → `obj()` callable
- `__contains__` → `in` operator

### Q14: Shallow copy vs Deep copy
- **Shallow**: new container, same element references. `copy.copy()`, `list.copy()`, `dict.copy()`, `lst[:]`
- **Deep**: fully independent copy of all nested objects. `copy.deepcopy()`

### Q15: `@property` use case
Allows attribute-style access while running getter/setter logic — validates input, computes derived values, maintains backward compatibility when refactoring.

---

## 21. Python Performance Tips

> **Definition:** Python is slower than compiled languages because of its dynamic typing, reference counting, and interpreter overhead. However, most Python programs spend the majority of time in a small fraction of code — profiling reveals the real bottlenecks. The key principle: **measure first, optimise second**. Common gains come from using built-in operations (which run at C speed), reducing object creation, and choosing the right data structure.

```python
# 1. Use local variable instead of global in tight loops
def fast():
    local_append = [].append   # cache method lookup
    for i in range(10000):
        local_append(i)

# 2. Use join instead of + for string building
"".join(["a", "b", "c"])   # O(n), vs += which is O(n²)

# 3. Use sets for membership testing — O(1) vs O(n) for lists
items_set = set(items)
if x in items_set: ...

# 4. Use slots for memory-efficient classes
class Point:
    __slots__ = ["x", "y"]

# 5. Prefer list comprehensions over explicit for loops
# (runs at C speed inside the interpreter)

# 6. Use generators for large data pipelines
total = sum(x**2 for x in range(10**7))   # no large list

# 7. @lru_cache for expensive repeated computations
from functools import lru_cache

# 8. Profile first
import cProfile
cProfile.run("my_function()")
```

---

## 22. Dataclasses (Python 3.7+)

> **Definition:** A `@dataclass` decorator automatically generates boilerplate methods (`__init__`, `__repr__`, `__eq__`) from class-level **type-annotated fields**. It is a code-generation tool, not a runtime type enforcer. Use `frozen=True` to make instances immutable and hashable (usable as dict keys or in sets). Use `field()` for mutable defaults and post-init processing.

```python
from dataclasses import dataclass, field, asdict, astuple

@dataclass(order=True, frozen=True)  # frozen → immutable, hashable
class Point:
    x: float
    y: float
    label: str = ""
    tags: list = field(default_factory=list)

    def distance(self):
        return (self.x**2 + self.y**2) ** 0.5

p = Point(3.0, 4.0)
p.distance()     # 5.0
asdict(p)        # {"x": 3.0, "y": 4.0, "label": "", "tags": []}
astuple(p)       # (3.0, 4.0, "", [])

# Auto-generates: __init__, __repr__, __eq__
# order=True: __lt__, __le__, __gt__, __ge__
# frozen=True: __hash__, immutable
```

---

## 23. Advanced: Descriptors

> **Definition:** A **descriptor** is any object that defines `__get__`, `__set__`, or `__delete__` and is assigned as a **class attribute**. Descriptors control what happens when that attribute is accessed, set, or deleted on an instance. They are the underlying mechanism powering `property`, `classmethod`, `staticmethod`, and `__slots__`. Understanding descriptors explains how Python's attribute lookup really works.

Descriptors control attribute access. Foundation of `property`, `classmethod`, `staticmethod`.

```python
class Validator:
    def __set_name__(self, owner, name):
        self.name = name

    def __get__(self, obj, objtype=None):
        if obj is None: return self
        return obj.__dict__.get(self.name)

    def __set__(self, obj, value):
        if not isinstance(value, int):
            raise TypeError(f"{self.name} must be an int")
        obj.__dict__[self.name] = value

class MyClass:
    age = Validator()   # descriptor instance as class attribute
```

**Descriptor Protocol:**
- `__get__` — accessing `obj.attr`
- `__set__` — assigning `obj.attr = val`
- `__delete__` — `del obj.attr`
- `__set_name__` — called at class creation (Python 3.6+)

---

## Quick Reference: Complexity Cheatsheet

| Operation            | list    | dict  | set   | deque |
|----------------------|---------|-------|-------|-------|
| Access by index      | O(1)    | -     | -     | O(n)  |
| Search               | O(n)    | O(1)  | O(1)  | O(n)  |
| Insert at end        | O(1)    | O(1)  | O(1)  | O(1)  |
| Insert at front      | O(n)    | -     | -     | O(1)  |
| Delete (known index) | O(n)    | O(1)  | O(1)  | O(n)  |
| Sort                 | O(n log n)| -   | -     | -     |

---

## Quick Reference: Sorting Algorithms (Timsort)

Python's `list.sort()` and `sorted()` both use **Timsort**:
- Hybrid of merge sort + insertion sort
- **Stable** (equal elements maintain original order)
- Best: O(n) — already sorted / nearly sorted
- Average/Worst: O(n log n)
- Space: O(n)

---

---

## 24. Strings & Formatting

> **Definition:** In Python, `str` is an **immutable sequence of Unicode code points** (not bytes). Every string operation returns a new string object. Python offers three modern formatting approaches: **f-strings** (fastest, most readable, Python 3.6+), `str.format()` (flexible, positional/keyword), and `%` formatting (legacy). Encoding (`str → bytes`) and decoding (`bytes → str`) are required when crossing the boundary to I/O or network operations.

### String Methods
```python
s = "  Hello, World!  "

s.strip()            # "Hello, World!"   — remove whitespace
s.lstrip()           # "Hello, World!  " — left only
s.rstrip()           # "  Hello, World!" — right only
s.upper()            # "  HELLO, WORLD!  "
s.lower()
s.title()            # "  Hello, World!  "
s.capitalize()       # "  hello, world!  "

s.startswith("He")
s.endswith("!")
s.find("World")      # first index, -1 if not found
s.index("World")     # like find but raises ValueError if missing
s.count("l")         # 3

s.replace("World", "Python")
s.split(", ")        # ["  Hello", "World!  "]
s.split()            # splits on any whitespace, strips edges
", ".join(["a", "b", "c"])   # "a, b, c"

s.isdigit()          # True if all chars are digits
s.isalpha()          # True if all chars are letters
s.isalnum()          # letters or digits
s.isspace()          # all whitespace

"hello".center(11, "-")   # "---hello---"
"42".zfill(5)             # "00042"
```

### f-strings (Python 3.6+)
```python
name = "Alice"
score = 95.678

f"Hello {name}"
f"Score: {score:.2f}"         # 2 decimal places → 95.68
f"Score: {score:>10.2f}"      # right-align in width 10
f"Hex: {255:#x}"              # 0xff
f"Binary: {10:08b}"           # 00001010
f"{name!r}"                   # repr(name) → "'Alice'"
f"{name!s}"                   # str(name)
f"{2 + 2}"                    # expressions allowed
f"{'yes' if score > 90 else 'no'}"

# Multiline f-string
msg = (
    f"Name: {name}\n"
    f"Score: {score}"
)

# Python 3.12+ — self-documenting expressions
x = 42
f"{x = }"   # "x = 42"
```

### `.format()` and `%` formatting
```python
"{} and {}".format("a", "b")
"{name} is {age}".format(name="Alice", age=30)
"{0:.2f}".format(3.14159)

# Old style (still seen in legacy code)
"%s is %d years old" % ("Alice", 30)
"%.2f" % 3.14159
```

### Bytes ↔ str
```python
# str → bytes
b = "hello".encode("utf-8")   # b'hello'
b = "hello".encode()          # default utf-8

# bytes → str
s = b.decode("utf-8")         # "hello"

# bytes literal
data = b"\x48\x65\x6c\x6c\x6f"  # b'Hello'
```

### String Internals
- Strings are **immutable** sequences of Unicode code points.
- `+` concatenation creates a new object every time → O(n²) in a loop.
- Use `"".join(parts)` for building strings in loops → O(n).
- String interning: short strings with identifier-like characters are often cached.

---

## 25. Standard Library — os, sys, subprocess

> **Definition:**
> - **`os`** — portable interface to operating system services: file system operations, environment variables, process information. Prefer `pathlib` for path manipulation in new code.
> - **`sys`** — access to the Python interpreter itself: command-line arguments (`argv`), the module search path (`path`), stdin/stdout/stderr streams, and interpreter metadata.
> - **`subprocess`** — spawn and communicate with child processes. The recommended API is `subprocess.run()` (Python 3.5+); use `Popen` only when you need streaming or interactive control.

### os module
```python
import os

os.getcwd()                    # current working directory
os.chdir("/tmp")               # change directory
os.listdir(".")                # list directory entries
os.makedirs("a/b/c", exist_ok=True)
os.remove("file.txt")
os.rename("old.txt", "new.txt")
os.path.join("dir", "file.txt")
os.path.exists("file.txt")
os.path.isfile("x")
os.path.isdir("x")
os.path.basename("/a/b/file.txt")   # "file.txt"
os.path.dirname("/a/b/file.txt")    # "/a/b"
os.path.splitext("file.txt")        # ("file", ".txt")
os.path.abspath("../x")             # absolute path

os.environ["HOME"]             # environment variable
os.environ.get("PORT", "8080") # with default
os.getpid()                    # process ID
os.cpu_count()                 # number of CPUs
```

### sys module
```python
import sys

sys.argv             # ['script.py', 'arg1', 'arg2']
sys.argv[0]          # script name
sys.argv[1:]         # actual arguments

sys.exit(0)          # exit with code (0 = success)
sys.exit("Error!")   # exit with message (non-zero)

sys.path             # list of dirs Python searches for modules
sys.path.insert(0, "/custom/path")  # prepend custom path

sys.version          # "3.11.0 ..."
sys.platform         # "linux", "darwin", "win32"

sys.stdin            # standard input stream
sys.stdout           # standard output stream
sys.stderr           # standard error stream

sys.getsizeof(obj)   # memory size of object in bytes
sys.getrecursionlimit()        # default 1000
sys.setrecursionlimit(5000)
```

### subprocess module
```python
import subprocess

# Simple run (recommended for most use cases)
result = subprocess.run(
    ["ls", "-la"],
    capture_output=True,    # capture stdout/stderr
    text=True,              # decode bytes → str
    check=True              # raise CalledProcessError on non-zero exit
)
print(result.stdout)
print(result.returncode)

# Shell string (use with caution — shell injection risk)
subprocess.run("echo hello | wc -c", shell=True, text=True)

# Check output shorthand
out = subprocess.check_output(["git", "log", "--oneline"], text=True)

# Popen — full control, streaming
proc = subprocess.Popen(
    ["python", "worker.py"],
    stdout=subprocess.PIPE,
    stderr=subprocess.PIPE
)
stdout, stderr = proc.communicate(timeout=30)
```

---

## 26. Standard Library — datetime & time

> **Definition:** Python's `datetime` module provides **timezone-aware and naive** date/time objects. Key distinction: a *naive* datetime has no timezone info and is ambiguous; an *aware* datetime carries a `tzinfo` and is unambiguous. `timedelta` represents a duration. Always use `datetime.now(tz=timezone.utc)` for UTC timestamps. The `time` module provides low-level Unix timestamps and high-resolution performance counters.

```python
from datetime import date, time, datetime, timedelta, timezone

# date
today = date.today()                  # date(2024, 3, 15)
d = date(2024, 1, 1)
d.year, d.month, d.day
d.weekday()    # 0=Mon … 6=Sun
d.isoformat()  # "2024-01-01"
d.strftime("%d/%m/%Y")   # "01/01/2024"

# datetime
now = datetime.now()                   # local time
utc = datetime.now(tz=timezone.utc)   # UTC (timezone-aware)
dt = datetime(2024, 3, 15, 10, 30, 0)
dt.strftime("%Y-%m-%d %H:%M:%S")
datetime.strptime("2024-03-15", "%Y-%m-%d")   # parse string

# timedelta — duration
delta = timedelta(days=7, hours=3, minutes=30)
tomorrow = today + timedelta(days=1)
diff = datetime(2024, 12, 31) - datetime.now()
diff.days         # number of days
diff.total_seconds()

# Common format codes
# %Y  4-digit year    %m  month (01-12)   %d  day (01-31)
# %H  hour (00-23)    %M  minute (00-59)  %S  second (00-59)
# %A  weekday name    %B  month name      %f  microseconds
```

```python
import time

time.time()           # Unix timestamp as float (seconds since epoch)
time.sleep(1.5)       # pause execution
time.perf_counter()   # high-res timer for benchmarking

start = time.perf_counter()
# ... do work ...
elapsed = time.perf_counter() - start
```

---

## 27. Enums

> **Definition:** An `Enum` is a set of **symbolic names bound to unique, constant values**. Enums improve code readability and safety by replacing magic numbers or strings with named constants that carry type identity. Python's `enum` module provides `Enum` (general), `IntEnum` (int-comparable), `Flag` (bitwise-combinable), and `auto()` for automatic value assignment. Enum members are singletons — comparison via `is` is safe.

```python
from enum import Enum, IntEnum, Flag, auto

class Color(Enum):
    RED   = 1
    GREEN = 2
    BLUE  = 3

Color.RED           # <Color.RED: 1>
Color.RED.name      # "RED"
Color.RED.value     # 1
Color(1)            # <Color.RED: 1>  — lookup by value
Color["RED"]        # <Color.RED: 1>  — lookup by name
list(Color)         # all members

# auto() — auto-assigns values
class Direction(Enum):
    NORTH = auto()   # 1
    SOUTH = auto()   # 2
    EAST  = auto()   # 3
    WEST  = auto()   # 4

# IntEnum — behaves like int
class Status(IntEnum):
    OK      = 200
    NOT_FOUND = 404

Status.OK == 200    # True (int comparison)
Status.OK > 100     # True

# Flag — supports bitwise operations
class Permission(Flag):
    READ    = auto()   # 1
    WRITE   = auto()   # 2
    EXECUTE = auto()   # 4

p = Permission.READ | Permission.WRITE   # combined
Permission.READ in p                     # True

# Enum with methods
class Planet(Enum):
    MERCURY = (3.303e+23, 2.4397e6)
    EARTH   = (5.976e+24, 6.37814e6)

    def __init__(self, mass, radius):
        self.mass   = mass
        self.radius = radius

    def surface_gravity(self):
        G = 6.67430e-11
        return G * self.mass / (self.radius ** 2)
```

---

## 28. Exception Hierarchy

> **Definition:** Python exceptions form a **class hierarchy** rooted at `BaseException`. `Exception` is the practical base for all user-facing errors. The hierarchy enables catching broad categories (e.g. `OSError` catches all OS-related errors) or specific ones (`FileNotFoundError`). Always catch the **most specific** exception first. Exception **chaining** (`raise X from Y`) preserves the original cause in the traceback, which is critical for debugging.

```
BaseException
├── SystemExit          ← sys.exit()
├── KeyboardInterrupt   ← Ctrl+C
├── GeneratorExit       ← generator.close()
└── Exception           ← catch-all for most errors
    ├── StopIteration
    ├── ArithmeticError
    │   ├── ZeroDivisionError
    │   ├── OverflowError
    │   └── FloatingPointError
    ├── LookupError
    │   ├── IndexError
    │   └── KeyError
    ├── AttributeError
    ├── NameError
    │   └── UnboundLocalError
    ├── TypeError
    ├── ValueError
    │   └── UnicodeError
    ├── OSError (IOError, EnvironmentError)
    │   ├── FileNotFoundError
    │   ├── PermissionError
    │   └── TimeoutError
    ├── RuntimeError
    │   └── RecursionError
    ├── ImportError
    │   └── ModuleNotFoundError
    ├── MemoryError
    └── NotImplementedError
```

```python
# Catching by hierarchy — catch specific before general
try:
    ...
except FileNotFoundError:   # more specific first
    ...
except OSError:             # broader parent class
    ...
except Exception as e:      # last resort
    ...

# Exception chaining
try:
    int("abc")
except ValueError as e:
    raise RuntimeError("parse failed") from e
    # preserves original traceback (e.__cause__)

# Suppress with cause hidden
raise RuntimeError("failed") from None
```

---

## 29. Logging

> **Definition:** The `logging` module provides a flexible, **hierarchical logging framework** that separates log generation from log output. Unlike `print()`, logging supports levels (DEBUG → CRITICAL), formatters, multiple handlers (file, console, network), and logger namespaces that mirror the module hierarchy. Libraries should always use `logging.getLogger(__name__)` and never call `basicConfig()` — that is the application's responsibility.

```python
import logging

# Basic configuration (call once at app startup)
logging.basicConfig(
    level=logging.DEBUG,
    format="%(asctime)s [%(levelname)s] %(name)s: %(message)s",
    datefmt="%Y-%m-%d %H:%M:%S",
    handlers=[
        logging.StreamHandler(),                      # console
        logging.FileHandler("app.log", encoding="utf-8"),  # file
    ]
)

# Log levels (lowest to highest)
logging.debug("Detailed info for diagnosing problems")     # 10
logging.info("Confirmation that things are working")       # 20
logging.warning("Something unexpected, still working")     # 30
logging.error("Serious problem, some function failed")     # 40
logging.critical("Program may be unable to continue")      # 50

# Module-level loggers (best practice)
logger = logging.getLogger(__name__)   # e.g. "myapp.utils"
logger.info("Module-level log")

# Log exceptions with traceback
try:
    1 / 0
except ZeroDivisionError:
    logger.exception("Division failed")   # includes full traceback

# Logger hierarchy
# root → myapp → myapp.utils
# Child loggers propagate to parent by default
logging.getLogger("myapp").setLevel(logging.WARNING)
logging.getLogger("myapp.utils").setLevel(logging.DEBUG)
```

### Handler types
| Handler              | Destination           |
|----------------------|-----------------------|
| `StreamHandler`      | stdout / stderr       |
| `FileHandler`        | file                  |
| `RotatingFileHandler`| file with size limit  |
| `TimedRotatingFileHandler` | file by time   |
| `NullHandler`        | discard (library use) |

---

## 30. Testing — unittest & pytest

> **Definition:** Testing verifies that code behaves as expected. Python ships with **`unittest`** (xUnit-style, class-based, built-in). **`pytest`** (third-party) is the industry standard — it uses plain `assert` statements, supports powerful fixtures for dependency injection, and has a rich plugin ecosystem. **`unittest.mock`** replaces real dependencies with controllable fakes, isolating the unit under test from external systems.

### unittest
```python
import unittest

class TestMath(unittest.TestCase):

    def setUp(self):          # runs before EACH test
        self.values = [1, 2, 3]

    def tearDown(self):       # runs after EACH test
        pass

    @classmethod
    def setUpClass(cls):      # runs ONCE before all tests in class
        cls.db = connect()

    @classmethod
    def tearDownClass(cls):   # runs ONCE after all tests in class
        cls.db.close()

    def test_sum(self):
        self.assertEqual(sum(self.values), 6)

    def test_type(self):
        self.assertIsInstance(self.values, list)

    def test_raises(self):
        with self.assertRaises(ZeroDivisionError):
            1 / 0

# Assert methods
# assertEqual / assertNotEqual
# assertTrue / assertFalse
# assertIsNone / assertIsNotNone
# assertIn / assertNotIn
# assertRaises / assertRaisesRegex
# assertAlmostEqual (floats)
# assertDictEqual / assertListEqual

if __name__ == "__main__":
    unittest.main()
```

### pytest
```python
# pytest auto-discovers test_*.py files and test_* functions

def test_add():
    assert 1 + 1 == 2      # plain assert — pytest rewrites it for detail

def test_raises():
    import pytest
    with pytest.raises(ValueError, match="invalid"):
        int("abc")

# Fixtures — dependency injection for test setup
import pytest

@pytest.fixture
def db_conn():
    conn = create_connection()
    yield conn              # setup done, yield resource
    conn.close()            # teardown after test

def test_query(db_conn):    # fixture injected automatically
    result = db_conn.query("SELECT 1")
    assert result is not None

# Parametrize — run one test with multiple inputs
@pytest.mark.parametrize("x,y,expected", [
    (1, 2, 3),
    (0, 0, 0),
    (-1, 1, 0),
])
def test_add(x, y, expected):
    assert x + y == expected

# Markers
@pytest.mark.skip(reason="not ready")
def test_wip(): ...

@pytest.mark.xfail
def test_known_bug(): ...

@pytest.mark.slow
def test_heavy(): ...
# Run only slow: pytest -m slow
```

### unittest.mock
```python
from unittest.mock import MagicMock, patch, call

# MagicMock — auto-creates attributes and return values
m = MagicMock()
m.method(1, 2)
m.method.assert_called_once_with(1, 2)
m.method.return_value = 42

# patch — temporarily replace an object
with patch("mymodule.requests.get") as mock_get:
    mock_get.return_value.status_code = 200
    mock_get.return_value.json.return_value = {"key": "val"}
    result = fetch_data("http://example.com")

# patch as decorator
@patch("mymodule.send_email")
def test_signup(mock_email):
    signup("user@example.com")
    mock_email.assert_called_once()
```

---

## 31. Design Patterns (Pythonic)

> **Definition:** Design patterns are **reusable solutions to common software design problems**. In Python, many classic GoF (Gang of Four) patterns simplify significantly because functions are first-class objects, and duck typing reduces the need for abstract interfaces. The patterns below show idiomatic Python implementations rather than verbose Java-style class hierarchies.

### Singleton
```python
class Singleton(type):
    _instances = {}
    def __call__(cls, *a, **kw):
        if cls not in cls._instances:
            cls._instances[cls] = super().__call__(*a, **kw)
        return cls._instances[cls]

class Config(metaclass=Singleton):
    pass
```

### Factory
```python
class Animal:
    @staticmethod
    def create(kind):
        return {"dog": Dog, "cat": Cat}[kind]()

# Or using a registry dict
SHAPES = {}
def register(name):
    def deco(cls):
        SHAPES[name] = cls
        return cls
    return deco

@register("circle")
class Circle: ...

shape = SHAPES["circle"]()
```

### Observer (Event system)
```python
class EventBus:
    def __init__(self):
        self._listeners = {}

    def subscribe(self, event, fn):
        self._listeners.setdefault(event, []).append(fn)

    def publish(self, event, **data):
        for fn in self._listeners.get(event, []):
            fn(**data)

bus = EventBus()
bus.subscribe("user.created", lambda name: print(f"Welcome {name}!"))
bus.publish("user.created", name="Alice")
```

### Strategy
```python
from typing import Callable

class Sorter:
    def __init__(self, strategy: Callable):
        self.strategy = strategy

    def sort(self, data):
        return self.strategy(data)

sorter = Sorter(strategy=sorted)
sorter = Sorter(strategy=lambda d: sorted(d, reverse=True))
```

### Decorator Pattern (Object, not @decorator)
```python
class CachedReader:
    def __init__(self, reader):
        self._reader = reader
        self._cache = {}

    def read(self, path):
        if path not in self._cache:
            self._cache[path] = self._reader.read(path)
        return self._cache[path]
```

---

## 32. Python Execution Model

> **Definition:** When you run a `.py` file, CPython **compiles** it to platform-independent bytecode (`.pyc` stored in `__pycache__`), then the CPython **virtual machine** (a stack-based interpreter) executes the bytecode instructions one by one. Understanding this model helps diagnose import issues, measure performance, and appreciate why PyPy (which JIT-compiles bytecode to machine code) is much faster for CPU-bound loops.

```
Source Code (.py)
      │
      ▼  compile()
   Bytecode (.pyc — in __pycache__)
      │
      ▼  CPython VM
   Execution (bytecode interpreter)
```

### Bytecode & dis
```python
import dis

def add(a, b):
    return a + b

dis.dis(add)
# LOAD_FAST 0 (a)
# LOAD_FAST 1 (b)
# BINARY_OP 0 (+)
# RETURN_VALUE

# inspect bytecode object
code = add.__code__
code.co_varnames    # ('a', 'b')
code.co_consts      # (None,)
code.co_argcount    # 2
```

### CPython vs Alternatives
| Implementation | Description                           |
|----------------|---------------------------------------|
| **CPython**    | Reference impl., C, most widely used  |
| **PyPy**       | JIT compiler, 5-10x faster for loops  |
| **Jython**     | Python on JVM, Java interop           |
| **IronPython** | Python on .NET / CLR                  |
| **MicroPython**| Python for microcontrollers           |

### Import Machinery
```python
# Python's import steps:
# 1. Check sys.modules (cache)
# 2. Find module via sys.meta_path finders
# 3. Load and execute module code
# 4. Cache in sys.modules

import sys
"os" in sys.modules    # True after first import
del sys.modules["os"]  # force re-import next time

# __pycache__ stores .pyc files
# Recompiled only when source .py changes (mtime check)
```

---

## 33. Virtual Environments & Packaging

> **Definition:** A **virtual environment** is an isolated Python installation with its own `site-packages`, preventing dependency conflicts between projects. `venv` (built-in) creates lightweight environments. **pip** is the standard package installer, using `requirements.txt` for reproducibility. The modern standard is **`pyproject.toml`** (PEP 517/518/621), which replaces `setup.py`/`setup.cfg`. **poetry** is a popular all-in-one tool for dependency management, virtual envs, and publishing.

### venv (built-in)
```bash
# Create
python -m venv .venv

# Activate
source .venv/bin/activate      # macOS / Linux
.venv\Scripts\activate         # Windows

# Deactivate
deactivate

# Install packages
pip install requests
pip install -r requirements.txt

# Freeze current environment
pip freeze > requirements.txt
```

### pyproject.toml (modern standard — PEP 517/518/621)
```toml
[build-system]
requires = ["setuptools>=68", "wheel"]
build-backend = "setuptools.backends.legacy:build"

[project]
name = "myapp"
version = "1.0.0"
requires-python = ">=3.11"
dependencies = [
    "requests>=2.28",
    "pydantic>=2.0",
]

[project.optional-dependencies]
dev = ["pytest", "mypy", "ruff"]

[project.scripts]
myapp = "myapp.cli:main"
```

### poetry (popular alternative)
```bash
poetry new myproject
poetry add requests
poetry add --group dev pytest
poetry install          # install all deps from poetry.lock
poetry run python main.py
poetry build            # build wheel + sdist
poetry publish          # publish to PyPI
```

---

## 34. TypedDict, NamedTuple vs Dataclass

> **Definition:** Python offers three ways to define **structured data records**, each with different trade-offs:
> - **`TypedDict`** — a plain `dict` with type annotations; useful for JSON/API shapes. No overhead, no new class behaviour.
> - **`NamedTuple`** — an immutable `tuple` subclass with named fields; supports index access and unpacking.
> - **`@dataclass`** — a full class with auto-generated methods; supports mutability, inheritance, and `__post_init__` logic.

### TypedDict — typed dict structure (no runtime enforcement)
```python
from typing import TypedDict, NotRequired

class UserDTO(TypedDict):
    id: int
    name: str
    email: NotRequired[str]   # optional key

user: UserDict = {"id": 1, "name": "Alice"}
# Still a plain dict at runtime — useful for JSON / APIs
```

### NamedTuple — immutable record with field names
```python
from typing import NamedTuple

class Point(NamedTuple):
    x: float
    y: float
    label: str = ""

p = Point(1.0, 2.0)
p.x, p.y        # 1.0, 2.0
p[0], p[1]      # also accessible by index (it's a tuple!)
x, y, _ = p     # unpack
p._asdict()     # {"x": 1.0, "y": 2.0, "label": ""}
p._replace(x=5) # returns new instance
```

### Comparison Table
| Feature                | TypedDict | NamedTuple | dataclass      |
|------------------------|-----------|------------|----------------|
| Mutable                | Yes (dict)| No         | Yes (default)  |
| Type checked           | Static    | Static     | Static         |
| Runtime enforcement    | No        | No         | No             |
| Index access           | No        | Yes        | No             |
| dict methods           | Yes       | No         | No             |
| Inheritance            | Yes       | Yes        | Yes            |
| Default values         | No*       | Yes        | Yes            |
| Memory efficiency      | Low       | High       | Medium         |
| Best for               | JSON/APIs | Records    | Domain objects |

---

## 35. Protocols & Structural Subtyping

Python's **structural subtyping** (duck typing made explicit) — if it has the right methods, it's compatible, regardless of inheritance.

```python
from typing import Protocol, runtime_checkable

@runtime_checkable
class Drawable(Protocol):
    def draw(self) -> None: ...
    def resize(self, factor: float) -> None: ...

class Circle:        # does NOT inherit Drawable
    def draw(self): print("draw circle")
    def resize(self, factor): self.radius *= factor

class Square:
    def draw(self): print("draw square")
    def resize(self, factor): self.side *= factor

def render(shape: Drawable) -> None:
    shape.draw()

render(Circle())   # OK — Circle satisfies Drawable structurally
render(Square())   # OK

# runtime_checkable allows isinstance checks
isinstance(Circle(), Drawable)   # True
```

### Protocol vs ABC
| | Protocol | ABC |
|---|---|---|
| Subtyping | Structural (implicit) | Nominal (explicit) |
| Registration needed | No | Yes (`register()` or inherit) |
| `isinstance` support | With `@runtime_checkable` | Always |
| Use case | Duck typing, interop | Enforced interface, `abstractmethod` |

### Common Built-in Protocols
```python
from typing import (
    Iterable,        # __iter__
    Iterator,        # __iter__ + __next__
    Sequence,        # __getitem__ + __len__
    Mapping,         # __getitem__ + __len__ + __iter__
    Callable,        # __call__
    Awaitable,       # __await__
    AsyncIterable,   # __aiter__
    ContextManager,  # __enter__ + __exit__
)
```

---

## 36. Python 3.10+ Modern Features

> **Definition:** Each Python release adds syntax and standard library improvements. Python 3.10 introduced **structural pattern matching** (`match`/`case`) — similar to switch statements but far more powerful, supporting destructuring of sequences, mappings, and class instances. 3.11 brought significant performance improvements (~25% faster), `ExceptionGroup`, and `tomllib`. 3.12+ continues refinements to the type system and f-strings.

### Structural Pattern Matching (`match`/`case`) — Python 3.10
```python
command = ("move", 10, 20)

match command:
    case ("quit",):
        quit()
    case ("move", x, y):
        print(f"Move to {x}, {y}")
    case ("say", message):
        print(message)
    case _:
        print("unknown command")

# Match with guards
match point:
    case (x, y) if x == y:
        print("On diagonal")
    case (x, y):
        print(f"Point at {x}, {y}")

# Match dataclass/class
match event:
    case Click(position=(x, y)):
        handle_click(x, y)
    case KeyPress(key=key_name):
        handle_key(key_name)
```

### Other Modern Additions
```python
# Union type with | (3.10+)
def f(x: int | str) -> int | None: ...

# Match built-in types in isinstance (3.10+)
isinstance(x, int | str)

# Exception groups & except* (3.11+)
try:
    async with asyncio.TaskGroup() as tg:
        tg.create_task(task1())
        tg.create_task(task2())
except* ValueError as eg:
    for e in eg.exceptions:
        print(e)

# tomllib — built-in TOML parser (3.11+)
import tomllib
with open("pyproject.toml", "rb") as f:
    config = tomllib.load(f)

# Self type (3.11+)
from typing import Self
class Builder:
    def set_name(self, name: str) -> Self:
        self.name = name
        return self

# TypeAlias (3.10+)
from typing import TypeAlias
Vector: TypeAlias = list[float]
```

---

*End of Notes — Python In-Depth Reference*
