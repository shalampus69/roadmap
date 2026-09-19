# 🐍 Python + Git + Docker: Comprehensive Learning Roadmap

> **For:** Parsa  
> **Goal:** Master Python (OOP, APIs, HTTP), integrate Git into workflow, learn Docker for deployment  
> **Style:** Project-oriented with deep explanations, spoiler-tagged solutions, and Git integrated from Day 1  
> **Date:** September 2026

---

## 📖 How to Use This Roadmap

- **Each phase** has a learning section with deep explanations + code snippets, followed by **projects** you build yourself.
- **Projects** have clear requirements and hints. Try them *before* reading solutions.
- **Spoiler blocks** (`>! SPOILER`) contain full solutions — expand them *after* you've attempted the project.
- **Git checkpoints** appear throughout — these are not optional side lessons, they're part of the workflow. You'll commit after every meaningful milestone.
- **Docker** enters gradually: first as a "just run this command" thing, then deeper as you go.
- **Time estimates** are rough — adjust to your pace.

---

## 🗺️ Roadmap Overview

| Phase | Topic | Key Projects | Git Integration | Docker |
|-------|-------|-------------|-----------------|--------|
| 1 | Python Foundations Review | CLI Task Tracker | Init, commit, branch, merge | — |
| 2 | OOP & Python Deep Dive | Library System, Contact Book | Tags, history, stash | — |
| 3 | Consuming HTTP APIs | Weather Dashboard, Movie CLI | Remote repos, push/pull | — |
| 4 | Building APIs (Flask/FastAPI) | Todo API, Blog API | Feature branches, PRs | — |
| 5 | Databases & ORM | Bookshelf, Inventory System | .gitignore, migration commits | — |
| 6 | Docker Fundamentals | Containerize everything so far | Dockerfile commits | **Core intro** |
| 7 | Full-Stack Integration | URL Shortener, Chat App | Rebase, CI basics | Compose, networking |
| 8 | Capstone Projects | Finance Tracker, Blog Platform, Task Queue | Full workflow | Production builds |

**Prerequisites before starting:**
- Python 3.10+ installed
- Git installed (`git --version`)
- A text editor (VS Code recommended)
- A GitHub account

---

## Phase 1: Python Foundations Review + Git Basics

> **Duration:** 2–4 days  
> **Goal:** Refresh Python fundamentals, set up Git workflow habits from day one

### 1.1 — Quick Python Review

You already know Python basics, so this is a rapid-fire review with depth on things people often have gaps in.

#### Variables, Types & Mutability

```python
# Python is dynamically typed but strongly typed
x = 42          # int
y = 3.14        # float
s = "hello"     # str
b = True        # bool
n = None        # NoneType (the only null)

# Mutable vs Immutable — this matters A LOT
# Immutable: int, float, str, tuple, frozenset
# Mutable: list, dict, set

a = [1, 2, 3]
b = a            # b points to the SAME list
b.append(4)
print(a)         # [1, 2, 3, 4] — a changed too!

# To make an independent copy:
a = [1, 2, 3]
b = a.copy()     # or b = a[:]
b.append(4)
print(a)         # [1, 2, 3] — a is safe
```

**Why this matters:** Most "bug" conversations in Python forums come from mutable default arguments or aliasing. Understand this deeply now.

#### String Formatting (All Three Ways)

```python
name = "Parsa"
age = 25

# 1. f-strings (preferred — fast, readable)
print(f"Name: {name}, Age: {age}")
print(f"2 + 3 = {2 + 3}")          # expressions inside {}
print(f"{3.14159:.2f}")             # formatting: 3.14
print(f"{'centered':^20}")          # ^ = center, < = left, > = right
print(f"{'binary':>10}")            # right-align in 10 chars

# 2. .format() (older, still seen in codebases)
print("Name: {}, Age: {}".format(name, age))
print("{name} is {age}".format(name="Parsa", age=25))

# 3. % formatting (legacy, you'll see it in old code)
print("Name: %s, Age: %d" % (name, age))
```

#### List, Dict, Set Comprehensions

```python
# List comprehension
squares = [x**2 for x in range(10)]
evens = [x for x in range(20) if x % 2 == 0]
# With condition + transformation:
result = [x * 2 if x % 2 == 0 else x for x in range(10)]

# Dict comprehension
word_lengths = {word: len(word) for word in ["hello", "world", "python"]}
# {'hello': 5, 'world': 5, 'python': 6}

# Set comprehension (unique values)
unique_lengths = {len(word) for word in ["hello", "world", "python"]}

# Generator expression (memory efficient — evaluates lazily)
total = sum(x**2 for x in range(1_000_000))  # no brackets needed for single arg
```

#### Collections Module (Underrated Power Tools)

```python
from collections import Counter, defaultdict, namedtuple, deque

# Counter — count things
words = ["apple", "banana", "apple", "cherry", "banana", "apple"]
counts = Counter(words)
print(counts.most_common(2))  # [('apple', 3), ('banana', 2)]

# defaultdict — dict that auto-creates missing keys
groups = defaultdict(list)
for word in words:
    groups[word[0]].append(word)
# {'a': ['apple', 'apple', 'apple'], 'b': ['banana', 'banana'], 'c': ['cherry']}

# namedtuple — lightweight class without boilerplate
Point = namedtuple('Point', ['x', 'y'])
p = Point(3, 4)
print(p.x, p.y)  # 3 4

# deque — double-ended queue (fast appends/pops on both ends)
queue = deque()
queue.append("first")
queue.appendleft("before-first")
queue.pop()          # "first"
queue.popleft()      # "before-first"
```

#### Error Handling

```python
# Always catch SPECIFIC exceptions, never bare except
def safe_divide(a, b):
    try:
        result = a / b
    except ZeroDivisionError:
        return None
    except TypeError as e:
        print(f"Type error: {e}")
        raise  # re-raise if you can't handle it
    else:
        # Runs ONLY if no exception occurred
        print(f"Success: {a} / {b} = {result}")
        return result
    finally:
        # ALWAYS runs — cleanup code
        print("Division attempt complete")

# Custom exceptions
class AppError(Exception):
    """Base exception for our app."""
    pass

class ValidationError(AppError):
    """Raised when input validation fails."""
    def __init__(self, field, message):
        self.field = field
        self.message = message
        super().__init__(f"{field}: {message}")

# Raising with context
def validate_age(age):
    if not isinstance(age, int):
        raise ValidationError("age", "must be an integer")
    if age < 0 or age > 150:
        raise ValidationError("age", "must be between 0 and 150")
    return True
```

#### File I/O

```python
from pathlib import Path

# Modern approach — pathlib (object-oriented, cross-platform)
data_dir = Path("data")
data_dir.mkdir(exist_ok=True)  # create if not exists

# Writing
file_path = data_dir / "notes.txt"  # Path concatenation with /
file_path.write_text("Hello, world!\nLine 2\n")

# Reading
content = file_path.read_text()
print(content)

# Reading line by line (memory efficient for large files)
for line in file_path.open():
    print(line.strip())

# JSON
import json

data = {"users": [{"name": "Parsa", "age": 25}]}
json_path = data_dir / "data.json"
json_path.write_text(json.dumps(data, indent=2))

loaded = json.loads(json_path.read_text())
print(loaded["users"][0]["name"])  # Parsa

# CSV
import csv

csv_path = data_dir / "users.csv"
with csv_path.open("w", newline="") as f:
    writer = csv.DictWriter(f, fieldnames=["name", "age"])
    writer.writeheader()
    writer.writerow({"name": "Parsa", "age": 25})
    writer.writerow({"name": "Ali", "age": 30})

with csv_path.open() as f:
    reader = csv.DictReader(f)
    for row in reader:
        print(row["name"], row["age"])
```

#### Lambda, Map, Filter, Sorted

```python
# Lambda — anonymous one-line function
square = lambda x: x ** 2
print(square(5))  # 25

# map — apply function to every element
nums = [1, 2, 3, 4]
doubled = list(map(lambda x: x * 2, nums))  # [2, 4, 6, 8]
# Equivalent (and preferred) with comprehension:
doubled = [x * 2 for x in nums]

# filter — keep elements where function returns True
odds = list(filter(lambda x: x % 2 != 0, nums))  # [1, 3]
# Equivalent:
odds = [x for x in nums if x % 2 != 0]

# sorted with key — sort by custom criteria
students = [("Parsa", 90), ("Ali", 85), ("Sara", 95)]
by_grade = sorted(students, key=lambda s: s[1], reverse=True)
# [('Sara', 95), ('Parsa', 90), ('Ali', 85)]

# sorted with operator for complex keys
from operator import itemgetter
by_grade = sorted(students, key=itemgetter(1), reverse=True)
```

### 1.2 — Git Basics: Your First Workflow

#### What is Git?

Git is a **distributed version control system**. It tracks changes to files over time, lets you revert mistakes, branch off to experiment, and collaborate with others. Every developer on a project has a full copy of the history.

#### Core Concepts

```
Working Directory  →  Staging Area  →  Repository
   (your files)      (git add)        (git commit)

  What you edit    What you prepare   What you save
```

- **Repository (repo):** The `.git/` directory — stores all history, branches, metadata.
- **Commit:** A snapshot of your project at a point in time. Every commit has a unique SHA hash.
- **Branch:** An independent line of development. `main` is the default.
- **HEAD:** Points to the current commit you're looking at.
- **Remote:** A version of your repo hosted somewhere (e.g., GitHub).

#### Setup (One Time)

```bash
# Install git (if not installed)
# Debian/Ubuntu:
sudo apt install git

# Configure identity (shows up in every commit)
git config --global user.name "Parsa"
git config --global user.email "your@email.com"

# Set default branch name
git config --global init.defaultBranch main

# Useful aliases (optional but saves time)
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.lg "log --oneline --graph --all"
```

#### First Git Workflow

```bash
# 1. Create a project and initialize git
mkdir my-project && cd my-project
git init                    # creates .git/ directory

# 2. Create some files
echo "# My Project" > README.md
echo "print('hello')" > main.py

# 3. Check status — shows you what's changed
git status
# Untracked files: README.md, main.py

# 4. Stage files (prepare for commit)
git add README.md main.py   # specific files
git add .                   # or everything

# 5. Commit (save the snapshot)
git commit -m "Initial commit: project setup"

# 6. Check history
git log --oneline
# a1b2c3d Initial commit: project setup
```

#### Gitignore — What NOT to Track

Create `.gitignore` in your project root:

```
# Python
__pycache__/
*.pyc
*.pyo
.env
venv/
.venv/

# IDE
.vscode/
.idea/

# OS
.DS_Store
Thumbs.db

# Project-specific
data/*.json
*.log
```

```bash
git add .gitignore
git commit -m "Add .gitignore"
```

#### Branching and Merging

```bash
# Create and switch to a new branch
git checkout -b feature/task-manager
# (same as: git branch feature/task-manager && git checkout feature/task-manager)

# Work on the feature...
# ... make commits ...

# Switch back to main
git checkout main

# Merge the feature branch
git merge feature/task-manager

# Delete the merged branch (cleanup)
git branch -d feature/task-manager
```

**Visual model of branching:**
```
main:       A --- B --- C
                     \
feature:              D --- E

After merge:
main:       A --- B --- C --- F (merge commit)
                     \       /
feature:              D --- E
```

#### Handling Merge Conflicts

```bash
# If git can't auto-merge, it marks conflicts in the file:
<<<<<<< HEAD
print("version from main")
=======
print("version from feature")
>>>>>>> feature/branch

# You edit the file to keep what you want, remove the markers, then:
git add .
git commit -m "Resolve merge conflict"
```

#### Undoing Things (Don't Panic)

```bash
# Discard changes in working directory (DANGEROUS — can't undo)
git checkout -- filename.py
# Or modern way:
git restore filename.py

# Unstage a file (keep changes, remove from staging)
git restore --staged filename.py

# Amend last commit (forgot a file, typo in message)
git add forgotten_file.py
git commit --amend -m "Fixed commit message"

# Revert a commit (creates a NEW commit that undoes changes)
git revert <commit-hash>

# View what changed
git diff                    # unstaged changes
git diff --staged           # staged changes
git diff main..feature      # difference between branches
```

