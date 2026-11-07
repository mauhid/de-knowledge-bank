# Python 03: Operators, Type Casting and User Input

Nov 4, 2026 · @Mohammad Mauhid Usmani

## 1. Arithmetic operators

An operator performs an operation on values (operands). In `2 + 3`, `+` is the operator and `2` and `3` are the operands.

| Operator | Name | Example | Result |
| --- | --- | --- | --- |
| `+` | Addition | `15 + 6` | `21` |
| `-` | Subtraction | `15 - 6` | `9` |
| `*` | Multiplication | `15 * 6` | `90` |
| `/` | Division (always returns float) | `15 / 6` | `2.5` |
| `//` | Floor division | `15 // 6` | `2` |
| `%` | Modulo (remainder) | `15 % 6` | `3` |
| `**` | Power | `2 ** 5` | `32` |

**House example: area of the land**

```python
from loguru import logger

length_of_land = 100
breadth_of_land = 100

# Calculate the total area of the land
total_area_of_land = length_of_land * breadth_of_land
logger.info(f"Total area of my land is {total_area_of_land} sq ft")
# ... | INFO | __main__:<module>:8 - Total area of my land is 10000 sq ft
```

From this chapter on, we use `logger.info` (or `logging.info`) instead of `print`.

**Don't name your file after a Python module.** Manish named his file `operator.py` and got `ImportError: ... circular import`. Python has a built-in `operator` module, and your file shadows it. The same happens with `math.py`, `random.py`, `logging.py`, etc. Rename the file (`F2` in VS Code), e.g. `operators_and_type_casting.py`.

The `operator` module itself provides operators as functions (`operator.add(a, b)`, `operator.mod(a, b)`); its [docs page](https://docs.python.org/3/library/operator.html) is a handy list of every operator.

## 2. Operator precedence (BODMAS)

When an expression has several operators, Python decides the order by precedence, just like BODMAS in maths.

**The perimeter bug.** Perimeter = 2 × (length + breadth), so a 100 × 100 plot should give 400 ft.

```python
perimeter_of_land = 2 * length_of_land + breadth_of_land
logger.info(f"Perimeter of land is {perimeter_of_land} ft")   # 300 ft  (wrong!)
```

`*` runs before `+`, so Python computed (2 × 100) + 100 = 300. Brackets fix it:

```python
perimeter_of_land = 2 * (length_of_land + breadth_of_land)    # 400 ft
```

**Python's precedence, highest first**

| Level | Operators |
| --- | --- |
| 1 | `()` brackets |
| 2 | `**` power |
| 3 | `-x` unary minus |
| 4 | `*`, `/`, `//`, `%` |
| 5 | `+`, `-` |
| 6 | Comparisons: `==`, `!=`, `<`, `>`, `<=`, `>=`, `in`, `is` |
| 7 | `not`, then `and`, then `or` |

Operators on the same level run left to right, so `*` and `/` are equal; BODMAS's "D before M" doesn't apply. The exception is `**`, which runs right to left: `2 ** 3 ** 2` = `2 ** 9` = `512`.

**Rule for real code:** when in doubt, add brackets. They make the intent clear to the reader, too.

## 3. Modulo, floor division, ceil, floor and abs

**Modulo `%`** gives the remainder of a division.

```python
print(15 % 6)    # 3   (6 × 2 = 12, remainder 3)
```

Uses: checking even/odd (`n % 2 == 0`), extracting digits (`153 % 10` → `3`, the last digit; used in Armstrong number and digit-sum problems), cycling through items, and "every Nth row" logic in data pipelines.

**Division vs floor division**

```python
print(15 / 6)    # 2.5  (true division, always a float)
print(15 // 6)   # 2    (floor division)
print(7.0 // 2)  # 3.0  (floor division of a float stays a float)
```

**Floor vs ceiling.** Think of a room: the floor is the integer below, the ceiling is the integer above. For 2.14, floor is 2 and ceiling is 3. `//` gives the floor directly; for ceiling, use the `math` module.

```python
import math

print(15 / 7)               # 2.142857142857143
print(math.ceil(15 / 7))    # 3
print(math.floor(15 / 7))   # 2
```

**Careful with negative numbers.** Floor means "round down towards minus infinity", not "chop off the decimal":

```python
print(-15 // 6)          # -3   (not -2)
print(-15 % 6)           # 3    (the remainder takes the sign of the divisor)
print(math.floor(-2.2))  # -3
print(math.ceil(-2.2))   # -2
print(int(-2.7))         # -2   (int() truncates towards zero)
```

Python keeps `(a // b) * b + (a % b) == a` true for all integers, which is why `-15 % 6` is `3`.

**Absolute value `abs()`** drops the sign: `abs(-26.2)` → `26.2`. Manish used it so a negative area still shows as positive. In real code, it's usually better to reject invalid input (a negative length) than to hide it with `abs()` (you'll learn this with `if` statements).

