# Python 07: For Loops (Part 1)

Nov 7, 2026 · @Mohammad Mauhid Usmani

Roadmap: DataPathsala IC study plan, Day 8. Video: [For Loop in Python Part-1 | Lec-10](https://youtu.be/5J_xH6X-qGw).

## 1. What is a loop, and why do we need one?

A **loop** repeats the same work again and again, like putting a song on repeat in Spotify. Python has two loops:

- **`for` loop**: goes through a collection (a list, a range of numbers, a string, ...) one item at a time. Manish's estimate is that about 95% of the loops you'll see in his team's code are `for` loops.
- **`while` loop**: repeats while a condition is true. It comes later (Day 10).

Manish warns that loops are the point where most beginners get frustrated and quit, so go slowly and make sure lists, indexes, variables and `if` are clear first.

**The problem.** We have our four labourers and want this output:

```text
Labour 1 name is Mahesh
Labour 2 name is Ramesh
Labour 3 name is Mithilesh
Labour 4 name is Sumesh
```

With what we know so far, that's four `logger.info` lines using `labour_name[0]` to `labour_name[3]`. Fine for four people. Now imagine a class of 100 students before the 10th board exams: you wouldn't write 100 lines. Doing the same step repeatedly is exactly what we **automate**: write one line, and let a loop run it for every item.

## 2. Method 1: loop directly over the list

```python
from loguru import logger

logger.info("Programming is easy")

labour_name = ["Mahesh", "Ramesh", "Mithilesh", "Sumesh"]

for name in labour_name:
    logger.info(name)
```

Output (the timestamp is cut off; the number after `<module>:` is the line number):

```text
INFO     | __main__:<module>:3 - Programming is easy
INFO     | __main__:<module>:8 - Mahesh
INFO     | __main__:<module>:8 - Ramesh
INFO     | __main__:<module>:8 - Mithilesh
INFO     | __main__:<module>:8 - Sumesh
```

This is why Manish switches from `print` to `logger.info` here: every name comes from **line 8**, so you can see the program going back to the same line again and again.

**How it runs, step by step:**

1. Python takes the list stored in `labour_name`.
2. It creates a **new variable**, `name`, and assigns it the **first** item: `name = "Mahesh"`.
3. It runs the indented block (the loop **body**): logs `Mahesh`.
4. It goes back to the top and assigns the **next** item: `name = "Ramesh"`. The old value is overwritten.
5. This repeats until the list runs out. Then the loop ends and the program continues after the indented block.

Each full run of the body is called an **iteration**. Four items, four iterations.

**Syntax rules:**

- `for <variable> in <collection>:` ends with a **colon**.
- The body is **indented** (4 spaces; VS Code does it when you press Enter after the colon), just like `if` in chapter 4.
- The loop variable can be any valid name: `for abc in labour_name:` works, then you log `abc`. Pick a name that describes one item (`name`, `wage`, `number`), and don't reuse the list's name (`for labour_name in labour_name` would overwrite your list).

If you add another value to the list (Manish adds `200`), the loop picks it up automatically: lists can hold mixed types, and the loop doesn't care.

**Errors you'll hit (verified):**

```python
for name in labour_name     # missing colon
# SyntaxError: expected ':'

for name in labour_name:
logger.info(name)           # body not indented
# IndentationError: expected an indented block after 'for' statement on line 2
```

## 3. Where Method 1 falls short

Try to produce the target output with an f-string:

```python
for name in labour_name:
    logger.info(f"Labour 1 name is {name}")
```

```text
INFO     | __main__:<module>:6 - Labour 1 name is Mahesh
INFO     | __main__:<module>:6 - Labour 1 name is Ramesh
INFO     | __main__:<module>:6 - Labour 1 name is Mithilesh
INFO     | __main__:<module>:6 - Labour 1 name is Sumesh
```

The name changes, but the number is stuck at 1. We need something that counts. That's `range`.

## 4. range(): a sequence of numbers

`range` gives you whole numbers one after another. It takes **one, two or three** arguments:

| Call | Meaning | Numbers produced |
| --- | --- | --- |
| `range(5)` | from 0 up to 5, 5 excluded | `0, 1, 2, 3, 4` |
| `range(5, 11)` | from 5 up to 11, 11 excluded | `5, 6, 7, 8, 9, 10` |
| `range(5, 11, 2)` | from 5, jumping 2 each time | `5, 7, 9` |
| `range(10, 0, -2)` | counting down | `10, 8, 6, 4, 2` |
| `range(5, 5)` | start equals end | nothing (empty) |

It's the same start / end / step idea as slicing (chapter 6): **start included, end excluded**, step defaults to 1. So to get 5 to 10 you write `range(5, 11)`.

Manish says the one-argument form `range(n)` is what you'll use almost all the time.

**range is lazy.** It doesn't build a list in memory; it hands out numbers as the loop asks for them. Logging `range(5)` shows `range(0, 5)`, not the numbers. Wrap it in `list()` to see them: `list(range(5))` → `[0, 1, 2, 3, 4]`.

**Only integers:** `range(1.5)` → `TypeError: 'float' object cannot be interpreted as an integer`.

## 5. Method 2: loop over the indexes with range(len(...))

`len(labour_name)` is 4, so `range(len(labour_name))` is `range(4)`: `0, 1, 2, 3`, exactly the valid indexes of the list. Loop over those, and use each index to fetch the name:

```python
from loguru import logger

labour_name = ["Mahesh", "Ramesh", "Mithilesh", "Sumesh"]

for i in range(len(labour_name)):
    logger.info(f"Labour {i + 1} name is {labour_name[i]}")
```

```text
INFO     | __main__:<module>:6 - Labour 1 name is Mahesh
INFO     | __main__:<module>:6 - Labour 2 name is Ramesh
INFO     | __main__:<module>:6 - Labour 3 name is Mithilesh
INFO     | __main__:<module>:6 - Labour 4 name is Sumesh
```

Walk through the first iteration: `i = 0`, so `labour_name[i]` is `labour_name[0]` = `"Mahesh"`, and `i + 1` is 1. Indexes start at 0 but people count from 1, hence the `+ 1`. (Manish asks you to find your own fix first; `i + 1` is one of many.)

**Hard-coded vs dynamic values.** Why `len(labour_name)` and not `range(4)`? Because today there are 4 labourers, tomorrow 5, next month 10. A fixed number typed into code is a **hard-coded value**; it silently breaks when the data changes. `len(labour_name)` is **dynamic**: it always matches the list.

**Method 1 or Method 2?**

| | `for name in labour_name` | `for i in range(len(labour_name))` |
| --- | --- | --- |
| You get | the item | the index (fetch the item with `labour_name[i]`) |
| Use when | you only need the values | you need the position, or to change the list in place |

**Changing items needs the index (verified):**

```python
daily_wage = [500, 400, 400, 300]

for wage in daily_wage:
    wage = wage + 50        # changes only the loop variable
print(daily_wage)           # [500, 400, 400, 300]  ← list unchanged

for i in range(len(daily_wage)):
    daily_wage[i] = daily_wage[i] + 50
print(daily_wage)           # [550, 450, 450, 350]
```

**The Pythonic way (beyond the lecture): `enumerate`.** It gives you the position and the item together, and `start=1` handles the counting:

```python
for number, name in enumerate(labour_name, start=1):
    logger.info(f"Labour {number} name is {name}")
# Labour 1 name is Mahesh ... Labour 4 name is Sumesh  (same output as Method 2)
```

You'll see `enumerate` a lot in real code; it's worth knowing that it exists now.

**A house example (beyond the lecture).** Two lists that line up by position, as in the wage examples from chapter 5, walked together with Method 2:

```python
from loguru import logger

labour_name = ["Mahesh", "Ramesh", "Mithilesh", "Sumesh"]
daily_wage = [500, 400, 400, 300]
days_worked = 30

for i in range(len(labour_name)):
    monthly_pay = daily_wage[i] * days_worked
    logger.info(f"{labour_name[i]} earns {monthly_pay} this month")
```

```text
INFO     | __main__:<module>:9 - Mahesh earns 15000 this month
INFO     | __main__:<module>:9 - Ramesh earns 12000 this month
INFO     | __main__:<module>:9 - Mithilesh earns 12000 this month
INFO     | __main__:<module>:9 - Sumesh earns 9000 this month
```

## 6. Syntax vs logic building

After Method 2, Manish stops to make a point: so far you have learnt **syntax**, the grammar of Python. **Programming** is **logic building**: looking at a problem, spotting the pattern, and planning the steps. At the start writing code feels hard; later the logic is the hard part. His advice: **solve it on pen and paper first**, tracing every iteration by hand, before you type anything.

## 7. Example: right-angle triangle of stars

Print this (each star is followed by a space):

```text
* 
* * 
* * * 
* * * * 
* * * * * 
```

**Spot the pattern:** 5 lines, so the loop runs 5 times. Line 1 has 1 star, line 2 has 2, ... line *n* has *n* stars. The count grows with the loop, so we must use `i`.

**Attempt 1: ignore `i`.**

```python
for i in range(5):
    print("*")
```

```text
*
*
*
*
*
```

Five lines, one star each: the body does the same thing every time because it never uses `i`.

**The key trick: multiplying a string repeats it.**

```python
print(2 * "m")      # mm
print("x" * 3)      # xxx
print(0 * "*")      # (an empty line: 0 copies is the empty string '')
```

**Attempt 2: `i * "*"`.**

```python
for i in range(5):
    print(i * "*")
```

```text

*
**
***
****
```

Close, but `range(5)` starts at 0, so the first line is empty and we only reach 4 stars.

**Attempt 3: add 1 (and trip over precedence).**

```python
for i in range(5):
    print(i + 1 * "*")
# TypeError: unsupported operand type(s) for +: 'int' and 'str'
```

Multiplication happens before addition (the BODMAS rule from chapter 3), so Python first does `1 * "*"` = `"*"`, then tries `0 + "*"`: an int plus a string. Brackets fix the order.

**Final version:**

```python
for i in range(5):
    print((i + 1) * "* ")
```

```text
* 
* * 
* * * 
* * * * 
* * * * * 
```

Here `print` is used on purpose: logger lines would put a prefix in front of every row and spoil the shape. (`range(1, 6)` with `i * "* "` also works.)

## 8. Example: even numbers from 1 to 100

Before coding, Manish writes the **algorithm**, the plan in plain steps:

1. Run a loop.
2. Loop over the numbers with `range`, up to 101 (the end is excluded).
3. For each number, check the condition `number % 2 == 0` (divisible by 2; `%` is from chapter 3, `if` from chapter 4).
4. Only numbers that pass the check are logged. No `else` is needed: for odd numbers we do nothing.

```python
from loguru import logger

for number in range(101):
    if number % 2 == 0:
        logger.info(number)
```

```text
INFO     | __main__:<module>:5 - 0
INFO     | __main__:<module>:5 - 2
INFO     | __main__:<module>:5 - 4
...
INFO     | __main__:<module>:5 - 98
INFO     | __main__:<module>:5 - 100
```

(51 lines in total; the middle is cut here.) Note that this includes **0**, which isn't in "1 to 100". `range(1, 101)` fixes that.

**Flow of one iteration:** `for` line → `if` check → `logger.info` only if true → back to the `for` line for the next number, until the range runs out. The `if` is indented inside the `for`, and the `logger.info` is indented inside the `if`: indentation decides what belongs to what.

**Variations from the lecture:** multiples of 3 below 40:

```python
for number in range(1, 40):
    if number % 3 == 0:
        logger.info(number)
# 3, 6, 9, 12, 15, 18, 21, 24, 27, 30, 33, 36, 39
```

**Naming:** Manish first writes `i`, then renames it to `number`. A descriptive name tells the reader what the variable holds.

**Shortcut (beyond the lecture):** when the pattern is regular, the step argument can replace the `if` entirely: `range(2, 101, 2)` gives exactly the 50 even numbers from 2 to 100. Knowing both ways matters: the `if` version works for any condition, the step version only for evenly spaced numbers.

## 9. Interview questions

1. What is a loop? What are the two kinds of loops in Python, and when would you use each?
2. Explain step by step what happens when `for name in labour_name:` runs.
3. What does `range(2, 10, 3)` produce? Is 10 included?
4. Why is `range(len(my_list))` better than `range(4)`? What is a hard-coded value?
5. When do you need the index in a loop instead of the item?
6. Why doesn't `for wage in wages: wage = wage + 50` change the list?
7. What does `enumerate` do?
8. What does `3 * "ab"` return? What about `0 * "ab"`?
9. Why does `i + 1 * "*"` raise a `TypeError`?
10. Is the loop variable still available after the loop ends? What value does it hold?

## 10. Practice questions

Use a `for` loop in every question. Use `logger.info` (except for star patterns, where `print` keeps the shape). Plan on paper first.

**Easy**

- [ ] E1. Log every labourer's name in capital letters (`.upper()`).
- [ ] E2. Log the numbers from 10 down to 1 using `range`.
- [ ] E3. Log the odd numbers from 1 to 50, once with an `if` and once with only `range`.
- [ ] E4. Log `"Mistri: Jagmohan"` and `"Mistri: Ram Pyare"` from the list `["Jagmohan", "Ram Pyare"]`.

**Medium**

- [ ] M1. **Manish's assignment:** print the inverted right-angle triangle (5 stars on the first line, then 4, 3, 2, 1, each star followed by a space).
- [ ] M2. With `labour_name` and `daily_wage = [500, 400, 400, 300]`, add up the total daily wage bill in a loop (don't use `sum()`), then log it.
- [ ] M3. Bricks cost ₹10.5 each. Log the cost of 1000, 2000, ... 5000 bricks using `range` with a step.
- [ ] M4. Log only the labourers whose daily wage is 400 or more.

**Hard**

- [ ] H1. **DSA (Day 8 SOLVE): check if an array is sorted.** Given a list of numbers, log `True` if it is sorted in non-decreasing order, otherwise `False`. Test with `[300, 400, 400, 500]`, `[500, 400, 300]`, `[7]` and `[]`. Don't use `sort()` or `sorted()`.
- [ ] H2. Print a right-aligned triangle (spaces before the stars) of height 5.
- [ ] H3. Without running it, predict the output of this code, then run it and explain:
  ```python
  labour_name = ["Mahesh", "Ramesh", "Mithilesh", "Sumesh"]
  for i in range(1, len(labour_name), 2):
      print(i, labour_name[i])
  print(i)
  ```

**External practice:** HackerRank Python → "Loops", "Print Function".

## 11. Notes and corrections

Add your own notes here. When this chapter is final, tell me and I'll mark it **Final** in the README.

**Notes on the lecture**

- Manish calls `range` and `len` "methods". Strictly, they're **built-in functions** (`range` is actually a built-in type): you call them as `range(5)` and `len(x)`, not on an object with a dot like `labour_name.append(x)`. The word "method" is for the dotted kind.
- He says range "takes three inputs by default". It *accepts* one, two or three arguments; only `stop` is required.
- He says you shouldn't write `i` in real code. That's good advice for **values** (`number`, `name`, `wage`). For a plain index, `i` is a widely accepted convention; most style guides allow it.
- While writing the even-numbers plan, the auto-transcript says "range 10" in places; the code he runs uses `range(101)`, which is what this chapter shows.
- The even-numbers code starts at 0 and so logs 0, although the question says 1 to 100 (section 8).
- He mentions a live class "on Wednesday" and that the camera overheated; those are left out.
- His "95% of loops are for loops" is his experience, not a measured figure.

**Added beyond the lecture:** counting down with a negative step, `range` being lazy, the float `TypeError`, the colon and indentation errors, changing list items via the index, `enumerate`, the monthly wage example, the `range(2, 101, 2)` shortcut, the loop variable after the loop (interview question 10), and the DSA problem (from the study plan, not the video).

**Gotcha found while testing this chapter:** don't name your own file after a standard module. A test script saved as `enum.py` broke `from loguru import logger` with `ImportError: cannot import name 'logger' from partially initialized module 'loguru' (most likely due to a circular import)`, because Python imported the local `enum.py` instead of its own `enum` module. Same trap with `random.py`, `logging.py`, `string.py`, etc.

**Sources:** Manish Kumar, [For Loop in Python Part-1 | Lec-10](https://youtu.be/5J_xH6X-qGw) (transcript); [Python docs: for statements](https://docs.python.org/3/tutorial/controlflow.html#for-statements); [Python docs: the range() function](https://docs.python.org/3/tutorial/controlflow.html#the-range-function); [Python docs: enumerate](https://docs.python.org/3/library/functions.html#enumerate).