#### Remote Repositories (GitHub)

```bash
# On GitHub, create a new empty repository (no README)

# Connect local to remote
git remote add origin https://github.com/YOUR_USERNAME/PROJECT.git

# Push (upload) your commits
git push -u origin main     # -u sets upstream, future pushes are just `git push`

# Clone an existing repo
git clone https://github.com/USER/REPO.git

# Pull (download changes from remote)
git pull origin main
```

### 1.3 — 📂 Project 1: CLI Task Tracker

**Objective:** Build a command-line task manager that persists to a JSON file, and manage it with Git from the very first commit.

**Features:**
- Add tasks with a title and priority (low/medium/high)
- List all tasks (filterable by status: todo/done)
- Mark tasks as done
- Delete tasks
- Tasks persist to `tasks.json`

**Starter commands:**
```bash
mkdir task-tracker && cd task-tracker
git init
touch task_tracker.py
echo "__pycache__/" > .gitignore
echo "*.pyc" >> .gitignore
git add .gitignore
git commit -m "Init: project setup with .gitignore"
```

**Expected CLI interface:**
```bash
python task_tracker.py add "Learn OOP" --priority high
python task_tracker.py list
python task_tracker.py list --status todo
python task_tracker.py done 1
python task_tracker.py delete 1
python task_tracker.py --help
```

**Example output:**
```
$ python task_tracker.py list

 ID │ Priority │ Status │ Title
────┼──────────┼────────┼─────────────────
  1 │ 🔴 high  │  todo  │ Learn OOP
  2 │ 🟡 medium│  todo  │ Read Flask docs
  3 │ 🟢 low   │  done  │ Install Python
```

**Hints (expand if stuck):**
> HINT 1: Use `argparse` for CLI argument parsing. It handles `--flags` and positional args automatically.
> 
> HINT 2: Store tasks as a list of dicts in JSON. Each task should have: `{"id": 1, "title": "...", "priority": "...", "status": "todo"}`.
> 
> HINT 3: Auto-increment IDs — find the max existing ID and add 1.
> 
> HINT 4: Use a try/except block around JSON loading to handle the case where the file doesn't exist yet.

**Git checkpoints for this project:**
```bash
# After getting add working:
git add task_tracker.py
git commit -m "feat: implement task addition"

# After listing:
git commit -am "feat: implement task listing with filters"

# After done/delete:
git commit -am "feat: implement done and delete commands"

# After polishing (colors, table formatting):
git commit -am "feat: improve output formatting"
```

---

## Phase 2: OOP & Python Deep Dive

> **Duration:** 4–7 days  
> **Goal:** Master OOP, decorators, generators, and advanced Python patterns  
> **Projects:** Library Management System, Contact Book with persistence

### 2.1 — Object-Oriented Programming (OOP) Deep Dive

OOP is about organizing code into **objects** — bundles of data (attributes) and behavior (methods). Python supports OOP but doesn't enforce it — you choose when it makes sense.

#### Classes and Objects — The Full Picture

```python
class Dog:
    """A Dog class — the blueprint for dog objects."""
    
    # Class attribute — shared by ALL instances
    species = "Canis familiaris"
    
    # Constructor — called when you do Dog(...)
    def __init__(self, name: str, age: int, breed: str = "Mixed"):
        # Instance attributes — unique to each object
        self.name = name
        self.age = age
        self.breed = breed
        self._tricks = []  # convention: single underscore = "internal"
    
    # Instance method — first param is always 'self'
    def bark(self) -> str:
        return f"{self.name} says: Woof!"
    
    # Method with logic
    def learn_trick(self, trick: str) -> None:
        if trick not in self._tricks:
            self._tricks.append(trick)
    
    def show_tricks(self) -> str:
        if not self._tricks:
            return f"{self.name} hasn't learned any tricks yet."
        return f"{self.name} knows: {', '.join(self._tricks)}"
    
    # String representation — what print() and str() show
    def __repr__(self) -> str:
        """Developer-friendly representation."""
        return f"Dog(name={self.name!r}, age={self.age}, breed={self.breed!r})"
    
    def __str__(self) -> str:
        """User-friendly representation."""
        return f"{self.name} ({self.breed}, {self.age} years old)"
    
    # Comparison
    def __eq__(self, other) -> bool:
        if not isinstance(other, Dog):
            return NotImplemented
        return self.name == other.name and self.breed == other.breed

# Usage
rex = Dog("Rex", 5, "German Shepherd")
buddy = Dog("Buddy", 3)

print(rex)                    # Rex (German Shepherd, 5 years old)
print(rex.bark())             # Rex says: Woof!
print(rex.species)            # Canis familiaris (from class attribute)
print(repr(rex))              # Dog(name='Rex', age=5, breed='German Shepherd')

rex.learn_trick("sit")
rex.learn_trick("roll over")
print(rex.show_tricks())     # Rex knows: sit, roll over
```

#### Inheritance — Building on Existing Classes

```python
class Animal:
    def __init__(self, name: str, sound: str):
        self.name = name
        self.sound = sound
    
    def speak(self) -> str:
        return f"{self.name} says {self.sound}!"
    
    def __repr__(self):
        return f"{self.__class__.__name__}(name={self.name!r})"

class Cat(Animal):
    """Cat inherits from Animal."""
    
    def __init__(self, name: str, indoor: bool = True):
        # Call parent constructor
        super().__init__(name, sound="Meow")
        self.indoor = indoor
    
    def describe(self) -> str:
        location = "indoor" if self.indoor else "outdoor"
        return f"{self.name} is an {location} cat"

class Dog(Animal):
    """Dog inherits from Animal."""
    
    def __init__(self, name: str, breed: str):
        super().__init__(name, sound="Woof")
        self.breed = breed
    
    def fetch(self, item: str) -> str:
        return f"{self.name} fetches the {item}!"

# Polymorphism — same method, different behavior
cat = Cat("Whiskers")
dog = Dog("Rex", "Labrador")

animals = [cat, dog]
for animal in animals:
    print(animal.speak())  # Each uses its own sound
# Whiskers says Meow!
# Rex says Woof!
```

#### Encapsulation and Properties

```python
class BankAccount:
    """Bank account with controlled access to balance."""
    
    def __init__(self, owner: str, balance: float = 0):
        self.owner = owner
        self.__balance = balance  # name mangling: _BankAccount__balance
        self._transaction_history = []
    
    # @property lets you access methods like attributes
    @property
    def balance(self) -> float:
        """Read-only access to balance."""
        return self.__balance
    
    @property
    def history(self) -> list:
        """Return a copy, not the internal list."""
        return self._transaction_history.copy()
    
    def deposit(self, amount: float) -> None:
        if amount <= 0:
            raise ValueError("Deposit must be positive")
        self.__balance += amount
        self._transaction_history.append(f"+{amount:.2f} (balance: {self.__balance:.2f})")
    
    def withdraw(self, amount: float) -> None:
        if amount <= 0:
            raise ValueError("Withdrawal must be positive")
        if amount > self.__balance:
            raise ValueError(f"Insufficient funds: have {self.__balance}, need {amount}")
        self.__balance -= amount
        self._transaction_history.append(f"-{amount:.2f} (balance: {self.__balance:.2f})")
    
    def __repr__(self):
        return f"BankAccount(owner={self.owner!r}, balance={self.__balance:.2f})"

acc = BankAccount("Parsa", 1000)
acc.deposit(500)
acc.withdraw(200)
print(acc.balance)       # 1300 (accessed like a property, not a method)
print(acc.history)       # ['+500.00 (balance: 1500.00)', '-200.00 (balance: 1300.00)']
# acc.__balance          # AttributeError — it's "private"
```

#### Class Methods and Static Methods

```python
from datetime import datetime

class Employee:
    raise_pct = 1.05  # 5% raise
    employee_count = 0
    
    def __init__(self, name: str, salary: float):
        self.name = name
        self.salary = salary
        Employee.employee_count += 1
        self.id = Employee.employee_count
    
    def apply_raise(self):
        self.salary *= self.raise_pct
    
    @classmethod
    def set_raise_pct(cls, pct: float):
        """Modify class-level state — cls is the class itself."""
        cls.raise_pct = pct
    
    @classmethod
    def from_string(cls, emp_str: str):
        """Alternative constructor — parse from string."""
        name, salary = emp_str.split("-")
        return cls(name, float(salary))
    
    @staticmethod
    def is_workday(day: datetime) -> bool:
        """No access to class or instance — just a utility function."""
        return day.weekday() < 5  # Monday-Friday

# Usage
emp1 = Employee("Parsa", 75000)
emp2 = Employee.from_string("Ali-80000")  # alternative constructor
Employee.set_raise_pct(1.10)              # modify for all instances
print(Employee.is_workday(datetime.now())) # True if weekday
```

#### Multiple Inheritance and MRO

```python
class A:
    def greet(self):
        return "Hello from A"

class B(A):
    def greet(self):
        return "Hello from B"

class C(A):
    def greet(self):
        return "Hello from C"

class D(B, C):  # D inherits from both B and C
    pass

d = D()
print(d.greet())  # "Hello from B" — uses MRO (Method Resolution Order)

# Check the order:
print(D.__mro__)
# (<class 'D'>, <class 'B'>, <class 'C'>, <class 'A'>, <class 'object'>)
# Python searches left to right, depth first
```

### 2.2 — Decorators

Decorators modify functions/classes without changing their code. They're used everywhere in Python (Flask routes, property, classmethod, etc.).

```python
import functools
import time

# === Basic Decorator Pattern ===
def timer(func):
    """Measure how long a function takes."""
    @functools.wraps(func)  # preserves func's name, docstring, etc.
    def wrapper(*args, **kwargs):
        start = time.perf_counter()
        result = func(*args, **kwargs)
        elapsed = time.perf_counter() - start
        print(f"{func.__name__} took {elapsed:.4f}s")
        return result
    return wrapper

@timer
def slow_function():
    time.sleep(1)
    return "done"

# slow_function()  # prints: slow_function took 1.0012s

# === Decorator with Arguments ===
def repeat(times):
    """Repeat a function `times` times."""
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            results = []
            for _ in range(times):
                results.append(func(*args, **kwargs))
            return results
        return wrapper
    return decorator

@repeat(times=3)
def say_hello(name):
    return f"Hello, {name}!"

# say_hello("Parsa")  # ["Hello, Parsa!", "Hello, Parsa!", "Hello, Parsa!"]

# === Practical: Login Required Decorator ===
def login_required(func):
    @functools.wraps(func)
    def wrapper(user, *args, **kwargs):
        if not user.get("is_authenticated"):
            raise PermissionError("You must be logged in")
        return func(user, *args, **kwargs)
    return wrapper

@login_required
def view_dashboard(user):
    return f"Welcome to dashboard, {user['name']}!"

# view_dashboard({"name": "Parsa", "is_authenticated": True})  # works
# view_dashboard({"name": "Guest"})  # raises PermissionError
```

### 2.3 — Generators and Iterators

Generators produce values lazily — one at a time — saving memory for large datasets.

```python
# === Generator Function ===
def fibonacci(limit):
    """Generate Fibonacci numbers up to limit."""
    a, b = 0, 1
    while a <= limit:
        yield a        # yield pauses and returns a value
        a, b = b, a + b

# Usage
for num in fibonacci(100):
    print(num, end=" ")
# 0 1 1 2 3 5 8 13 21 34 55 89

# Convert to list if needed
fib_list = list(fibonacci(100))

# === Generator Expression (like list comp but lazy) ===
# List comprehension — creates entire list in memory
big_list = [x**2 for x in range(10_000_000)]  # ~80MB of RAM

# Generator expression — produces one at a time
big_gen = (x**2 for x in range(10_000_000))   # ~0 RAM

# === Practical: Reading Large Files ===
def read_large_file(path):
    """Memory-efficient line-by-line reading."""
    with open(path) as f:
        for line in f:
            yield line.strip()

# Process a 10GB log file without loading it all:
# for line in read_large_file("huge.log"):
#     if "ERROR" in line:
#         print(line)

# === yield from — Delegating to Sub-generators ===
def flatten(nested_list):
    """Flatten arbitrarily nested lists."""
    for item in nested_list:
        if isinstance(item, list):
            yield from flatten(item)  # delegate to sub-generator
        else:
            yield item

print(list(flatten([1, [2, [3, 4], 5], [6, 7]])))
# [1, 2, 3, 4, 5, 6, 7]
```