## 4. Other operator types

The lecture only touches these briefly. They're used heavily from the `if` and loops chapters onwards.

| Type | Operators | Example | Result |
| --- | --- | --- | --- |
| Comparison | `==` `!=` `<` `>` `<=` `>=` | `length_of_land >= 100` | `True` |
| Assignment | `=` `+=` `-=` `*=` `/=` `//=` `%=` `**=` | `count += 1` (same as `count = count + 1`) | adds 1 |
| Logical | `and` `or` `not` | `is_home and length_of_land > 50` | `True` |
| Membership | `in` `not in` | `"Ram" in "Ram Pyare"` | `True` |
| Identity | `is` `is not` | `house_owner is None` | `True` |
| Bitwise | AND `&`, OR (vertical bar), XOR `^`, NOT `~`, shifts `<<` `>>` | `5 & 3` | `1` |

- `=` assigns; `==` compares. Mixing them up is a classic bug.
- Comparisons return a `bool`, so you can store them: `is_big_plot = total_area_of_land > 5000`.
- Bitwise operators work on binary digits. They're rare in data engineering, but PySpark uses `&` and `|` for combining filter conditions (covered in Spark).

## 5. Type casting

**Why it matters: the same operator behaves differently by type.**

```python
a = "1"
b = "2"
print(a + b)          # 12    (not 3!) — both are strings, so + joins them
print("Manish" + "252")  # Manish252
print("25" * 3)       # 252525  (str * int repeats the string)
```

Joining strings with `+` is called **concatenation**. Mixing a string and a number fails:

```python
print("25" + 25)
# TypeError: can only concatenate str (not "int") to str
```

This error shows up a lot in Spark too, when someone tries to add a column that was loaded as a string.

**Type casting** means converting a value from one data type to another.

| Function | Converts to | Example | Result |
| --- | --- | --- | --- |
| `int()` | integer | `int("25")`, `int(12.9)` | `25`, `12` (truncates, doesn't round) |
| `float()` | float | `float("12.5")`, `float(7)` | `12.5`, `7.0` |
| `str()` | string | `str(25)` | `"25"` |
| `bool()` | boolean | `bool(0)`, `bool("")`, `bool("0")` | `False`, `False`, `True` |

```python
a = "25"
b = 25
print(int(a) + b)   # 50
print(a + str(b))   # 2525
```

**Two kinds of type casting**

|  | Explicit | Implicit |
| --- | --- | --- |
| Who converts | You, the developer, with `int()`, `float()`, `str()` | Python, automatically |
| When | Whenever you write it | Mixing compatible numbers (`bool` → `int` → `float`) |
| Example | `int("25") + 25` → `50` | `1.5 + 7` → `8.5` (7 becomes `7.0`) |

```python
a = 1.5
b = 7
print(a + b)        # 8.5   implicit: b is promoted to float
print(int(a) + b)   # 8     explicit: 1.5 → 1
print(True + 1)     # 2     implicit: True → 1
```

Python never implicitly converts between `str` and numbers; that's why `"25" + 25` errors (strong typing, from chapter 1).

**When casting fails: `ValueError`**

```python
int("Manish")   # ValueError: invalid literal for int() with base 10: 'Manish'
int("12.5")     # ValueError — use int(float("12.5")) → 12
```

"Base 10" means the decimal system we normally use (digits 0–9). Binary is base 2 (0–1) and hexadecimal is base 16 (0–9, A–F). `int()` can read other bases too: `int("1100100", 2)` → `100`.

**Know the error names:** `NameError` (unknown variable), `SyntaxError` (invalid code), `TypeError` (wrong type for an operation), `ValueError` (right type, but an invalid value).

## 6. User input with input()

So far every value was hard-coded. A real program asks the user, e.g. a house-cost calculator where each person enters their own land size and brick price. `input("message")` shows the message, waits for the user to type and press Enter, and returns what they typed.

**`input()` always returns a string**, even if the user types a number:

```python
length_of_land = input("Please enter the length of your land: ")
breadth_of_land = input("Please enter the breadth of your land: ")

total_area_of_your_land = length_of_land * breadth_of_land
# TypeError: can't multiply sequence by non-int of type 'str'

logger.info(f"{type(length_of_land)}")   # <class 'str'>
```

**Fix: cast the input.** Two equivalent ways:

```python
# Way 1: cast while reading (preferred)
length_of_land = float(input("Please enter the length of your land: "))

# Way 2: read first, cast after
breadth_of_land = input("Please enter the breadth of your land: ")
breadth_of_land = float(breadth_of_land)

total_area_of_your_land = length_of_land * breadth_of_land
logger.info(f"Total area of your land is {total_area_of_your_land} sq ft")
# Input 26.2 and 89.36 → Total area of your land is 2341.232 sq ft
```

Use `int(input(...))` for whole numbers and `float(input(...))` for decimals.

**Shaping the output**

```python
int(26.2 * 89.36)          # 2341   (drops the decimals)
round(26.2 * 89.36, 2)     # 2341.23
abs(26.2 * -89.36)         # 2341.232 (positive even if the user typed a negative)
```

Brackets run first, so `int(length_of_land * breadth_of_land)` multiplies, then converts.

**What can go wrong:** if the user types `abc`, `float("abc")` raises `ValueError` and the program crashes. Handling that properly needs `try`/`except` (exception handling chapter).

**Coming up:** a basic two-number calculator is the assignment for this lecture. The advanced calculator, which keeps taking inputs, needs loops.

## 7. Interview questions

1. What is the difference between `/` and `//`?
2. What does `%` return? Give two real uses.
3. What is `-7 // 2`? Why isn't it `-3`? *(It's `-4`: floor rounds towards minus infinity.)*
4. Difference between `math.floor()`, `math.ceil()`, `int()` and `round()`?
5. Explain Python's operator precedence. What is `2 + 3 * 4 ** 2`? *(50)*
6. What is the difference between `=` and `==`? Between `==` and `is`?
7. What is type casting? Explain explicit vs implicit casting with examples.
8. Why does `"1" + "2"` give `"12"`, and `"1" + 2` give an error?
9. What data type does `input()` return? How do you read a number from the user?
10. What is the difference between `TypeError` and `ValueError`? Give an example of each.
11. Why might naming your file `math.py` or `operator.py` break your program?

