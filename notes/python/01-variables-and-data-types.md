# Python 01: Variables and Data Types

Oct 31, 2026 · @Mohammad Mauhid Usmani

## 1. What is Python?

**Interview definition:** Python is a high-level, interpreted programming language known for its dynamic typing, simplicity and versatility. Be ready to explain every word.

| Term | Meaning |
| --- | --- |
| High-level | Written in human-readable form. Low-level code (machine code) talks directly to hardware (CPU, RAM). C++ and Python are both high-level. |
| Interpreted | Code is run by an interpreter, statement by statement, not compiled to a machine-code file in advance. |
| Dynamically typed | You don't declare a variable's type; Python works it out at runtime (see section 5). |
| Simple | Clean, readable code close to English. `print("Hello World")` vs about 5 lines in C++. |
| Versatile | Used everywhere: data engineering, data analysis, data science, web/backend, automation, ML. |

**How Python actually runs your code (more precise than "line by line")**

1. You write source code in a `.py` file (high-level).
2. The CPython interpreter first compiles the whole file to **bytecode** (`imported modules are cached as .pyc files` in `__pycache__`). This is why a syntax error anywhere stops the file before anything runs.
3. The **Python Virtual Machine (PVM)** executes that bytecode instruction by instruction on your CPU and RAM. Runtime errors (like `NameError`) appear only when execution reaches that line.

**Compiled vs interpreted**

|  | Compiled (C, C++) | Interpreted (Python) |
| --- | --- | --- |
| Translation | Whole program to machine code before running | Bytecode, executed by the interpreter at runtime |
| Speed | Faster | Slower |
| Errors | Most type errors caught at compile time | Many errors appear only at runtime |
| Portability | Recompile for each OS/CPU | Same code runs anywhere Python is installed |

## 2. Variables

A variable is a name that refers to a value stored in memory. "Variable" means it can vary: you can reassign it any time.

**Running example (used through the whole Python series):** we are building a 4BHK house on a 100 ft × 100 ft plot, with a 100 × 20 ft garden, an 80 × 60 ft home area, 2 mistris (Jagmohan, Ram Pyare), 4 labourers and building materials.

```python
length_of_land = 100
breadth_of_land = 100
print(length_of_land, breadth_of_land)   # 100 100
```

**What happens in RAM**

- Programs run in RAM, not on the hard disk, because the CPU can read and write RAM fast.
- `length_of_land = 100` creates an integer **object** `100` somewhere in RAM and binds the **name** `length_of_land` to it.
- Stored as binary: 100 = `1100100` (64 + 32 + 4).
- `id(x)` returns the object's identity (in CPython, its memory address).

```python
print(id(length_of_land))   # e.g. 140712345678928
```

**Names point to objects; objects can be shared**

```python
length_of_land = 100
breadth_of_land = 100
print(id(length_of_land) == id(breadth_of_land))   # True
print(length_of_land is breadth_of_land)           # True
```

Both names point to the same object. Why: CPython **caches small integers from -5 to 256**, so every `100` is the same object. Short identifier-like strings are often reused too ("interning"). For larger numbers, whether two equal values share an object is an implementation detail, so never rely on it.

**Reassignment moves the name, it does not overwrite the object**

```python
breadth_of_land = 300   # name now points to a new object; 100 is untouched
```

When no name refers to an object any more, Python frees it (reference counting + garbage collection).

**Rule of thumb:** use `==` to compare values; use `is` only for identity checks like `x is None`.

## 3. Naming rules and conventions

**Rules (break one and you get an error)**

1. Use letters (a–z, A–Z), digits (0–9) and underscore `_` only.
2. Cannot start with a digit: `1st_length = 100` → `SyntaxError: invalid decimal literal`; `length_1st = 100` is fine.
3. Cannot be a keyword: `if`, `for`, `while`, `break`, `pass`, `class`, `def`, `True`, `None`, etc. See all with `import keyword; print(keyword.kwlist)`.
4. Case sensitive: `length`, `Length` and `LENGTH` are three different variables.

```python
length = 100
Length = 200
LENGTH = 300
print(length, Length, LENGTH)   # 100 200 300
```