### 2.4 — Context Managers

Context managers ensure setup and cleanup happen reliably (file closing, DB connections, etc.).

```python
# === Using 'with' statement (you've done this) ===
with open("file.txt") as f:
    content = f.read()
# File is automatically closed even if an error occurs

# === Writing Your Own Context Manager ===
class Timer:
    """Context manager that times a block of code."""
    
    def __enter__(self):
        import time
        self.start = time.perf_counter()
        print("Timer started")
        return self
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        import time
        self.elapsed = time.perf_counter() - self.start
        print(f"Elapsed: {self.elapsed:.4f}s")
        # Return False to propagate exceptions, True to suppress them
        return False

# Usage
with Timer() as t:
    sum(range(1_000_000))
# Timer started
# Elapsed: 0.0234s

# === Simpler: contextlib ===
from contextlib import contextmanager

@contextmanager
def managed_resource(name):
    print(f"Acquiring {name}")
    resource = {"name": name, "active": True}
    try:
        yield resource          # yield gives the value to 'as' variable
    finally:
        resource["active"] = False
        print(f"Releasing {name}")

with managed_resource("database") as res:
    print(f"Using {res['name']}")
# Acquiring database
# Using database
# Releasing database
```

### 2.5 — Type Hints (Modern Python)

```python
from typing import Optional, Union, Callable

# Basic type hints
def greet(name: str) -> str:
    return f"Hello, {name}"

# Collections
def process_items(items: list[str]) -> dict[str, int]:
    return {item: len(item) for item in items}

# Optional (can be None)
def find_user(user_id: int) -> Optional[str]:
    # Returns str or None
    return None

# Union (multiple types)
def format_value(value: Union[int, float, str]) -> str:
    return str(value)

# New in Python 3.10+: use | instead of Union/Optional
def find_user_v2(user_id: int) -> str | None:
    return None

# Type aliases
UserId = int
UserName = str

def get_user(uid: UserId) -> UserName:
    return "Parsa"

# Callable (functions as types)
def apply_twice(func: Callable[[int], int], value: int) -> int:
    return func(func(value))
```

### 2.6 — 📂 Project 2: Library Management System

**Objective:** Build a complete library management system using OOP with persistence, managing everything with Git branches.

**Features:**
- Book class with title, author, ISBN, genre
- Library class that manages a collection
- Borrow/return system with due dates
- Search by title, author, or genre
- Display overdue books
- Data persistence to JSON

**Expected interface:**
```python
# programmatic usage (not CLI yet — that's the exercise)
lib = Library("City Library")
lib.add_book(Book("The Hobbit", "Tolkien", "978-0547928227", "Fantasy"))
lib.add_book(Book("1984", "Orwell", "978-0451524935", "Dystopian"))

lib.borrow_book("978-0547928227", borrower="Parsa", days=14)
lib.search_by_author("Orwell")  # [Book("1984", ...)]
lib.overdue_books()              # books past due date
lib.return_book("978-0547928227")
```

**Hints:**
> HINT 1: Think about what classes you need and their relationships. A `Library` HAS books. A `Book` might have a `BorrowRecord`.
> 
> HINT 2: Use `datetime` module for due dates: `datetime.now() + timedelta(days=14)`
> 
> HINT 3: For persistence, create a `save()` and `load()` method on Library. Use `__dict__` or manual serialization.
> 
> HINT 4: For search, use list comprehensions with string `.lower().contains()` or `in`.

**Full Solution (read after attempting):**

>! **SPOILER: Full Solution**
> 
> ```python
> import json
> from datetime import datetime, timedelta
> from pathlib import Path
> from typing import Optional
> 
> 
> class Book:
>     def __init__(self, title: str, author: str, isbn: str, genre: str):
>         self.title = title
>         self.author = author
>         self.isbn = isbn
>         self.genre = genre
>     
>     def to_dict(self) -> dict:
>         return {
>             "title": self.title,
>             "author": self.author,
>             "isbn": self.isbn,
>             "genre": self.genre,
>         }
>     
>     @classmethod
>     def from_dict(cls, data: dict) -> "Book":
>         return cls(**data)
>     
>     def __repr__(self):
>         return f'Book("{self.title}" by {self.author})'
>     
>     def __eq__(self, other):
>         if not isinstance(other, Book):
>             return NotImplemented
>         return self.isbn == other.isbn
> 
> 
> class BorrowRecord:
>     def __init__(self, isbn: str, borrower: str, due_date: datetime):
>         self.isbn = isbn
>         self.borrower = borrower
>         self.due_date = due_date
>         self.borrowed_at = datetime.now()
>     
>     @property
>     def is_overdue(self) -> bool:
>         return datetime.now() > self.due_date
>     
>     def to_dict(self) -> dict:
>         return {
>             "isbn": self.isbn,
>             "borrower": self.borrower,
>             "due_date": self.due_date.isoformat(),
>             "borrowed_at": self.borrowed_at.isoformat(),
>         }
>     
>     @classmethod
>     def from_dict(cls, data: dict) -> "BorrowRecord":
>         data["due_date"] = datetime.fromisoformat(data["due_date"])
>         data["borrowed_at"] = datetime.fromisoformat(data["borrowed_at"])
>         record = cls(data["isbn"], data["borrower"], data["due_date"])
>         record.borrowed_at = data["borrowed_at"]
>         return record
> 
> 
> class Library:
>     def __init__(self, name: str, data_file: str = "library.json"):
>         self.name = name
>         self.data_file = Path(data_file)
>         self.books: list[Book] = []
>         self.borrowed: list[BorrowRecord] = []
>         self._load()
>     
>     def add_book(self, book: Book) -> None:
>         if any(b.isbn == book.isbn for b in self.books):
>             print(f"Book with ISBN {book.isbn} already exists.")
>             return
>         self.books.append(book)
>         self.save()
>         print(f"Added: {book}")
>     
>     def remove_book(self, isbn: str) -> None:
>         book = self._find_book(isbn)
>         if not book:
>             print(f"No book found with ISBN {isbn}")
>             return
>         if any(r.isbn == isbn for r in self.borrowed):
>             print("Cannot remove — book is currently borrowed.")
>             return
>         self.books.remove(book)
>         self.save()
>         print(f"Removed: {book}")
>     
>     def borrow_book(self, isbn: str, borrower: str, days: int = 14) -> None:
>         book = self._find_book(isbn)
>         if not book:
>             print(f"No book found with ISBN {isbn}")
>             return
>         if any(r.isbn == isbn for r in self.borrowed):
>             print(f"'{book.title}' is already borrowed.")
>             return
>         due = datetime.now() + timedelta(days=days)
>         record = BorrowRecord(isbn, borrower, due)
>         self.borrowed.append(record)
>         self.save()
>         print(f"'{book.title}' borrowed by {borrower}. Due: {due.strftime('%Y-%m-%d')}")
>     
>     def return_book(self, isbn: str) -> None:
>         record = next((r for r in self.borrowed if r.isbn == isbn), None)
>         if not record:
>             print("This book is not currently borrowed.")
>             return
>         self.borrowed.remove(record)
>         self.save()
>         print(f"Book returned. Thanks, {record.borrower}!")
>     
>     def search_by_title(self, query: str) -> list[Book]:
>         q = query.lower()
>         return [b for b in self.books if q in b.title.lower()]
>     
>     def search_by_author(self, query: str) -> list[Book]:
>         q = query.lower()
>         return [b for b in self.books if q in b.author.lower()]
>     
>     def search_by_genre(self, genre: str) -> list[Book]:
>         g = genre.lower()
>         return [b for b in self.books if b.genre.lower() == g]
>     
>     def overdue_books(self) -> list[BorrowRecord]:
>         return [r for r in self.borrowed if r.is_overdue]
>     
>     def all_books(self) -> list[Book]:
>         return self.books.copy()
>     
>     def _find_book(self, isbn: str) -> Optional[Book]:
>         return next((b for b in self.books if b.isbn == isbn), None)
>     
>     def save(self) -> None:
>         data = {
>             "name": self.name,
>             "books": [b.to_dict() for b in self.books],
>             "borrowed": [r.to_dict() for r in self.borrowed],
>         }
>         self.data_file.write_text(json.dumps(data, indent=2))
>     
>     def _load(self) -> None:
>         if not self.data_file.exists():
>             return
>         data = json.loads(self.data_file.read_text())
>         self.books = [Book.from_dict(b) for b in data.get("books", [])]
>         self.borrowed = [BorrowRecord.from_dict(r) for r in data.get("borrowed", [])]
>     
>     def __repr__(self):
>         return f"Library({self.name!r}, books={len(self.books)}, borrowed={len(self.borrowed)})"
> 
> 
> # Demo
> if __name__ == "__main__":
>     lib = Library("City Library")
>     lib.add_book(Book("The Hobbit", "J.R.R. Tolkien", "978-0547928227", "Fantasy"))
>     lib.add_book(Book("1984", "George Orwell", "978-0451524935", "Dystopian"))
>     lib.add_book(Book("Dune", "Frank Herbert", "978-0441013593", "Sci-Fi"))
>     
>     lib.borrow_book("978-0547928227", "Parsa", days=14)
>     print(lib.search_by_author("orwell"))
>     print(lib.overdue_books())
>     print(lib)
> ```

**Git checkpoints:**
```bash
# Create a feature branch for this project
git checkout -b feature/library-system

# After Book class:
git commit -am "feat: add Book class with serialization"

# After BorrowRecord:
git commit -am "feat: add BorrowRecord with due date logic"

# After Library class:
git commit -am "feat: add Library class with CRUD and persistence"

# After testing and polish:
git checkout main
git merge feature/library-system
```

### 2.7 — 📂 Project 3: Contact Book with Rich CLI

**Objective:** Build a contact book with search, grouping, and a polished terminal UI.

**Features:**
- Add/edit/delete contacts (name, phone, email, group)
- Search contacts (fuzzy matching)
- Group contacts (family, work, friends)
- Export contacts to CSV
- Rich terminal output (tables, colored text)

