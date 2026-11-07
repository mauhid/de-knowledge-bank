# Python 04: if, elif, else and Nested Conditions

Nov 4, 2026 · @Mohammad Mauhid Usmani

## 1. Why conditions?

In everyday life: "*If* it's the weekend, we'll go out; *else* I'll be working." Programs make the same kind of decision with `if`, `elif` and `else`.

**Where data engineers use them**

| Situation | The check |
| --- | --- |
| Validate output | If the result matches what's expected, continue; otherwise stop |
| Empty data | If the list or DataFrame has rows, process it; else raise an error ("data is empty, fix the source first") |
| Backdated job runs | When a pipeline fails and you must rerun yesterday's load: if the date passed is today, run today's process; else run it for the given date |
| User input checks | Like a sign-up form rejecting an email without `@` |

Every `if` builds on the operators from chapter 3, especially the comparison operators.

## 2. Comparison operators return True or False

| Operator | Meaning | Note |
| --- | --- | --- |
| `==` | equal to | `=` assigns a value; `==` compares |
| `!=` | not equal to |  |
| `<` / `>` | less than / greater than |  |
| `<=` / `>=` | less than or equal / greater than or equal | Written `<=`, not `=<` as in maths (≤) |

```python
a = 5
print(a > 5)    # False
print(a >= 5)   # True
print(a == 5)   # True
print(a != 5)   # False
```

Every comparison produces a `bool`: `True` or `False`, with a capital T and F. **This is the key idea of the chapter:** an `if` doesn't understand "length is small". It only sees the `True` or `False` that the condition produces.

## 3. if and else

**Rule:** the code inside an `if` runs only when its condition is `True`. Otherwise the `else` block runs. Exactly one of the two runs.

Our scenario: a 4BHK house needs land at least 100 ft long.

```python
from loguru import logger

length_of_land = int(input("Enter the length of your land: "))

if length_of_land < 100:
    logger.info("Your length is not sufficient to build a 4BHK")
    logger.info("second line")     # also runs: same indentation = same block
else:
    logger.info("Share more details with us")

logger.info("Done")                 # outside both blocks: always runs
```

Input `50` → `50 < 100` is `True` → the two `if` lines run. Input `120` → `False` → the `else` line runs.

**Syntax rules**

1. `if`, then the condition, then a colon `:`. Brackets around the condition are optional: `if (length_of_land < 100):` also works.
2. The block below must be **indented**, by convention 4 spaces (`Tab` in VS Code inserts them). Every line with the same indentation belongs to that block.
3. `else` takes no condition, just `else:`.
4. To end the block, go back to the previous indentation (`Backspace`).

**Common errors**

```python
if length_of_land = 100:   # SyntaxError: invalid syntax. Maybe you meant '==' or ':=' instead of '='?
if length_of_land == 100   # SyntaxError: expected ':'
if length_of_land == 100:
logger.info("hi")          # IndentationError: expected an indented block after 'if' statement
```

Indentation is not style in Python; it's how Python knows where a block starts and ends (other languages use `{ }`).

## 4. elif: checking more than two cases

`elif` ("else if") adds more conditions between `if` and `else`.

```python
if length_of_land < 100:
    logger.info("Your length is not sufficient to build a 4BHK")
elif length_of_land >= 500:
    logger.info("You can build more than two buildings")
else:
    logger.info("Share more details with us")
```

**How Python decides (top to bottom):**

1. Check the `if`. `True` → run its block and **skip everything else** in the chain.
2. `False` → check the first `elif`. Same rule.
3. Keep going through each `elif` until one is `True`.
4. None was `True` → run `else` (if there is one).

At most one block runs, and it's the **first** one whose condition is `True`.

| Input | `< 100`? | `>= 500`? | Output |
| --- | --- | --- | --- |
| 50 | True | not checked | not sufficient |
| 300 | False | False | share more details (else) |
| 500 | False | True | more than two buildings |

