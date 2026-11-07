# Python 02: print, String Formatting, Escape Sequences and Logging

Nov 1, 2026 · @Mohammad Mauhid Usmani

## 1. Why do we need print?

Variables change while a program runs. `print()` lets you check that a value is what you expect at a given point. Scattering prints through code to watch its behaviour is the simplest form of **debugging**.

```python
length_of_land = 100

if length_of_land == 100:
    length_of_land = 200
else:
    length_of_land = 300

print(length_of_land)   # 200, as expected
```

Now change the first line to `length_of_land = "100"` (a string). The output becomes `300`, because `"100" == 100` is `False`. Without the print, you might never notice the bug. Converting with `int("100")` fixes it.

**Label your output.** `print(200, 10.5)` forces the reader to guess what each number means:

```python
print("Length of land is", length_of_land)   # Length of land is 200
print("Labour name is", labour_mistri_1)    # Labour name is Jagmohan
```

**VS Code tips from the lecture:** `Ctrl + /` comments or uncomments the current line (cursor can be anywhere on it). `Home` and `End` jump to the start and end of a line; `Shift + arrows` select text.

## 2. print() in depth

**Three kinds of quotes.** A string must start and end with the same quote.

| Quotes | Example | Use |
| --- | --- | --- |
| Single `'...'` | `'My home is of 4BHK'` | Same as double |
| Double `"..."` | `"My home is of 4BHK"` | Same as single |
| Triple `'''...'''` or `"""..."""` | text across several lines | Multi-line strings, docstrings |

A normal string can't span lines (red squiggle, `SyntaxError`). Triple quotes can, and keep the line breaks and indentation exactly as typed:

```python
print("""My home is of 4BHK
Length of the total land is 100""")
```

**Mixing quotes.** Use one kind outside and the other inside:

```python
print("My home is of '4BHK'")    # My home is of '4BHK'
print('My home is of "4BHK"')    # My home is of "4BHK"
print("My home is of "4BHK"")   # SyntaxError: the string closes before 4BHK
```

This comes up constantly when building SQL queries in Python, where SQL strings need their own quotes.

**Useful print parameters** (full signature: `print(*objects, sep=' ', end='\n', file=sys.stdout, flush=False)`)

```python
print("Land", 100, "ft")                 # Land 100 ft   (items joined with a space)
print("2026", "11", "01", sep="-")     # 2026-11-01
print("Loading", end="...")
print("done")                            # Loading...done (same line)
```

- `sep` changes what goes between items (default one space).
- `end` changes what is printed at the end (default a newline).

## 3. Escape sequences

A backslash `\` inside a string changes the meaning of the character right after it.

| Sequence | Meaning | Example | Output |
| --- | --- | --- | --- |
| `\n` | New line | `"4BHK\nLength 100"` | two lines |
| `\t` | Tab | `"of\t4BHK"` | `of      4BHK` |
| `\"` | Literal double quote | `"My home is of \"4BHK\""` | `My home is of "4BHK"` |
| `\'` | Literal single quote | `'Ram\'s house'` | `Ram's house` |
| `\\` | Literal backslash | `"C:\\data"` | `C:\data` |

```python
print("My home is of 4BHK\nLength of the total land is 100")
# My home is of 4BHK
# Length of the total land is 100

print("My home is of \"4BHK\"")   # My home is of "4BHK"
```

**Correction to the lecture:** `\` does not "skip any character". It only has a special meaning for specific characters (the ones above, plus a few like `\r`). `\t` moves to the next tab stop, which is usually 8 columns in a terminal, not always 4 spaces.

**Raw strings** (`r"..."`) turn off escapes. Very common for Windows paths and regular expressions:

```python
print("C:\new_folder")    # C:
                          # ew_folder   (\n became a newline!)
print(r"C:\new_folder")   # C:\new_folder
```

## 4. String formatting

Putting variables inside text cleanly. Production code uses f-strings (most common) or `.format()`.

**f-strings (Python 3.6+)**: put `f` before the quote and variables in `{}`.

```python
bricks_cost_per_piece = 10.5
labour_mistri_1 = "Jagmohan"
labour_mistri_2 = "Ram Pyare"

print(f"Cost of bricks per unit is {bricks_cost_per_piece}")
# Cost of bricks per unit is 10.5

print(f"Mistris: {labour_mistri_1} and {labour_mistri_2}")
```

Without the `f`, `{bricks_cost_per_piece}` prints literally as text.

**`.format()`**: `{}` are placeholders filled in order by the arguments.

```python
print("Cost of bricks per unit is {}".format(bricks_cost_per_piece))

print("{} costs {}, mistri {}".format("Brick", bricks_cost_per_piece, labour_mistri_2))
#        index 0     index 1                   index 2