**Conventions (PEP 8, Python's style guide)**

| Style | Example | Use in Python |
| --- | --- | --- |
| snake\_case | `length_of_garden` | Variables and functions (preferred) |
| camelCase | `lengthOfGarden` | Avoid in Python (common in Java/JS) |
| UPPER\_CASE | `PLOT_SIZE_FT = 100` | Constants |
| PascalCase | `HouseBuilder` | Class names |

- Use descriptive names: `bricks_cost_per_piece`, not `x`, `y`, `z`. Vague names get rejected in code review.
- Put one space on each side of `=` and after commas (PEP 8).
- Don't shadow built-ins: `list = [1, 2]` or `type = 5` breaks `list()` and `type()` later.

## 4. Data types

A data type tells Python what kind of value a variable holds and what you can do with it. Lists, tuples, sets and dictionaries are covered later as data structures.

| Type | What it holds | House example |
| --- | --- | --- |
| `int` | Whole numbers, positive or negative, any size | `length_of_land = 100` |
| `float` | Decimal numbers | `bricks_cost_per_piece = 10.5` |
| `str` | Text, in single or double quotes | `labour_mistri_1 = "Jagmohan"` |
| `bool` | `True` or `False` (capital T/F, no quotes) | `is_home = True` |
| `NoneType` | `None`: "no value yet" | `house_owner = None` |

(`complex`, e.g. `3 + 4j`, also exists but is rare in data engineering.)

**Check a type with `type()`**

```python
length_of_land = 100
bricks_cost_per_piece = 10.5
labour_mistri_1 = "Jagmohan"
is_home = True

print(type(length_of_land))          # <class 'int'>
print(type(bricks_cost_per_piece))   # <class 'float'>
print(type(labour_mistri_1))         # <class 'str'>
print(type(is_home))                 # <class 'bool'>
```

"class" appears because everything in Python is an object of some class (covered in OOP).

**Common mistakes**

```python
labour_mistri_1 = Jagmohan   # NameError: name 'Jagmohan' is not defined
is_home = true               # NameError: name 'true' is not defined
is_home = "True"             # works, but it's a str, not a bool
```

Without quotes, Python treats `Jagmohan` as a variable name and looks for it. That's why it's a `NameError`, not a `SyntaxError`.

**Good to know**

- `bool` is a subtype of `int`: `True + True == 2`.
- `0.1 + 0.2 == 0.3` is `False` because floats are approximate. Use `round()` or `decimal.Decimal` for money.
- `isinstance(x, int)` is the preferred way to check types in real code.

**float vs `decimal.Decimal`, with examples**

Floats are stored in binary, and most decimal fractions (like 0.1) have no exact binary form, so tiny errors appear. `Decimal` stores numbers in base 10, exactly as you write them.

```python
from decimal import Decimal

print(0.1 + 0.2)                     # 0.30000000000000004
print(0.1 + 0.2 == 0.3)              # False

print(Decimal("0.1") + Decimal("0.2"))           # 0.3
print(Decimal("0.1") + Decimal("0.2") == Decimal("0.3"))  # True
```

**Errors add up.** Paying ₹0.10 a thousand times:

```python
total = 0
for _ in range(1000):
    total += 0.1
print(total)        # 99.9999999999986

total = Decimal("0")
for _ in range(1000):
    total += Decimal("0.1")
print(total)        # 100.0
```

**Always create a Decimal from a string, not a float.** A float is already inexact before Decimal sees it:

```python
print(Decimal(0.1))     # 0.1000000000000000055511151231257827021181583404541015625
print(Decimal("0.1"))   # 0.1
```

**Rounding money to 2 decimal places:**

```python
from decimal import Decimal, ROUND_HALF_UP

price = Decimal("10.555")
print(price.quantize(Decimal("0.01"), rounding=ROUND_HALF_UP))   # 10.56
```

**When to use which:** use `float` for measurements and analytics where tiny errors don't matter (speed, averages). Use `Decimal` for money, invoices and anything that must match to the paisa. In data engineering, this is why warehouses have a `DECIMAL(p, s)` column type (Spark: `DecimalType`) for amounts.

## 5. Dynamic vs static typing

Python is dynamically typed: the type belongs to the **value**, and it is decided at runtime, not declared in advance.

|  | Static (C, C++, Java) | Dynamic (Python) |
| --- | --- | --- |
| Declare type? | Yes: `int length_of_land = 100;` | No: `length_of_land = 100` |
| Type checked | At compile time | At runtime |
| Can a variable change type? | No | Yes |

```python
house_status = 0               # int
house_status = "in progress"   # now str: allowed
```

**Dynamic but strong.** Python won't silently mix incompatible types:

```python
"Area: " + 100        # TypeError: can only concatenate str (not "int") to str
"Area: " + str(100)   # 'Area: 100'
```

**Type hints** are optional annotations that document what type a variable, parameter or return value should be. They were added in Python 3.5 (PEP 484).

```python
length_of_land: int = 100
bricks_cost_per_piece: float = 10.5
labour_mistri_1: str = "Jagmohan"
is_home: bool = True
house_owner: str | None = None   # Python 3.10+: either str or None

def land_area(length: int, breadth: int) -> int:
    return length * breadth
```

**Does Python enforce them? No.** The interpreter ignores hints at runtime, so this runs without any error:

```python
length_of_land: int = "hundred"   # no error
print(land_area("10", 3))         # '101010', no error
```

So why use them?

- **Readability:** the reader knows what each value should be.
- **Editor help:** VS Code autocompletes and underlines mismatches.
- **Static checkers:** tools like `mypy` read the hints before running and report `land_area("10", 3)` as an error. Many teams run them in CI.
- **Frameworks use them:** FastAPI and Pydantic read hints to validate and convert incoming data, so there they effectively do enforce types (you'll see this in the FastAPI chapter).

Hints are stored and can be inspected: `land_area.__annotations__` returns `{'length': int, 'breadth': int, 'return': int}`.

**Full definition, now explained:** Python is a high-level language (human-readable) whose code is compiled to bytecode and executed by an interpreter, with variable types decided at runtime (dynamic typing), and it is simple to read and used across many domains.

## 6. Interview questions

Try answering aloud before reading the hint.

1. What is Python? Explain each word of your definition. *(Section 1)*
2. Is Python compiled or interpreted? *(Both: compiled to bytecode, then interpreted by the PVM.)*
3. What is the difference between a high-level and a low-level language?
4. What is dynamic typing? Is Python strongly or weakly typed? *(Dynamic and strong.)*
5. What does `id()` return?
6. Why does `a = 100; b = 100; a is b` give `True`, but the same check with large numbers may give `False`? *(Small-integer cache, -5 to 256.)*
7. What is the difference between `==` and `is`?
8. What are the rules for naming a variable? Is Python case sensitive?
9. What are Python's basic data types? How do you check a variable's type?
10. Why is `0.1 + 0.2 == 0.3` false? How would you handle money values?
11. What happens in memory when you reassign a variable?
12. What are type hints, and does Python enforce them?

## 7. Practice questions

Solve these yourself in VS Code before asking for help. Answers are deliberately not included.

**Easy**

- [x] E1. Create variables for the garden (length 100, breadth 20) and print both with their types.
- [x] E2. Store both mistris' names and daily wages (1000 and 800) in four well-named variables and print them.
- [x] E3. Which of these names are valid? `home_area`, `2nd_floor`, `floor_2`, `class`, `_temp`, `total-cost`, `Total_Cost`. Predict first, then test.
- [x] E4. Predict the output, then run: `print(type(10)), print(type(10.0)), print(type("10")), print(type(True))`.

**Medium**

- [x] M1. Calculate the land area, garden area and home area (100×100, 100×20, 80×60) and the leftover open area. Print each with a label.
- [x] M2. Assign `a = 256; b = 256` and `c = 257; d = 257`. Print `a is b` and `c is d` in a script and in the interactive shell. Note what you see and explain why.
- [x] M3. Swap the values of `length_of_land` and `breadth_of_land` without a third variable.
- [x] M4. Fix this code without changing what it means: `total = "Total wage: " + 1000 + 800`.

**Hard (thinking questions)**

- [x] H1. Write `x = 10`, then `y = x`, then `x = 20`. What is `y`? Draw the names and objects to explain.
- [x] H2. Show with code that `bool` is a subtype of `int` (use `isinstance` and arithmetic).
- [x] H3. Compute the total cost of 12,500 bricks at 10.5 each using float, then using `decimal.Decimal`. Do they ever differ? When would the difference matter in a data pipeline?

**External practice:** HackerRank Python → Introduction section ("Say Hello, World!", "Python If-Else", "Arithmetic Operators").

## 8. Your notes and corrections

Add your own explanations, mistakes you made and anything from the lecture worth keeping. When this chapter is final, tell me and I'll mark it **Final saved** in the roadmap tracker.

**Corrections to the lecture (verified against Python behaviour)**

- The lecture says Python runs "line by line" with no compiler. More precisely, CPython compiles the file to bytecode first, then the virtual machine executes it. That's why a syntax error on line 50 stops line 1 from running.
- The lecture's binary for 100 was left unfinished: 100 = `1100100`.
- Two variables with the same value share one object because of small-integer caching and interning, not because RAM always reuses equal values.

**Sources:** Manish Kumar, Python lecture 4 (variables and data types); [Python tutorial: numbers, strings](https://docs.python.org/3/tutorial/introduction.html); [PEP 8 naming conventions](https://peps.python.org/pep-0008/#naming-conventions).