**`>` vs `>=` matters at the edge.** With `elif length_of_land > 500:`, input `500` falls through to `else`, because 500 isn't greater than 500. Always test the boundary values.

- You can have any number of `elif`s; `elif` and `else` are both optional.
- Order matters: put the most specific conditions first, or a broader one will catch the value before it gets there.
- `if`, `elif` and `else` of one chain must line up at the same indentation.

## 5. Nested if: an if inside an if

If the land is under 100 ft, we can still help: between 80 and 100 ft fits a 3BHK.

```python
if length_of_land < 100:
    logger.info("Your length is not sufficient to build a 4BHK")
    logger.info("second line")
    if length_of_land > 80:                         # only checked when < 100 was True
        logger.info("You can build a 3BHK house")
    else:
        logger.info("Your land does not have enough space")
elif length_of_land >= 500:
    logger.info("You can build more than two buildings")
else:
    logger.info("Share more details with us")
```

The inner `if`/`else` is indented one level deeper, so it belongs to the outer `if` block.

**Trace it like Manish did on the board** (all outputs verified by running):

| Input | Outer check | Inner check | Lines logged |
| --- | --- | --- | --- |
| 20 | `20 < 100` True | `20 > 80` False → else | not sufficient, second line, not enough space (3 lines) |
| 90 | `90 < 100` True | `90 > 80` True | not sufficient, second line, build a 3BHK (3 lines) |
| 150 | False; `150 >= 500` False → else | not reached | share more details |
| 600 | False; `600 >= 500` True | not reached | more than two buildings |

If the inner `else` were removed, input 20 would log only the first two lines.

Keep nesting shallow: more than 2–3 levels gets hard to read. Section 6 shows how `and` can often flatten it.

## 6. Beyond the lecture

**Combining conditions with `and`, `or`, `not`**

| Expression | True when |
| --- | --- |
| `A and B` | both are True |
| `A or B` | at least one is True |
| `not A` | A is False |

```python
if length_of_land < 100 and length_of_land > 80:
    logger.info("You can build a 3BHK house")

if 80 < length_of_land < 100:     # chained comparison: same thing, more readable
    logger.info("You can build a 3BHK house")
```

`and` and `or` **short-circuit**: in `A and B`, if `A` is False, Python doesn't evaluate `B` at all. That's useful for safe checks like `if data and data[0] > 0:`.

**Truthiness: any value can be a condition.** These count as `False`: `0`, `0.0`, `""` (empty string), `[]`, `{}`, `None`. Almost everything else is `True` (even `" "` and `[0]`).

```python
rows = []
if not rows:
    logger.error("Data is empty, fix the source before processing")
```

This is the Pythonic way to do the "is the list empty?" check from section 1, instead of `if len(rows) == 0:`.

**One-line if (conditional expression)**

```python
status = "even" if n % 2 == 0 else "odd"
```

Use it for simple value choices only; use a normal `if` block for anything longer.

**`match`/`case`** (Python 3.10+) compares one value against many patterns, like `switch` in other languages. You'll meet it later; `if`/`elif` covers almost everything at this stage.

## 7. Interview problem: even or odd

**Question:** how will you find out whether a number given by the user is even or odd?

Interviews test two things here: logic, and turning it into code. **Plan on pen and paper first, then code.** That habit matters more as problems get harder.

**Logic:** a number is even if it divides by 2 with nothing left over, i.e. `n % 2 == 0` (`4 % 2` → `0`, `5 % 2` → `1`).

**Steps**

1. Take the user's input (as an `int`).
2. Calculate `number % 2`.
3. Store the result in a variable.
4. Check it with a condition.
5. Return (log) the output.

Manish's assignment: write the code yourself before looking below.

```python
from loguru import logger

number = int(input("Enter a number: "))
result = number % 2

if result == 0:
    logger.info(f"{number} is even")
else:
    logger.info(f"{number} is odd")
```