## 8. Practice questions

Use `logger.info` instead of `print`. Solve them yourself before asking for help.

**Easy**

- [x] E1. Calculate and log the area and perimeter of the garden (100 × 20) and the home (80 × 60).
- [x] E2. Predict, then run: `17 / 5`, `17 // 5`, `17 % 5`, `2 ** 10`, `-17 // 5`, `-17 % 5`.
- [x] E3. Predict, then run: `"5" + "5"`, `"5" * 3`, `int("5") + 5`, `str(5) + "5"`, `float("5") + 5`.
- [x] E4. Take the user's name and age as input and log `Hello <name>, next year you will be <age + 1>`.

**Medium**

- [x] M1. **Assignment (basic calculator):** take two numbers from the user and log their sum, difference, product, division, floor division, remainder and power.
- [x] M2. A truck carries 1,500 bricks. How many trucks are needed for 12,500 bricks? How many bricks does the last truck carry? (Use `math.ceil` and `%`.)
- [x] M3. Take a 3-digit number from the user and log the sum of its digits using only `//` and `%` (no strings).
- [x] M4. Ask the user for land length, breadth and cost per sq ft, then log the total cost of the land, rounded to 2 decimal places and with a thousands separator.

**Hard**

- [ ] H1. Check whether a 3-digit number is an Armstrong number (e.g. 153 = 1³ + 5³ + 3³), using only arithmetic operators. Log `True` or `False` using a comparison operator, no `if`.
- [ ] H2. Take total minutes from the user (e.g. 1,000) and convert them to days, hours and minutes using `//` and `%`.
- [ ] H3. Prove with code that `(a // b) * b + (a % b) == a` for `a = -17, b = 5` and `a = 17, b = -5`. Then explain in your notes why Python's `%` differs from C or Java for negative numbers.

**External practice:** HackerRank Python → "Arithmetic Operators", "Python: Division", "Mod Divmod", "Power - Mod Power".

## 9. Your notes and corrections

Add your own notes here. When this chapter is final, tell me and I'll mark it **Final saved** in the roadmap tracker.

**Corrections to the lecture**

- Hexadecimal is base 16 (digits 0–9 and A–F), not base 15.
- `1.5 + 7` gives `8.5` (the transcript says "88.5", most likely a transcription error; verified by running the code).
- Floor division doesn't just "discard the digits after the point"; it rounds down, which differs for negatives (`-15 // 6` is `-3`).
- `abs()` hides a wrong (negative) input instead of rejecting it; it's fine for a demo, but validate input in real code.
- The circular-import error came from naming the file `operator.py`, which shadows Python's built-in `operator` module.

**Added beyond the lecture:** full precedence table, other operator types, `bool()`/`round()` casting, `int()` with other bases, the `TypeError` vs `ValueError` distinction.

**Sources:** Manish Kumar, Python lecture 6 (operators, type casting and user input); [Python docs: operator precedence](https://docs.python.org/3/reference/expressions.html#operator-precedence); [Python docs: built-in functions int(), float(), input()](https://docs.python.org/3/library/functions.html); [operator module](https://docs.python.org/3/library/operator.html).