print("{1} then {0}".format("first", "second"))   # second then first
print("{name} earns {wage}".format(name="Jagmohan", wage=1000))
```

Indexes start at 0. f-strings are preferred because the variable sits right where it's used.

**f-strings can do more than insert variables**

```python
length, breadth = 100, 20
print(f"Garden area: {length * breadth} sq ft")   # expressions work
print(f"{1250000:,}")             # 1,250,000   (thousands separator)
print(f"{10.5:.2f}")              # 10.50       (2 decimal places)
print(f"{0.875:.1%}")             # 87.5%
print(f"[{'4BHK':>8}]")           # [    4BHK] (right-align in 8 chars)
print(f"{length=}")               # length=100  (handy for debugging, 3.8+)
```

You'll also see the old `%` style in older code: `"Cost is %s" % bricks_cost_per_piece`. Recognise it; don't write new code with it.

## 5. Logging: print for real projects

In a 10,000-line codebase, `print` output doesn't tell you *when* or *where* it came from. Hard-coding line numbers breaks the moment you add a line. Production code uses **logging**, which adds timestamp, level, file and line number automatically. You'll rarely see `print` in company code.

**Built-in `logging` module**

```python
import logging

logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s | %(levelname)s | %(filename)s:%(lineno)d | %(message)s",
)

length_of_land = 100
logging.info(f"Length of land is {length_of_land}")
# 2026-11-01 14:50:12,345 | INFO | print.py:9 | Length of land is 100
```

Without `level=logging.INFO`, `info` messages are hidden, because the default level is `WARNING`.

**Log levels** (lowest to highest): `DEBUG` → `INFO` → `WARNING` → `ERROR` → `CRITICAL`. Setting a level shows that level and everything above it. Levels are covered in detail later.

**loguru (third-party, what Manish uses)**: cleaner, coloured, zero setup.

```bash
pip install loguru
```

```python
from loguru import logger

logger.info(f"Cost of bricks per unit is {bricks_cost_per_piece}")
# 2026-11-01 14:50:12.345 | INFO | __main__:<module>:12 - Cost of bricks per unit is 10.5
```

Note it's `from loguru import logger`; plain `import loguru` then `logger.info` gives `NameError`.

**Script layout used in companies:** imports at the top, then constants and variables, then functions and classes, then the code that runs. Logging calls instead of prints.

## 6. Interview questions

1. What is the difference between single, double and triple quotes in Python?
2. How do you print a double quote inside a double-quoted string? Give two ways.
3. What are escape sequences? Name four.
4. What is a raw string and when would you use one?
5. What do the `sep` and `end` parameters of `print()` do?
6. Compare f-strings, `.format()` and `%` formatting. Which do you prefer and why?
7. How do you format a float to 2 decimal places in an f-string?
8. Why use logging instead of print in production code?
9. What are the logging levels, and what is the default level?
10. Why doesn't `logging.info("hi")` show anything with the default setup? *(Default level is WARNING.)*

## 7. Practice questions

Solve these yourself in VS Code before asking for help.

**Easy**

- [x] E1. Print `My home is of "4BHK"` in three different ways (different outer quotes, escape sequence, triple quotes).
- [x] E2. Print the land, garden and home dimensions on three lines using a single `print()` with `\n`.
- [x] E3. Print today's date as `2026-11-01` from three separate values using `sep`.
- [x] E4. Using an f-string, print `Mistri Jagmohan earns Rs 1000 per day`, with the name and wage from variables.

**Medium**

- [x] M1. Print a small bill for the house materials as an aligned table (item name left-aligned in 12 characters, price right-aligned in 10, 2 decimal places), using f-string format specs.
- [x] M2. Write the same sentence with an f-string, `.format()` with positional indexes, and `.format()` with keyword names.
- [x] M3. Print the Windows path `C:\new\test.csv` correctly two ways. Explain why the plain version prints wrongly.
- [x] M4. Set up `logging` so every message shows time, level, file name and line number. Log the land area at INFO and a warning if the home area is bigger than the land.

**Hard**

- [x] H1. Build a SQL query string in Python that selects a labourer by name, where the name comes from a variable and SQL needs single quotes around it. Then find out what goes wrong if the name is `D'Souza`, and why real code uses query parameters instead.
- [ ] H2. Switch the M4 logger to DEBUG and add debug messages. Then switch it to WARNING. Note which messages disappear and explain why.
- [ ] H3. Install loguru and make it also write logs to a file `house.log`. (Hint: the docs for `logger.add`.)

**External practice:** HackerRank Python → "Print Function", "String Formatting", "Text Alignment".

## 8. Your notes and corrections

Add your own notes here. When this chapter is final, tell me and I'll mark it **Final saved** in the roadmap tracker.

**Corrections to the lecture**

- `\` doesn't skip any following character; it only means something for specific escape characters (`\n`, `\t`, `\"`, `\'`, `\\`, etc.).
- `\t` jumps to the next tab stop (usually 8 columns in a terminal), not exactly 4 spaces.
- The lecture copies the `basicConfig` setup from Stack Overflow; the full version with level and line number is in section 5.
- loguru must be imported as `from loguru import logger`.

**Added beyond the lecture:** `sep`/`end`, raw strings, f-string format specs, `{var=}` debugging, log levels.

**Sources:** Manish Kumar, Python lecture 5 (print, string formatting, escape sequences, logging); [Python docs: f-strings and format()](https://docs.python.org/3/tutorial/inputoutput.html); [Python logging HOWTO](https://docs.python.org/3/howto/logging.html); [loguru on GitHub](https://github.com/Delgan/loguru).