It works for negatives and zero too: `-3 % 2` is `1` (odd), `0 % 2` is `0` (even). Variable names are case sensitive, so stick to one spelling (`result`, not `Result` in one place and `result` in another).

## 8. Interview questions

1. What is the difference between `=` and `==`?
2. What does a comparison like `a >= 5` return?
3. Explain how Python executes an `if`/`elif`/`else` chain. Can more than one block run?
4. What is the difference between several `elif`s and several separate `if`s? *(Separate `if`s are each checked; in a chain, only the first True runs.)*
5. Why is indentation important in Python? What error do you get without it?
6. What is a nested `if`? When would you replace it with `and`?
7. Which values are "falsy" in Python? How do you check if a list is empty?
8. What is short-circuit evaluation? Give an example where it prevents an error.
9. Write a one-line expression that returns `"pass"` or `"fail"` based on a score.
10. How would you check whether a number is even or odd? Does your solution work for negatives?

## 9. Practice questions

Plan each one on paper first (input → steps → conditions → output), then code it with `logger.info`.

**Easy**

- [x] E1. Even or odd (section 7), written by you without looking.
- [x] E2. Take a number from the user and log whether it is positive, negative or zero.
- [x] E3. Take a person's age and log whether they can vote (18 or older).
- [x] E4. Predict the output, then run: `bool(0)`, `bool("0")`, `bool([])`, `bool(" ")`, `bool(None)`, `5 == 5.0`.

**Medium**

- [x] M1. Take two numbers and log the larger one, or "equal" if they're the same. Then do it for three numbers.
- [x] M2. Grade calculator: marks ≥ 90 → A, ≥ 75 → B, ≥ 50 → C, else Fail. Then put the conditions in the wrong order (≥ 50 first) and explain what breaks.
- [x] M3. Take a year and check if it's a leap year (divisible by 4, but not by 100 unless also by 400). Test with 2024, 1900, 2000.
- [x] M4. Extend the house program: take both length and breadth. Reject if either is under 100; suggest a 3BHK if both are between 80 and 100; otherwise approve the 4BHK.

**Hard**

- [ ] H1. Calculator: take two numbers and an operator (`+ - * / // % **`) from the user and log the result. Handle division by zero and an unknown operator with clear messages.
- [ ] H2. Electricity bill: first 100 units at ₹5, next 100 at ₹7, above 200 at ₹10 per unit. Take units and log the total bill.
- [ ] H3. Backdated run: take a date string like `2026-11-03` and today's date (`2026-11-04`) as input. Log "running today's load" if they match, "running backfill for \<date>" if the date is earlier, and an error if it's in the future. (Hint: dates in `YYYY-MM-DD` form compare correctly as strings.)

**External practice:** HackerRank Python → "Python If-Else", "Write a function" (leap year).

## 10. Your notes and corrections

Add your own notes here. When this chapter is final, tell me and I'll mark it **Final saved** in the roadmap tracker.

**Notes on the lecture**

- In his first `elif` demo he wrote `> 500`; input 500 went to `else`. He then changed it to `>= 500`. This chapter uses `>= 500` throughout.
- The transcript at one point says the condition for input 50 "came out False"; it's `True` (`50 < 100`), which is why the `if` block ran. Likely a slip of the tongue.
- He says brackets around the condition are optional; that's correct, but Python style (PEP 8) leaves them out.

**Added beyond the lecture:** the error messages for missing `:`, `=` vs `==` and indentation; `and`/`or`/`not`; chained comparisons; short-circuiting; truthiness; one-line `if`; `match`/`case` mention.

**Sources:** Manish Kumar, Python lecture 7 (if-else in Python); [Python tutorial: if statements](https://docs.python.org/3/tutorial/controlflow.html#if-statements); [Python docs: truth value testing](https://docs.python.org/3/library/stdtypes.html#truth-value-testing); [Python docs: conditional expressions](https://docs.python.org/3/reference/expressions.html#conditional-expressions).