**Install Rich library:** `pip install rich` (we'll use it for pretty output)

**Hints:**
> HINT 1: Use `from rich.console import Console` and `from rich.table import Table` for beautiful output.
> 
> HINT 2: For fuzzy search, check if the query is a substring of any field (case-insensitive). For better matching, you could use `difflib.get_close_matches()`.
> 
> HINT 3: Use inheritance: `Contact` base class, then `PersonalContact`, `WorkContact` with extra fields.

**Git checkpoints:**
```bash
git checkout -b feature/contact-book
# ... build and commit incrementally ...
git checkout main && git merge feature/contact-book
```

### 2.8 — Virtual Environments (Essential for Every Project)

```bash
# Create a virtual environment
python3 -m venv .venv

# Activate it
source .venv/bin/activate    # Linux/Mac
# .venv\Scripts\activate     # Windows

# Now pip installs go into .venv, not system-wide
pip install rich
pip freeze > requirements.txt   # save dependencies

# Deactivate when done
deactivate

# Recreate environment from requirements.txt
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

**Always add to .gitignore:**
```
.venv/
venv/
```

**Always commit `requirements.txt`:**
```bash
git add requirements.txt
git commit -m "Add requirements.txt"
```

---

## Phase 3: Consuming HTTP APIs

> **Duration:** 5–7 days  
> **Goal:** Understand HTTP, call external APIs, handle authentication and pagination  
> **Projects:** Weather Dashboard CLI, Movie Search App  
> **Git:** Remote repos, push/pull workflow

### 3.1 — HTTP Fundamentals

Every web interaction uses HTTP. Understanding it makes API work trivial.

#### The HTTP Request-Response Cycle

```
Your Python Script                    External Server
      |                                      |
      |──── HTTP Request ──────────────────>|
      |     GET /api/weather?city=tehran     |
      |                                      |
      |<─── HTTP Response ─────────────────|
      |     Status: 200 OK                  |
      |     Body: {"temp": 32, ...}         |
```

#### HTTP Methods (Verbs)

| Method | Purpose | Has Body? | Idempotent? | Example |
|--------|---------|-----------|-------------|---------|
| `GET` | Read data | No | Yes | Fetch weather |
| `POST` | Create data | Yes | No | Submit form |
| `PUT` | Replace data | Yes | Yes | Update profile |
| `PATCH` | Partial update | Yes | Yes | Change email |
| `DELETE` | Remove data | Yes | Yes | Delete post |

**Idempotent** = calling it twice gives the same result as calling once.

#### Status Codes

```
1xx — Informational (rare in APIs)
2xx — Success
  200 OK              — Standard success
  201 Created         — Resource created (after POST)
  204 No Content      — Success, but no body (after DELETE)

3xx — Redirection (automatic in most libraries)
  301 Moved Permanently

4xx — Client Error (your fault)
  400 Bad Request     — Malformed request (missing params)
  401 Unauthorized    — Need to authenticate
  403 Forbidden       — Authenticated but not allowed
  404 Not Found       — Resource doesn't exist
  429 Too Many Requests — Rate limited

5xx — Server Error (their fault)
  500 Internal Server Error
  503 Service Unavailable
```

#### Headers

Headers carry metadata about the request/response:

```
Content-Type: application/json     # What format is the body?
Authorization: Bearer abc123       # Authentication token
Accept: application/json           # What format I want back
User-Agent: MyApp/1.0             # Who is making the request
X-API-Key: your-key-here          # API key (some APIs use this)
```

#### URLs and Query Parameters

```
https://api.example.com/v1/weather?city=tehran&units=metric
\_________/\____________/\____/\________/\________________/
   host        path      ver    endpoint    query params
```

```python
# Building URLs with requests:
import requests

params = {"city": "tehran", "units": "metric", "lang": "fa"}
response = requests.get("https://api.example.com/v1/weather", params=params)
# Automatically encodes: ...?city=tehran&units=metric&lang=fa
print(response.url)
```

### 3.2 — The `requests` Library

The de-facto HTTP library for Python. Install it: `pip install requests`

```python
import requests

# === Basic GET Request ===
response = requests.get("https://httpbin.org/get")

# Response object properties:
print(response.status_code)   # 200
print(response.headers)       # dict of response headers
print(response.text)          # response body as string
print(response.json())        # parsed JSON → dict/list
print(response.encoding)      # 'utf-8'
print(response.ok)            # True if status < 400
print(response.elapsed)       # timedelta of request duration

# === GET with Parameters ===
response = requests.get(
    "https://httpbin.org/get",
    params={"key": "value", "name": "parsa"}
)
# Sends: https://httpbin.org/get?key=value&name=parsa

# === POST with JSON Body ===
data = {"username": "parsa", "email": "parsa@example.com"}
response = requests.post(
    "https://httpbin.org/post",
    json=data  # automatically sets Content-Type: application/json
)
print(response.json()["json"])  # {'username': 'parsa', 'email': 'parsa@example.com'}

# === Custom Headers ===
headers = {
    "Authorization": "Bearer YOUR_API_KEY",
    "Accept": "application/json",
    "X-Custom-Header": "value"
}
response = requests.get("https://api.example.com/data", headers=headers)

# === Error Handling — ALWAYS do this ===
try:
    response = requests.get("https://api.example.com/data", timeout=10)
    response.raise_for_status()  # raises HTTPError for 4xx/5xx
    data = response.json()
except requests.exceptions.Timeout:
    print("Request timed out")
except requests.exceptions.ConnectionError:
    print("Connection failed — check your internet")
except requests.exceptions.HTTPError as e:
    print(f"HTTP error: {e.response.status_code} — {e.response.text}")
except requests.exceptions.JSONDecodeError:
    print("Response was not valid JSON")
else:
    print("Success!", data)
finally:
    print("Request completed")

# === Session (for multiple requests to same API) ===
session = requests.Session()
session.headers.update({"Authorization": "Bearer token123"})
# All requests through this session send the auth header automatically
r1 = session.get("https://api.example.com/users")
r2 = session.get("https://api.example.com/posts")
# Session also persists cookies (useful for login flows)
```

### 3.3 — Working with JSON APIs

```python
import requests
from datetime import datetime

class APIClient:
    """Reusable base for API interactions."""
    
    def __init__(self, base_url: str, api_key: str = None):
        self.base_url = base_url.rstrip("/")
        self.session = requests.Session()
        if api_key:
            self.session.headers["Authorization"] = f"Bearer {api_key}"
        self.session.headers["Accept"] = "application/json"
    
    def _request(self, method: str, endpoint: str, **kwargs) -> dict:
        url = f"{self.base_url}/{endpoint.lstrip('/')}"
        try:
            response = self.session.request(method, url, timeout=10, **kwargs)
            response.raise_for_status()
            return response.json()
        except requests.exceptions.RequestException as e:
            print(f"API Error: {e}")
            raise
    
    def get(self, endpoint: str, **kwargs) -> dict:
        return self._request("GET", endpoint, **kwargs)
    
    def post(self, endpoint: str, **kwargs) -> dict:
        return self._request("POST", endpoint, **kwargs)
```

### 3.4 — API Authentication Patterns

```python
# 1. API Key (simplest — in header or query param)
headers = {"X-API-Key": "your-key"}
# OR
params = {"api_key": "your-key"}

# 2. Bearer Token (OAuth 2.0 style)
headers = {"Authorization": "Bearer eyJhbGciOiJIUzI1..."}

# 3. Basic Auth (username:password)
response = requests.get(url, auth=("username", "password"))
# Sends: Authorization: Basic base64(username:password)

# 4. OAuth 2.0 (complex flow — simplified)
# Step 1: Get access token
token_response = requests.post("https://auth.example.com/token", data={
    "grant_type": "client_credentials",
    "client_id": "your_client_id",
    "client_secret": "your_client_secret"
})
access_token = token_response.json()["access_token"]

# Step 2: Use the token
headers = {"Authorization": f"Bearer {access_token}"}
response = requests.get("https://api.example.com/data", headers=headers)
```

### 3.5 — Rate Limiting and Pagination

```python
import time

# === Rate Limiting ===
class RateLimiter:
    def __init__(self, max_requests: int, per_seconds: float):
        self.max_requests = max_requests
        self.per_seconds = per_seconds
        self.timestamps = []
    
    def wait_if_needed(self):
        now = time.time()
        # Remove timestamps older than window
        self.timestamps = [t for t in self.timestamps if now - t < self.per_seconds]
        if len(self.timestamps) >= self.max_requests:
            sleep_time = self.per_seconds - (now - self.timestamps[0])
            print(f"Rate limited. Sleeping {sleep_time:.1f}s...")
            time.sleep(sleep_time)
        self.timestamps.append(time.time())

# Usage
limiter = RateLimiter(max_requests=5, per_seconds=60)
for page in range(10):
    limiter.wait_if_needed()
    data = requests.get(f"https://api.example.com/items?page={page}").json()

# === Pagination ===
def fetch_all_pages(base_url: str, params: dict = None) -> list:
    """Fetch all pages of a paginated API."""
    params = params or {}
    params.setdefault("page", 1)
    params.setdefault("per_page", 100)
    all_items = []
    
    while True:
        response = requests.get(base_url, params=params)
        response.raise_for_status()
        data = response.json()
        
        items = data.get("results", data.get("items", []))
        all_items.extend(items)
        
        # Check if there are more pages
        if data.get("has_next", False) or data.get("next"):
            params["page"] += 1
        elif isinstance(data.get("next"), str) and data["next"]:
            # Some APIs return the full next URL
            # Parse page from URL or use the next URL directly
            response_next = requests.get(data["next"])
            data = response_next.json()
            items = data.get("results", data.get("items", []))
            all_items.extend(items)
        else:
            break
    
    return all_items
```

### 3.6 — 📂 Project 4: Weather Dashboard CLI

**Objective:** Build a CLI that fetches weather from OpenWeatherMap API and displays it beautifully.

**Get a free API key:** Sign up at https://openweathermap.org/api (free tier: 60 calls/min)

**Features:**
- Current weather for any city
- 5-day forecast
- Display temperature, humidity, wind, conditions
- Colored output (blue for cold, red for hot)
- Save favorite cities
- Cache responses (don't re-fetch within 10 min)

**Expected interface:**
```bash
python weather.py current tehran
python weather.py forecast tehran
python weather.py favorites add tehran
python weather.py favorites list
```

**Example output:**
```
🌡  Tehran — Currently
━━━━━━━━━━━━━━━━━━━━━
  Condition:  ☁️  Overcast
  Temp:       32°C (feels like 34°C)
  Humidity:   45%
  Wind:       12 km/h NW
  Updated:    2 min ago
```

**Hints:**
> HINT 1: OpenWeatherMap API endpoint: `https://api.openweathermap.org/data/2.5/weather?q={city}&appid={key}&units=metric`
> 
> HINT 2: For 5-day forecast use: `/data/2.5/forecast?q={city}&appid={key}&units=metric` — it returns 3-hour intervals, so filter for once-per-day.
> 
> HINT 3: For caching, save responses to a JSON file with a timestamp. Before fetching, check if the cache is still fresh.
> 
> HINT 4: Use `rich` for colored output. `from rich.console import Console` and conditionally set styles based on temperature.

**Full Solution (read after attempting):**

>! **SPOILER: Full Solution**
> 
> ```python
> #!/usr/bin/env python3
> """Weather Dashboard CLI — fetches weather from OpenWeatherMap."""
> 
> import argparse
> import json
> import sys
> import requests
> from datetime import datetime, timedelta
> from pathlib import Path
> from rich.console import Console
> from rich.table import Table
> from rich.panel import Panel
> from rich import box
> 
> # ── Configuration ──
> API_KEY = None  # Set via env var or config file
> CACHE_FILE = Path("weather_cache.json")
> FAVORITES_FILE = Path("favorites.json")
> CACHE_MAX_AGE = timedelta(minutes=10)
> console = Console()
> 
> 
> def load_api_key() -> str:
>     global API_KEY
>     if API_KEY:
>         return API_KEY
>     
>     # Try environment variable
>     import os
>     key = os.environ.get("OPENWEATHER_API_KEY")
>     if key:
>         API_KEY = key
>         return key
>     
>     # Try config file
>     config = Path("config.json")
>     if config.exists():
>         data = json.loads(config.read_text())
>         key = data.get("openweather_api_key")
>         if key:
>             API_KEY = key
>             return key
>     
>     console.print("[red]Error:[/] No API key found.")
>     console.print("Set OPENWEATHER_API_KEY env var or add to config.json")
>     sys.exit(1)
> 
> 
> def get_cache(city: str) -> dict | None:
>     if not CACHE_FILE.exists():
>         return None
>     cache = json.loads(CACHE_FILE.read_text())
>     entry = cache.get(city.lower())
>     if not entry:
>         return None
>     cached_at = datetime.fromisoformat(entry["timestamp"])
>     if datetime.now() - cached_at > CACHE_MAX_AGE:
>         return None  # expired
>     return entry["data"]
> 
> 
> def set_cache(city: str, data: dict) -> None:
>     cache = {}
>     if CACHE_FILE.exists():
>         cache = json.loads(CACHE_FILE.read_text())
>     cache[city.lower()] = {
>         "data": data,
>         "timestamp": datetime.now().isoformat()
>     }
>     CACHE_FILE.write_text(json.dumps(cache, indent=2))
> 
> 
> def fetch_current(city: str) -> dict:
>     cached = get_cache(city)
>     if cached:
>         return cached
>     
>     api_key = load_api_key()
>     url = "https://api.openweathermap.org/data/2.5/weather"
>     resp = requests.get(url, params={
>         "q": city, "appid": api_key, "units": "metric"
>     }, timeout=10)
>     resp.raise_for_status()
>     data = resp.json()
>     set_cache(city, data)
>     return data
> 
> 
> def fetch_forecast(city: str) -> list:
>     api_key = load_api_key()
>     url = "https://api.openweathermap.org/data/2.5/forecast"
>     resp = requests.get(url, params={
>         "q": city, "appid": api_key, "units": "metric"
>     }, timeout=10)
>     resp.raise_for_status()
>     data = resp.json()
>     
>     # Group by day, take one reading per day
>     seen_days = set()
>     daily = []
>     for item in data.get("list", []):
>         dt = datetime.fromtimestamp(item["dt"])
>         day = dt.date()
>         if day not in seen_days and dt.hour >= 12:  # pick afternoon reading
>             seen_days.add(day)
>             daily.append(item)
>     return daily[:5]
> 
> 
> def temp_color(temp: float) -> str:
>     if temp < 0:
>         return "blue"
>     elif temp < 15:
>         return "cyan"
>     elif temp < 25:
>         return "green"
>     elif temp < 35:
>         return "yellow"
>     else:
>         return "red"
> 
> 
> def display_current(data: dict) -> None:
>     city = data["name"]
>     country = data["sys"]["country"]
>     temp = data["main"]["temp"]
>     feels = data["main"]["feels_like"]
>     humidity = data["main"]["humidity"]
>     desc = data["weather"][0]["description"].title()
>     icon = data["weather"][0]["icon"]
>     wind = data["wind"]["speed"] * 3.6  # m/s to km/h
>     wind_dir = deg_to_compass(data["wind"].get("deg", 0))
>     
>     color = temp_color(temp)
>     panel_content = f"""[bold]{desc}[/]
>   Temp:       [{color}]{temp}°C[/] (feels like {feels}°C)
>   Humidity:   {humidity}%
>   Wind:       {wind:.0f} km/h {wind_dir}
>   Updated:    {datetime.now().strftime('%H:%M')}"""
>     
>     console.print(Panel(
>         panel_content,
>         title=f"[bold]🌍 {city}, {country}[/]",
>         box=box.ROUNDED
>     ))
> 
> 
> def deg_to_compass(deg: float) -> str:
>     directions = ["N", "NE", "E", "SE", "S", "SW", "W", "NW"]
>     idx = round(deg / 45) % 8
>     return directions[idx]
> 
> 
> def display_forecast(city: str, items: list) -> None:
>     table = Table(title=f"📅 5-Day Forecast — {city}", box=box.ROUNDED)
>     table.add_column("Date", style="bold")
>     table.add_column("Condition")
>     table.add_column("High", justify="right")
>     table.add_column("Low", justify="right")
>     table.add_column("Humidity", justify="right")
>     
>     for item in items:
>         dt = datetime.fromtimestamp(item["dt"])
>         temp_max = item["main"]["temp_max"]
>         temp_min = item["main"]["temp_min"]
>         desc = item["weather"][0]["description"].title()
>         humidity = item["main"]["humidity"]
>         
>         color = temp_color((temp_max + temp_min) / 2)
>         table.add_row(
>             dt.strftime("%a %d"),
>             desc,
>             f"[{color}]{temp_max:.0f}°C[/]",
>             f"{temp_min:.0f}°C",
>             f"{humidity}%"
>         )
>     
>     console.print(table)
> 
> 
> def load_favorites() -> list:
>     if FAVORITES_FILE.exists():
>         return json.loads(FAVORITES_FILE.read_text())
>     return []
> 
> 
> def save_favorites(favs: list) -> None:
>     FAVORITES_FILE.write_text(json.dumps(favs, indent=2))
> 
> 
> def main():
>     parser = argparse.ArgumentParser(description="Weather Dashboard")
>     sub = parser.add_subparsers(dest="command")
>     
>     # current
>     cur = sub.add_parser("current", help="Current weather")
>     cur.add_argument("city", help="City name")
>     
>     # forecast
>     fc = sub.add_parser("forecast", help="5-day forecast")
>     fc.add_argument("city", help="City name")
>     
>     # favorites
>     fav = sub.add_parser("favorites", help="Manage favorites")
>     fav_sub = fav.add_subparsers(dest="action")
>     fav_sub.add_parser("list", help="List favorites")
>     add = fav_sub.add_parser("add", help="Add city")
>     add.add_argument("city")
>     rm = fav_sub.add_parser("remove", help="Remove city")
>     rm.add_argument("city")
>     
>     args = parser.parse_args()
>     
>     if args.command == "current":
>         data = fetch_current(args.city)
>         display_current(data)
>     elif args.command == "forecast":
>         items = fetch_forecast(args.city)
>         display_forecast(args.city, items)
>     elif args.command == "favorites":
>         favs = load_favorites()
>         if args.action == "list":
>             if favs:
>                 console.print("Favorites: " + ", ".join(favs))
>             else:
>                 console.print("No favorites yet.")
>         elif args.action == "add":
>             if args.city.lower() not in [f.lower() for f in favs]:
>                 favs.append(args.city)
>                 save_favorites(favs)
>             console.print(f"Added {args.city}")
>         elif args.action == "remove":
>             favs = [f for f in favs if f.lower() != args.city.lower()]
>             save_favorites(favs)
>             console.print(f"Removed {args.city}")
>     else:
>         parser.print_help()
> 
> 
> if __name__ == "__main__":
>     main()
> ```

### 3.7 — 📂 Project 5: Movie Search CLI

**Objective:** Search movies using The Movie Database (TMDB) API.

**Get API key:** https://www.themoviedb.org/settings/api (free)

**Features:**
- Search movies by title
- View movie details (rating, cast, synopsis)
- Trending movies
- Compare two movies side by side
- Save watchlist to file

**API endpoints:**
```
GET /3/search/movie?query=...&api_key=...
GET /3/movie/{id}?api_key=...
GET /3/trending/movie/week?api_key=...
GET /3/movie/{id}/credits?api_key=...    # cast & crew
```

**Hints:**
> HINT 1: TMDB returns many fields per movie — focus on: title, release_date, vote_average, overview, poster_path.
> 
> HINT 2: Poster images can be viewed at: `https://image.tmdb.org/t/p/w500{poster_path}`
> 
> HINT 3: For "compare", fetch both movies and display them in a side-by-side Rich table.

**Git workflow for this project:**
```bash
# Create a dedicated repo for API projects
mkdir api-projects && cd api-projects
git init
echo ".env" >> .gitignore   # Never commit API keys!
echo "config.json" >> .gitignore
git commit -m "Init: api-projects workspace"

# Weather dashboard
git checkout -b feature/weather-dashboard
# ... build ...
git checkout main && git merge feature/weather-dashboard

# Movie search
git checkout -b feature/movie-search
# ... build ...
git checkout main && git merge feature/movie-search
```

### 3.8 — Pushing to GitHub

Now that you have projects, let's share them:

```bash
# 1. Create a new repo on GitHub (web UI — name: api-projects, private)

# 2. Connect and push
git remote add origin https://github.com/YOUR_USERNAME/api-projects.git
git push -u origin main

# 3. Push feature branches too
git push -u origin feature/weather-dashboard
git push -u origin feature/movie-search

# 4. After merging, clean up remote branches
git push origin --delete feature/weather-dashboard
```

---

## Phase 4: Building APIs (Flask & FastAPI)

> **Duration:** 7–10 days  
> **Goal:** Build your own REST APIs with authentication, validation, and proper error handling  
> **Projects:** Todo API, Blog API with auth  
> **Git:** Feature branches, pull requests workflow

### 4.1 — Flask: The Lightweight Framework

Flask is a micro-framework — it gives you the essentials and lets you add what you need. Perfect for learning API fundamentals.

```bash
pip install flask
```

#### Your First Flask App

```python
from flask import Flask, jsonify, request, abort

app = Flask(__name__)

# In-memory data store (we'll add databases in Phase 5)
todos = []
next_id = 1

@app.route("/")
def index():
    return "Todo API is running!"

@app.route("/api/todos", methods=["GET"])
def list_todos():
    return jsonify(todos)

@app.route("/api/todos", methods=["POST"])
def create_todo():
    global next_id
    data = request.get_json()          # parse JSON body
    if not data or "title" not in data:
        abort(400, description="Title is required")
    
    todo = {
        "id": next_id,
        "title": data["title"],
        "done": False,
        "priority": data.get("priority", "medium")  # optional with default
    }
    todos.append(todo)
    next_id += 1
    return jsonify(todo), 201          # 201 = Created

@app.route("/api/todos/<int:todo_id>", methods=["GET"])
def get_todo(todo_id):
    todo = next((t for t in todos if t["id"] == todo_id), None)
    if not todo:
        abort(404, description="Todo not found")
    return jsonify(todo)

@app.route("/api/todos/<int:todo_id>", methods=["PATCH"])
def update_todo(todo_id):
    todo = next((t for t in todos if t["id"] == todo_id), None)
    if not todo:
        abort(404)
    data = request.get_json()
    if "title" in data:
        todo["title"] = data["title"]
    if "done" in data:
        todo["done"] = data["done"]
    if "priority" in data:
        todo["priority"] = data["priority"]
    return jsonify(todo)

@app.route("/api/todos/<int:todo_id>", methods=["DELETE"])
def delete_todo(todo_id):
    global todos
    before = len(todos)
    todos = [t for t in todos if t["id"] != todo_id]
    if len(todos) == before:
        abort(404)
    return "", 204                   # 204 = No Content (success, empty body)

# Error handler — returns JSON instead of HTML
@app.errorhandler(400)
@app.errorhandler(404)
def error_handler(e):
    return jsonify({"error": str(e.description)}), e.code

if __name__ == "__main__":
    app.run(debug=True, port=5000)
```

**Run it:** `python app.py` → serves at `http://localhost:5000`

**Test with curl:**
```bash
# List todos
curl http://localhost:5000/api/todos

# Create a todo
curl -X POST http://localhost:5000/api/todos \
  -H "Content-Type: application/json" \
  -d '{"title": "Learn Flask", "priority": "high"}'

# Get one todo
curl http://localhost:5000/api/todos/1

# Update
curl -X PATCH http://localhost:5000/api/todos/1 \
  -H "Content-Type: application/json" \
  -d '{"done": true}'

# Delete
curl -X DELETE http://localhost:5000/api/todos/1
```

#### Flask Blueprints (Organizing Large Apps)

```python
# app.py — main entry
from flask import Flask
from routes.todos import todos_bp
from routes.users import users_bp

app = Flask(__name__)
app.register_blueprint(todos_bp, url_prefix="/api/todos")
app.register_blueprint(users_bp, url_prefix="/api/users")
```

```python
# routes/todos.py — separate file for todo routes
from flask import Blueprint, jsonify, request, abort

todos_bp = Blueprint("todos", __name__)

todos = []
next_id = 1

@todos_bp.route("", methods=["GET"])    # no leading / — prefix handles it
def list_todos():
    return jsonify(todos)

@todos_bp.route("", methods=["POST"])
def create_todo():
    global next_id
    data = request.get_json()
    todo = {"id": next_id, "title": data["title"], "done": False}
    todos.append(todo)
    next_id += 1
    return jsonify(todo), 201
```

### 4.2 — FastAPI: Modern Python APIs

FastAPI is newer, faster, and has automatic API documentation. It uses **type hints** for validation — you write the code, you get validation for free.

```bash
pip install fastapi uvicorn
```

```python
# main.py
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel, Field
from typing import Optional
from datetime import datetime

app = FastAPI(title="Todo API", version="1.0.0")

# ── Data Models (Pydantic) ──
class TodoCreate(BaseModel):
    title: str = Field(..., min_length=1, max_length=200)
    priority: str = Field(default="medium", pattern="^(low|medium|high)$")

class TodoUpdate(BaseModel):
    title: Optional[str] = Field(None, min_length=1, max_length=200)
    done: Optional[bool] = None
    priority: Optional[str] = Field(None, pattern="^(low|medium|high)$")

class TodoResponse(BaseModel):
    id: int
    title: str
    done: bool
    priority: str
    created_at: str

# ── In-memory store ──
todos: list[dict] = []
next_id = 1

# ── Routes ──
@app.get("/api/todos", response_model=list[TodoResponse])
def list_todos(done: Optional[bool] = None):
    """List all todos, optionally filtered by status."""
    if done is not None:
        return [t for t in todos if t["done"] == done]
    return todos

@app.post("/api/todos", response_model=TodoResponse, status_code=201)
def create_todo(todo: TodoCreate):
    """Create a new todo."""
    global next_id
    new_todo = {
        "id": next_id,
        "title": todo.title,
        "done": False,
        "priority": todo.priority,
        "created_at": datetime.now().isoformat()
    }
    todos.append(new_todo)
    next_id += 1
    return new_todo

@app.get("/api/todos/{todo_id}", response_model=TodoResponse)
def get_todo(todo_id: int):
    """Get a single todo by ID."""
    todo = next((t for t in todos if t["id"] == todo_id), None)
    if not todo:
        raise HTTPException(status_code=404, detail="Todo not found")
    return todo

@app.patch("/api/todos/{todo_id}", response_model=TodoResponse)
def update_todo(todo_id: int, update: TodoUpdate):
    """Partially update a todo."""
    todo = next((t for t in todos if t["id"] == todo_id), None)
    if not todo:
        raise HTTPException(status_code=404, detail="Todo not found")
    
    # Only update fields that were provided
    update_data = update.model_dump(exclude_unset=True)
    todo.update(update_data)
    return todo

@app.delete("/api/todos/{todo_id}", status_code=204)
def delete_todo(todo_id: int):
    """Delete a todo."""
    global todos
    before = len(todos)
    todos = [t for t in todos if t["id"] != todo_id]
    if len(todos) == before:
        raise HTTPException(status_code=404, detail="Todo not found")
```

**Run it:** `uvicorn main:app --reload` → serves at `http://localhost:8000`

**Auto-generated docs:** Visit `http://localhost:8000/docs` — FastAPI creates interactive Swagger documentation from your code!

**Key differences from Flask:**
- Type hints = automatic validation (bad data gets 422 errors automatically)
- `pydantic` models define request/response schemas
- `status_code=201` / `status_code=204` set proper HTTP codes
- Async support built in (we'll use this later)
- Auto-generates API docs

### 4.3 — RESTful API Design Principles

```python
# REST resource naming — use nouns, not verbs
# ✅ GET /api/users          — list users
# ✅ GET /api/users/42       — get user 42
# ✅ POST /api/users         — create user
# ✅ PUT /api/users/42       — replace user 42
# ✅ PATCH /api/users/42     — update user 42 partially
# ✅ DELETE /api/users/42    — delete user 42
# ❌ GET /api/getUser        — not RESTful
# ❌ POST /api/createUser    — not RESTful

# Nested resources
# ✅ GET /api/users/42/todos         — todos belonging to user 42
# ✅ POST /api/users/42/todos        — create a todo for user 42

# Filtering, sorting, pagination via query params
# ✅ GET /api/todos?priority=high&done=false
# ✅ GET /api/todos?sort=created_at&order=desc
# ✅ GET /api/todos?page=2&per_page=20

# Response format (consistent across all endpoints):
{
    "data": [...],              # the actual payload
    "meta": {                   # pagination info
        "total": 150,
        "page": 2,
        "per_page": 20,
        "has_next": true
    }
}
```

### 4.4 — Pydantic Validation Deep Dive

```python
from pydantic import BaseModel, Field, field_validator
from typing import Optional
from datetime import datetime

class UserCreate(BaseModel):
    username: str = Field(..., min_length=3, max_length=30, pattern=r"^[a-zA-Z0-9_]+$")
    email: str = Field(..., pattern=r"^[\w.-]+@[\w.-]+\.\w+$")
    age: int = Field(..., ge=0, le=150)
    bio: Optional[str] = Field(None, max_length=500)
    
    @field_validator("username")
    @classmethod
    def username_alphanumeric(cls, v):
        if not v.isalnum():
            raise ValueError("Username must be alphanumeric")
        return v.lower()  # normalize to lowercase

# FastAPI automatically validates incoming JSON against this model
# Invalid data → 422 Unprocessable Entity with detailed error messages

# Example error response for bad input:
# {
#     "detail": [
#         {
#             "type": "string_pattern_mismatch",
#             "loc": ["body", "username"],
#             "msg": "String should match pattern '^[a-zA-Z0-9_]+$'",
#             "input": "parsa!@#"
#         }
#     ]
# }
```

### 4.5 — Authentication for Your API

```python
from fastapi import Depends, HTTPException, status
from fastapi.security import HTTPBearer, HTTPAuthorizationCredentials
from datetime import datetime, timedelta
import jwt  # pip install pyjwt

SECRET_KEY = "your-secret-key-change-in-production"
security = HTTPBearer()

# ── Token Generation ──
def create_token(user_id: int, expires_hours: int = 24) -> str:
    payload = {
        "user_id": user_id,
        "exp": datetime.utcnow() + timedelta(hours=expires_hours),
        "iat": datetime.utcnow()
    }
    return jwt.encode(payload, SECRET_KEY, algorithm="HS256")

# ── Token Verification ──
def verify_token(credentials: HTTPAuthorizationCredentials = Depends(security)) -> dict:
    token = credentials.credentials
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=["HS256"])
        return payload
    except jwt.ExpiredSignatureError:
        raise HTTPException(status_code=401, detail="Token expired")
    except jwt.InvalidTokenError:
        raise HTTPException(status_code=401, detail="Invalid token")

# ── Using it in routes ──
@app.get("/api/protected")
def protected_route(user: dict = Depends(verify_token)):
    return {"message": f"Hello user {user['user_id']}!"}

# ── Login endpoint ──
@app.post("/api/login")
def login(username: str, password: str):
    # In real app: look up user, verify password hash
    user = authenticate_user(username, password)
    if not user:
        raise HTTPException(status_code=401, detail="Invalid credentials")
    token = create_token(user["id"])
    return {"access_token": token, "token_type": "bearer"}
```

**Using the token in requests:**
```bash
# Get a token first
curl -X POST http://localhost:8000/api/login \
  -d "username=parsa&password=secret"

# Use it
curl http://localhost:8000/api/protected \
  -H "Authorization: Bearer ***"
```

### 4.6 — 📂 Project 6: Todo REST API

**Objective:** Build a full Todo API using FastAPI with validation, filtering, and error handling.

**Features:**
- Full CRUD (Create, Read, Update, Delete)
- Filter by status (done/not done) and priority
- Sort by created_at or priority
- Pagination (page, per_page)
- Consistent error responses
- Input validation via Pydantic

**Build it using FastAPI.** Store todos in-memory for now (Phase 5 adds databases).

**Expected endpoints:**
```
GET    /api/todos?done=false&priority=high&sort=created_at&page=1
POST   /api/todos
GET    /api/todos/{id}
PATCH  /api/todos/{id}
DELETE /api/todos/{id}
```

**Hints:**
> HINT 1: Use `app.include_router()` to keep routes organized in separate files.
> 
> HINT 2: For pagination, accept `page: int = 1, per_page: int = 20` as query params. Slice your list: `todos[(page-1)*per_page : page*per_page]`.
> 
> HINT 3: Return a wrapper dict: `{"data": items, "meta": {"total": N, "page": page, ...}}`

**Git checkpoints:**
```bash
git checkout -b feature/todo-api
git commit -am "feat: scaffold FastAPI todo API"
git commit -am "feat: add filtering and pagination"
git checkout main && git merge feature/todo-api
```

### 4.7 — 📂 Project 7: Blog API with Authentication

**Objective:** Build a blog API where users sign up, log in, and manage their own posts.

**Features:**
- User registration and login (JWT tokens)
- Create/read/update/delete blog posts
- Each user can only edit/delete their own posts
- Posts have title, body, tags, created_at, updated_at
- Public: anyone can read posts; only author can edit

**Expected endpoints:**
```
POST   /api/register         — {username, email, password}
POST   /api/login            — {username, password} → {token}
GET    /api/posts            — public, list all posts
GET    /api/posts/{id}       — public, single post
POST   /api/posts            — auth required, create post
PATCH  /api/posts/{id}       — auth required, owner only
DELETE /api/posts/{id}       — auth required, owner only
```

**Hints:**
> HINT 1: For password hashing, use `passlib`: `pip install passlib[bcrypt]`
> ```python
> from passlib.context import CryptContext
> pwd_ctx = CryptContext(schemes=["bcrypt"], deprecated="auto")
> hashed = pwd_ctx.hash("password123")
> pwd_ctx.verify("password123", hashed)  # True
> ```
> 
> HINT 2: Use `Depends()` for auth — create an `get_current_user` dependency that extracts and validates the JWT.
> 
> HINT 3: Store users and posts in dicts keyed by ID for O(1) lookup.

---

## Phase 5: Databases & ORM

> **Duration:** 5–7 days  
> **Goal:** Store data persistently with SQLite and PostgreSQL using SQLAlchemy  
> **Projects:** Bookshelf API, Inventory Management System  
> **Git:** .gitignore for DB files, migration commits

### 5.1 — Why Databases?

JSON files work for prototypes but break down: no concurrent access, no queries, no integrity constraints. Databases solve all of this.

**SQL vs NoSQL (you need SQL):**
- **SQL** (SQLite, PostgreSQL, MySQL): Structured tables with relationships. Best for 90% of apps.
- **NoSQL** (MongoDB, Redis): Flexible schemas, good for caching or specific use cases.

### 5.2 — SQLite (Zero Setup Database)

SQLite is a single file database — no server needed. Perfect for learning and small apps.

```python
import sqlite3

# Connect (creates file if it doesn't exist)
conn = sqlite3.connect("app.db")
conn.row_factory = sqlite3.Row  # Access columns by name
db = conn.cursor()

# Create table
db.execute("""
    CREATE TABLE IF NOT EXISTS users (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        username TEXT UNIQUE NOT NULL,
        email TEXT UNIQUE NOT NULL,
        created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
    )
""")

# Insert
db.execute("INSERT INTO users (username, email) VALUES (?, ?)", ("parsa", "p@e.com"))
conn.commit()  # MUST call commit or changes are lost!

# Query
db.execute("SELECT * FROM users WHERE username = ?", ("parsa",))
user = db.fetchone()
print(dict(user))  # {'id': 1, 'username': 'parsa', 'email': 'p@e.com', ...}

# Query all
db.execute("SELECT * FROM users")
users = db.fetchall()

# Update
db.execute("UPDATE users SET email = ? WHERE id = ?", ("new@email.com", 1))
conn.commit()

# Delete
db.execute("DELETE FROM users WHERE id = ?", (1,))
conn.commit()

conn.close()
```

**Key rules:**
- Always use `?` placeholders — NEVER string formatting (`f"WHERE id = {id}"` is an **SQL injection vulnerability**)
- Always `conn.commit()` after INSERT/UPDATE/DELETE
- Use `conn.row_factory = sqlite3.Row` for dict-like access

### 5.3 — SQLAlchemy ORM (The Real Deal)

SQLAlchemy is Python's most popular database toolkit. The ORM lets you work with Python objects instead of writing SQL.

```bash
pip install sqlalchemy
```

```python
# models.py
from sqlalchemy import create_engine, Column, Integer, String, DateTime, ForeignKey, Boolean
from sqlalchemy.orm import declarative_base, relationship, sessionmaker
from datetime import datetime

# Connect to database
engine = create_engine("sqlite:///bookshelf.db", echo=True)  # echo=True shows SQL queries
Session = sessionmaker(bind=engine)
Base = declarative_base()

# ── Define Models ──
class Author(Base):
    __tablename__ = "authors"
    
    id = Column(Integer, primary_key=True)
    name = Column(String(100), nullable=False)
    birth_year = Column(Integer)
    created_at = Column(DateTime, default=datetime.now)
    
    # Relationship: one author has many books
    books = relationship("Book", back_populates="author", cascade="all, delete-orphan")
    
    def __repr__(self):
        return f"Author(id={self.id}, name={self.name!r})"

class Book(Base):
    __tablename__ = "books"
    
    id = Column(Integer, primary_key=True)
    title = Column(String(200), nullable=False)
    isbn = Column(String(13), unique=True)
    published_year = Column(Integer)
    author_id = Column(Integer, ForeignKey("authors.id"), nullable=False)
    
    # Relationship: each book belongs to an author
    author = relationship("Author", back_populates="books")
    
    def __repr__(self):
        return f"Book(id={self.id}, title={self.title!r})"

# Create tables (only needs to run once)
Base.metadata.create_all(engine)
```

#### CRUD with SQLAlchemy

```python
# app.py
from models import Session, Author, Book

db = Session()

# ── CREATE ──
author = Author(name="J.R.R. Tolkien", birth_year=1892)
db.add(author)
db.flush()  # assigns ID without committing

book = Book(title="The Hobbit", isbn="9780547928227", published_year=1937, author=author)
db.add(book)
db.commit()
print(f"Created: {author}, {book}")

# ── READ ──
# Single record
hobbit = db.query(Book).filter_by(isbn="9780547928227").first()
print(hobbit.title)

# Filter with conditions
fantasy = db.query(Book).filter(Book.published_year > 1900).all()

# Join query — books with their authors
books_with_authors = db.query(Book, Author).join(Author).all()
for book, author in books_with_authors:
    print(f"{book.title} by {author.name}")

# Query with multiple filters
results = db.query(Book).filter(
    Book.published_year >= 1900,
    Book.title.ilike("%the%")  # case-insensitive LIKE
).order_by(Book.published_year).limit(10).all()

# ── UPDATE ──
hobbit = db.query(Book).filter_by(isbn="9780547928227").first()
hobbit.title = "The Hobbit, or There and Back Again"
db.commit()

# ── DELETE ──
db.delete(hobbit)
db.commit()

# ── Count and aggregate ──
book_count = db.query(Book).count()
authors_with_books = db.query(Author).join(Book).distinct().all()

db.close()
```

### 5.4 — Relationship Patterns

```python
# One-to-Many (Author → Books) — shown above

# Many-to-Many (Books ↔ Tags)
from sqlalchemy import Table

# Association table (no model needed)
book_tags = Table(
    "book_tags", Base.metadata,
    Column("book_id", Integer, ForeignKey("books.id")),
    Column("tag_id", Integer, ForeignKey("tags.id"))
)

class Tag(Base):
    __tablename__ = "tags"
    
    id = Column(Integer, primary_key=True)
    name = Column(String(50), unique=True, nullable=False)
    books = relationship("Book", secondary=book_tags, back_populates="tags")

# Add to Book model:
class Book(Base):
    # ... existing columns ...
    tags = relationship("Tag", secondary=book_tags, back_populates="books")

# Usage:
db = Session()
tag_fiction = Tag(name="fiction")
tag_classic = Tag(name="classic")

hobbit = db.query(Book).first()
hobbit.tags.extend([tag_fiction, tag_classic])
db.commit()

# Query books by tag
fiction_books = db.query(Book).filter(Book.tags.any(Tag.name == "fiction")).all()
```

### 5.5 — Alembic (Database Migrations)

When you change your models, you need to update the database schema. Alembic handles this.

```bash
pip install alembic
alembic init migrations  # creates migrations/ folder
```

```python
# alembic.ini — set your database URL
# sqlalchemy.url = sqlite:///bookshelf.db
```

```python
# migrations/env.py — import your models
from models import Base
target_metadata = Base.metadata
```

```bash
# After changing models, auto-generate migration:
alembic revision --autogenerate -m "add tags table"

# Review the generated file in migrations/versions/
# Then apply:
alembic upgrade head

# Rollback last migration:
alembic downgrade -1
```

### 5.6 — 📂 Project 8: Bookshelf API

**Objective:** Upgrade the Phase 2 library system into a proper API with SQLite via SQLAlchemy.

**Features:**
- FastAPI endpoints for Book and Author CRUD
- SQLite database with SQLAlchemy ORM
- Search books by title, author, genre
- Filter by year range
- Pagination
- Proper error handling

**Hints:**
> HINT 1: Define `Book` and `Author` models with SQLAlchemy. Author has a relationship to books.
> 
> HINT 2: Create a `get_db()` dependency for FastAPI:
> ```python
> from fastapi import Depends
> from sqlalchemy.orm import Session
> 
> def get_db():
>     db = SessionLocal()
>     try:
>         yield db
>     finally:
>         db.close()
> 
> # Use in routes:
> @app.get("/books")
> def list_books(db: Session = Depends(get_db)):
>     return db.query(Book).all()
> ```
> 
> HINT 3: Use Alembic to manage schema changes as you develop.

**Git checkpoints:**
```bash
git checkout -b feature/bookshelf-api
git commit -am "feat: add SQLAlchemy models for Book and Author"
git commit -am "feat: add CRUD API endpoints"
git commit -am "feat: add search and pagination"
# Add .db to .gitignore!
echo "*.db" >> .gitignore
git commit -am "chore: ignore database files"
git checkout main && git merge feature/bookshelf-api
```

### 5.7 — 📂 Project 9: Inventory Management System

**Objective:** Build an inventory API for a small warehouse/stockroom.

**Features:**
- Products (name, SKU, price, quantity, category)
- Categories with hierarchy (Electronics > Phones)
- Stock movements (in/out) with timestamp and reason
- Low-stock alerts
- Reports: total value, items by category

**Hints:**
> HINT 1: Use self-referential relationship for categories (parent_id points to another category).
> 
> HINT 2: Stock movements should reference a product and have: `quantity_change` (positive for in, negative for out), `reason` (purchase/sale/adjustment), `timestamp`.
> 
> HINT 3: Low-stock report: query products where `quantity <= threshold`.

---

## Phase 6: Docker Fundamentals

> **Duration:** 3–5 days  
> **Goal:** Containerize your Python projects so they run anywhere  
> **Project:** Containerize the Blog API from Phase 4  
> **Git:** Dockerfile committed alongside code

### 6.1 — What is Docker?

Docker packages your app + all its dependencies into a **container** — a lightweight, portable unit that runs the same everywhere.

```
Without Docker:     "Works on my machine" 😩
With Docker:         Works everywhere — your laptop, a server, CI/CD
```

**Key concepts:**
- **Image:** Blueprint for a container (like a class). Read-only.
- **Container:** Running instance of an image (like an object). Has its own filesystem.
- **Dockerfile:** Recipe to build an image (text file).
- **Docker Hub:** Registry of public images (like PyPI for Docker).

### 6.2 — Your First Dockerfile

```dockerfile
# Dockerfile (no extension)

# Start from an official Python image
FROM python:3.12-slim

# Set working directory inside the container
WORKDIR /app

# Copy requirements first (layer caching — only re-installs when deps change)
COPY requirements.txt .

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of the code
COPY . .

# Expose the port the app runs on (documentation — doesn't actually publish it)
EXPOSE 8000

# Command to run when container starts
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

```bash
# Build the image
docker build -t my-blog-api .

# Run the container
docker run -d -p 8000:8000 --name blog-api my-blog-api

# Test it
curl http://localhost:8000/docs

# See running containers
docker ps

# Stop and remove
docker stop blog-api
docker rm blog-api
```

### 6.3 — Dockerfile Best Practices

```dockerfile
# ── Multi-stage build (smaller final image) ──
# Stage 1: Build dependencies
FROM python:3.12-slim AS builder
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir --user -r requirements.txt

# Stage 2: Runtime (only what's needed to run)
FROM python:3.12-slim
WORKDIR /app

# Copy installed packages from builder
COPY --from=builder /root/.local /root/.local
ENV PATH=/root/.local/bin:$PATH

COPY . .

EXPOSE 8000
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
```

```dockerfile
# ── .dockerignore (like .gitignore for Docker) ──
# Create .dockerignore in project root:
# .venv/
# __pycache__/
# .git/
# *.db
# .env
# .dockerignore
```

### 6.4 — Docker Compose (Multi-Container Apps)

When your app needs multiple services (API + database + cache), use Docker Compose.

```yaml
# docker-compose.yml
services:
  # Your FastAPI app
  api:
    build: .
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql://user:pass@db:5432/blogdb
      - SECRET_KEY=change-me-in-production
    depends_on:
      - db
    volumes:
      - .:/app            # Hot reload in development
    
  # PostgreSQL database
  db:
    image: postgres:16-alpine
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=pass
      - POSTGRES_DB=blogdb
    ports:
      - "5432:5432"
    volumes:
      - pgdata:/var/lib/postgresql/data    # Persist data

volumes:
  pgdata:      # Named volume — survives container restarts
```

```bash
# Start everything
docker compose up -d

# See logs
docker compose logs -f api

# Stop everything
docker compose down

# Stop and delete all data
docker compose down -v
```

### 6.5 — Working with Volumes

```bash
# Named volumes (persistent data)
docker volume create mydata
docker run -v mydata:/app/data myimage

# Bind mounts (mount local folder into container)
docker run -v $(pwd):/app myimage

# Useful for development — edit files locally, changes appear in container
```

### 6.6 — 📂 Project 10: Containerize the Blog API

**Objective:** Take your Blog API from Phase 4 and make it run via Docker Compose with PostgreSQL.

**Steps:**
1. Move your blog API code into a `blog-api/` folder
2. Add a `requirements.txt`: `fastapi uvicorn sqlalchemy passlib pyjwt`
3. Create a `Dockerfile`
4. Create a `docker-compose.yml` with API + PostgreSQL
5. Update your SQLAlchemy connection string to use `DATABASE_URL` env var
6. Add Alembic for migrations
7. `docker compose up` and verify everything works

**Git checkpoints:**
```bash
git checkout -b feature/docker-blog
git commit -am "feat: add Dockerfile for blog API"
git commit -am "feat: add docker-compose with PostgreSQL"
git commit -am "feat: update app to use DATABASE_URL env var"
git checkout main && git merge feature/docker-blog
```

---

## Phase 7: Full-Stack Integration & Advanced Git

> **Duration:** 5–7 days  
> **Goal:** Build real multi-service projects, master Git workflows  
> **Projects:** URL Shortener, Real-time Chat  
> **Git:** Interactive rebase, CI/CD basics

### 7.1 — Advanced Git: Interactive Rebase & History Cleanup

```bash
# Interactive rebase — squash, reorder, edit past commits
git rebase -i HEAD~5   # last 5 commits

# In the editor that opens:
pick a1b2c3d feat: add user model
squash d4e5f6a fix: typo in user model
pick g7h8i9j feat: add auth endpoints

# Commands:
# pick   = keep as-is
# squash = merge into previous commit (combine messages)
# reword = keep commit, edit the message
# edit   = pause to amend the commit
# drop   = remove the commit entirely

# Stashing — save uncommitted work temporarily
git stash                    # save current changes
git stash list               # see all stashes
git stash pop                # apply most recent stash AND remove it
git stash apply stash@{2}   # apply specific stash, keep it
```

### 7.2 — 📂 Project 11: URL Shortener (Full Stack)

**Objective:** Build a URL shortener API with Docker, PostgreSQL, and click tracking.

**Architecture:**
```
Client → FastAPI (port 8000) → PostgreSQL (port 5432)
                ↓
         Short URL → redirects to original
```

**Features:**
- POST a long URL → get a short code
- GET /{code} → 302 redirect to original URL
- Track click count per short URL
- Stats endpoint: top URLs, click history
- Rate limiting (100 URLs per hour per IP)

**Database schema:**
```sql
CREATE TABLE urls (
    id SERIAL PRIMARY KEY,
    short_code VARCHAR(10) UNIQUE NOT NULL,
    original_url TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    click_count INTEGER DEFAULT 0
);

CREATE TABLE clicks (
    id SERIAL PRIMARY KEY,
    url_id INTEGER REFERENCES urls(id),
    clicked_at TIMESTAMP DEFAULT NOW(),
    ip_address VARCHAR(45),
    user_agent TEXT
);
```

**Hints:**
> HINT 1: For short codes, use `hashids` library or base62 encoding of the URL's hash.
> ```python
> import hashlib, string
> ALPHABET = string.ascii_letters + string.digits  # 62 chars
> 
> def generate_code(url: str, length: int = 6) -> str:
>     h = int(hashlib.md5(url.encode()).hexdigest()[:8], 16)
>     code = ""
>     while h > 0 and len(code) < length:
>         code += ALPHABET[h % 62]
>         h //= 62
>     return code
> ```
> 
> HINT 2: For redirects, use `from fastapi.responses import RedirectResponse` → `return RedirectResponse(url, status_code=302)`.
> 
> HINT 3: Record clicks in a background task using FastAPI's `BackgroundTasks`.

**Git workflow:**
```bash
git checkout -b feature/url-shortener
git commit -am "feat: database models for URL shortener"
git commit -am "feat: shortening and redirect endpoints"
git commit -am "feat: click tracking and stats"
git commit -am "feat: add docker-compose with PostgreSQL"
git checkout main && git merge feature/url-shortener
```

### 7.3 — WebSockets (Real-Time Communication)

```python
# FastAPI WebSocket — chat server
from fastapi import FastAPI, WebSocket, WebSocketDisconnect
from fastapi.responses import HTMLResponse

app = FastAPI()

html = """
<!DOCTYPE html>
<html>
<body>
    <h1>Chat</h1>
    <input type="text" id="message" placeholder="Type a message...">
    <button onclick="send()">Send</button>
    <div id="messages"></div>
    <script>
        const ws = new WebSocket(`ws://${location.host}/ws`);
        ws.onmessage = (e) => {
            document.getElementById("messages").innerHTML += `<p>${e.data}</p>`;
        };
        function send() {
            const msg = document.getElementById("message").value;
            ws.send(msg);
        }
    </script>
</body>
</html>
"""

class ConnectionManager:
    def __init__(self):
        self.connections: list[WebSocket] = []
    
    async def connect(self, ws: WebSocket):
        await ws.accept()
        self.connections.append(ws)
    
    def disconnect(self, ws: WebSocket):
        self.connections.remove(ws)
    
    async def broadcast(self, message: str, exclude: WebSocket = None):
        for conn in self.connections:
            if conn != exclude:
                await conn.send_text(message)

manager = ConnectionManager()

@app.get("/")
async def get():
    return HTMLResponse(html)

@app.websocket("/ws")
async def websocket_endpoint(websocket: WebSocket):
    await manager.connect(websocket)
    try:
        while True:
            data = await websocket.receive_text()
            await manager.broadcast(f"User says: {data}", exclude=websocket)
    except WebSocketDisconnect:
        manager.disconnect(websocket)
```

### 7.4 — 📂 Project 12: Real-Time Chat App

**Objective:** Build a chat application with WebSocket support.

**Features:**
- Multiple chat rooms
- User nicknames
- Message history (persisted to DB)
- Online user list
- Docker Compose (FastAPI + PostgreSQL)

**Hints:**
> HINT 1: Extend `ConnectionManager` to track which room each connection is in.
> 
> HINT 2: Store messages in a `messages` table with room_id, username, content, timestamp.
> 
> HINT 3: On connect, send the last 50 messages from the room as a "history" event.

### 7.5 — CI/CD Basics (GitHub Actions)

```yaml
# .github/workflows/test.yml
name: Tests

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:16-alpine
        env:
          POSTGRES_DB: testdb
          POSTGRES_USER: test
          POSTGRES_PASSWORD: test
        ports:
          - 5432:5432
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
    
    steps:
      - uses: actions/checkout@v4
      
      - uses: actions/setup-python@v5
        with:
          python-version: "3.12"
      
      - name: Install dependencies
        run: pip install -r requirements.txt
      
      - name: Run tests
        env:
          DATABASE_URL: postgresql://test:test@localhost:5432/testdb
        run: pytest -v
  
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: "3.12"
      - run: pip install ruff
      - run: ruff check .
```

**Commit it:**
```bash
git checkout -b feature/ci-pipeline
git add .github/
git commit -m "ci: add GitHub Actions test and lint workflow"
git checkout main && git merge feature/ci-pipeline
```

---

## Phase 8: Capstone Projects

> **Duration:** 2–3 weeks  
> **Goal:** Apply everything — OOP, APIs, databases, Docker, Git — in substantial projects  
> These are portfolio-worthy projects you can deploy and show to anyone.

### 8.1 — 📂 Project 13: Personal Finance Tracker

**Objective:** A full API to track income, expenses, budgets, and generate reports.

**Tech stack:** FastAPI + SQLAlchemy + PostgreSQL + Docker + Alembic

**Features:**
- User registration and JWT auth
- Accounts (checking, savings, cash)
- Transactions with categories (Food, Transport, Salary, etc.)
- Monthly budgets per category
- Reports: spending by category, income vs expenses, monthly trends
- CSV export

**Database schema to implement:**
```sql
-- users, accounts, categories, transactions, budgets
-- transactions table: id, user_id, account_id, category_id, amount, 
--   description, type (income/expense), date
-- budgets: user_id, category_id, amount, month, year
```

**Expected API:**
```
POST   /api/register
POST   /api/login
GET    /api/accounts
POST   /api/accounts
GET    /api/transactions?category=food&from=2024-01-01&to=2024-12-31
POST   /api/transactions
GET    /api/reports/summary?month=1&year=2024
GET    /api/reports/by-category?month=1&year=2024
GET    /api/reports/trends?months=6
GET    /api/export/transactions.csv
```

**Hints:**
> HINT 1: Use SQLAlchemy `func.sum()` and `func.count()` for aggregate reports:
> ```python
> from sqlalchemy import func
> 
> # Total spending by category
> results = db.query(
>     Category.name,
>     func.sum(Transaction.amount).label("total")
> ).join(Transaction).group_by(Category.name).all()
> ```
> 
> HINT 2: For date range filtering: `Transaction.date >= start_date, Transaction.date <= end_date`.
> 
> HINT 3: For CSV export, use Python's `csv` module with `StreamingResponse`:
> ```python
> from fastapi.responses import StreamingResponse
> import csv, io
> 
> @app.get("/export/transactions.csv")
> def export_csv(db: Session = Depends(get_db)):
>     output = io.StringIO()
>     writer = csv.writer(output)
>     writer.writerow(["Date", "Category", "Amount", "Description"])
>     for t in db.query(Transaction).all():
>         writer.writerow([t.date, t.category.name, t.amount, t.description])
>     output.seek(0)
>     return StreamingResponse(iter([output.getvalue()]),
>         media_type="text/csv",
>         headers={"Content-Disposition": "attachment; filename=transactions.csv"})
> ```

### 8.2 — 📂 Project 14: Markdown Blog Platform

**Objective:** A blog platform where you write posts in Markdown and they render as beautiful HTML.

**Tech stack:** FastAPI + SQLAlchemy + PostgreSQL + Jinja2 templates + Docker

**Features:**
- User auth (register, login)
- Write posts in Markdown (with preview)
- Render Markdown to HTML with syntax highlighting
- Tags and categories
- Full-text search
- RSS feed
- Static page serving

**New concepts to learn:**
```python
# Markdown rendering — pip install markdown
import markdown

md_text = """
# Hello World

This is **bold** and this is *italic*.

```python
print("code blocks work too!")
```
"""

html = markdown.markdown(md_text, extensions=["fenced_code", "codehilite", "tables"])
# Returns HTML string with proper formatting
```

```python
# RSS Feed — pip install feedgen or build manually
from fastapi.responses import Response

@app.get("/feed.xml")
def rss_feed(db: Session = Depends(get_db)):
    posts = db.query(Post).order_by(Post.created_at.desc()).limit(20).all()
    
    items = "\n".join(f"""
    <item>
        <title>{post.title}</title>
        <link>http://localhost:8000/posts/{post.slug}</link>
        <description>{post.summary}</description>
        <pubDate>{post.created_at.strftime('%a, %d %b %Y')}</pubDate>
    </item>""" for post in posts)
    
    xml = f"""<?xml version="1.0" encoding="UTF-8"?>
    <rss version="2.0"><channel>
        <title>My Blog</title><link>http://localhost:8000</link>
        {items}
    </channel></rss>"""
    
    return Response(content=xml, media_type="application/rss+xml")
```

### 8.3 — 📂 Project 15: Task Queue System

**Objective:** Build a background job processing system — like a simplified Celery.

**Tech stack:** FastAPI + Redis + PostgreSQL + Docker

**Architecture:**
```
API Server → adds job to Redis queue
                  ↓
Worker Process → pulls job from Redis → processes it → stores result
                  ↓
API Server → returns job status/result to client
```

**Features:**
- Submit a "task" (e.g., resize an image, send an email, process data)
- Track task status (pending/running/completed/failed)
- Store task results
- Task priority (high/normal/low)
- Worker that processes tasks from queue
- Docker Compose: API + Worker + Redis + PostgreSQL

**New concepts:**
```python
# Redis with Python — pip install redis
import redis
import json
import uuid

r = redis.Redis(host="localhost", port=6379, db=0)

# Push a task to the queue
task_id = str(uuid.uuid4())
task = {"id": task_id, "type": "resize_image", "params": {"url": "...", "width": 200}}
r.lpush("task_queue", json.dumps(task))   # LEFT push (FIFO)

# Worker pulls tasks
def worker():
    while True:
        _, raw = r.brpop("task_queue")     # BLOCKING right pop
        task = json.loads(raw)
        result = process_task(task)
        r.set(f"result:{task['id']}", json.dumps(result))

# Check task status
result = r.get(f"result:{task_id}")
```

**Docker Compose:**
```yaml
services:
  api:
    build: ./api
    ports: ["8000:8000"]
    depends_on: [redis, db]
  worker:
    build: ./worker
    depends_on: [redis, db]
  redis:
    image: redis:7-alpine
    ports: ["6379:6379"]
  db:
    image: postgres:16-alpine
    # ... same as before
```

### 8.4 — Where to Go From Here

After completing these projects, you have solid foundations. Here are natural next steps:

**Frontend Integration:**
- Build a React/Vue/Svelte frontend that consumes your APIs
- Or use HTMX for simple server-rendered interactivity

**Advanced Python:**
- `asyncio` deep dive — `async/await` for I/O-bound concurrency
- `aiohttp` or `httpx` for async HTTP clients
- `uvloop` for high-performance event loops

**DevOps Depth:**
- Kubernetes (container orchestration at scale)
- Terraform (infrastructure as code)
- Monitoring (Prometheus + Grafana)

**Career Projects:**
- Deploy to a VPS (Hetzner, DigitalOcean) with Docker Compose
- Set up a domain with nginx reverse proxy + Let's Encrypt SSL
- Open source a project on GitHub with proper README, docs, and CI

---

## 📚 Quick Reference: Common Commands

### Python
```bash
python3 -m venv .venv && source .venv/bin/activate
pip install package_name
pip freeze > requirements.txt
python -m pytest                    # run tests
python -m uvicorn main:app --reload # run FastAPI dev server
ruff check .                        # lint
```

### Git
```bash
git init
git checkout -b feature/name
git add . && git commit -m "feat: description"
git push -u origin feature/name
git checkout main && git merge feature/name
git stash / git stash pop
git log --oneline --graph --all
git rebase -i HEAD~N
```

### Docker
```bash
docker build -t name .
docker run -d -p 8000:8000 --name app name
docker compose up -d
docker compose logs -f service
docker compose down -v
docker ps / docker exec -it container bash
```

---

> *"The best way to learn is to build. The best way to build is to ship."*
> 
> Good luck, Parsa. 🚀

