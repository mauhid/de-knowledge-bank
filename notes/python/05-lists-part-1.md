# Python 05: Lists (Part 1)

Nov 4, 2026 · @Mohammad Mauhid Usmani

Roadmap: DataPathsala IC study plan, Day 6. Video: [List in Python | Lec-8](https://youtu.be/3v9f_KCCkrM).

## 1. What is a list?

A list is an **ordered collection of values of the same or different data types**, stored under one variable name.

It's the most widely used structure in Python, and you'll use it in almost every project. Weak list skills make the coming lectures (loops, comprehensions) much harder.

**Why we need it.** In chapter 1 we needed one variable per worker:

```python
labour_mistri_1 = "Ram Pyare"
labour_mistri_2 = "Jagmohan"
# 4 labourers → labour_1, labour_2, labour_3, labour_4 ...
```

With a list, one variable holds them all, and adding a new worker doesn't need a new variable:

```python
total_mistri = ["Ram Pyare", "Jagmohan"]
total_labour = ["Ramesh", "Mahesh", "Mithilesh", "Sumesh"]
```

## 2. Creating lists

Put comma-separated values inside square brackets `[ ]`.

```python
list_1 = [1, "Manish", 2.5, True]   # int, str, float, bool together: allowed
labour_names = ["Ramesh", "Mahesh", "Mithilesh", "Sumesh"]
logger.info(labour_names)           # pass just the variable name to see the whole list
```

**Empty list.** Often you create an empty list first and add values later, as your code finds them (e.g. "if this name passes the check, add it").

```python
empty_list = []
logger.info(len(empty_list))   # 0
```

`len(list)` gives the number of elements. An empty list is falsy, so `if not empty_list:` is the standard "is it empty?" check from chapter 4.

## 3. How a list sits in memory

Programs run in RAM. Think of RAM as a row of numbered blocks. A list's slots are stored **one after another (contiguously)**, and the variable points to the start of that block.

```text
labour_names ──► ┌────────┬────────┬───────────┬────────┐
                 │ Ramesh │ Mahesh │ Mithilesh │ Sumesh │
                 └────────┴────────┴───────────┴────────┘
 index:              0        1          2          3
 negative:          -4       -3         -2         -1
```

Because the slots are side by side, Python can jump straight to any position by its number, the **index**. That makes `labour_names[2]` instant, however long the list is.

**More precisely (CPython):** the contiguous slots hold *references* to the objects, not the objects themselves; the strings `"Ramesh"` etc. can live anywhere in memory. That's why one list can mix types: every slot is the same size (a reference), whatever it points to. It's also why `b = a` doesn't copy a list: both names point to the same list (more on this in Lists Part 2).

## 4. Accessing elements: indexing

Python uses **zero-based indexing**: the first element is at index `0`. Element number 1 for us is index 0 for the program, so a list of length *n* has indexes `0` to `n - 1`.

```python
labour_names = ["Ramesh", "Mahesh", "Mithilesh", "Sumesh"]

logger.info(f"First element in the list is {labour_names[0]}")   # Ramesh
logger.info(f"Last element in the list is {labour_names[3]}")    # Sumesh
```

Read `labour_names[0]` aloud as "labour\_names of zero".

**`IndexError`: list index out of range.** Ask for an index that doesn't exist and Python stops:

```python
labour_names[5]   # IndexError: list index out of range
```

You'll see this error many times in real work, usually because a list came back empty or shorter than expected. When you see it, check how many items the list actually has.

**Lists are mutable.** You can also change an element by its index:

```python
labour_names[1] = "Suresh"   # ['Ramesh', 'Suresh', 'Mithilesh', 'Sumesh']
```

**Ordered and mutable: what they mean**

**Ordered** means a list remembers the position of every item. Items stay in the order you put them, so index 0 is always the first one you added (until you change it). Two lists with the same items in a different order are *not* equal:

```python
["Ramesh", "Mahesh"] == ["Mahesh", "Ramesh"]   # False: order matters
```

That's why indexing works at all. A **set** (coming on Day 15) is unordered: it has no positions, so `my_set[0]` is an error.

**Mutable** means the list can be changed after it's created: replace, add or remove items, and it's still the *same* list object.

```python
labour_names = ["Ramesh", "Mahesh"]
old_id = id(labour_names)

labour_names[0] = "Suresh"      # replace
labour_names.append("Sumesh")   # add

logger.info(labour_names)                 # ['Suresh', 'Mahesh', 'Sumesh']
logger.info(id(labour_names) == old_id)   # True: same object, changed in place
```

**Immutable** types can't be changed after creation. Strings (and tuples, Day 14) are immutable:

```python
name = "Ramesh"
name[0] = "S"           # TypeError: 'str' object does not support item assignment
name = "S" + name[1:]   # works: builds a NEW string "Samesh" and moves the name to it
```

|  | Ordered? | Mutable? |
| --- | --- | --- |
| list | Yes | Yes |
| str | Yes | No |
| tuple (Day 14) | Yes | No |
| set (Day 15) | No | Yes |

**Why it matters:** because lists are mutable, `b = a` makes both names point to one list, so changing `b` changes `a` too (practice H3). With immutable types that surprise can't happen.

In real code you rarely pick elements one by one; you **iterate** over the list with a loop (coming in the For Loops lectures). Indexing is the foundation for that.

## 5. Negative indexing

Negative indexes count from the end: `-1` is the last element, `-2` the second last, and so on.

```python
logger.info(labour_names[-1])   # Sumesh
logger.info(labour_names[-4])   # Ramesh
```

The big advantage: `[-1]` always gives the last element **without knowing the list's length**.

**Where data engineers actually use it.** Cloud storage paths and API URLs have a fixed structure, and the part you need is often near the end. Split the string on `/` to get a list, then index from the back:

```python
url = "https://mystorage.blob.core.windows.net/raw/sales/2026/11/04/orders.csv"
parts = url.split("/")
# ['https:', '', 'mystorage.blob.core.windows.net', 'raw', 'sales', '2026', '11', '04', 'orders.csv']

file_name = parts[-1]   # 'orders.csv'
day = parts[-2]         # '04'
```

With hundreds of URLs of different lengths, counting from the front breaks; counting from the back doesn't. Manish's example was the same idea with an Azure URL: take `[-1]` for the endpoint or `[-2]` for the subscription ID. (`.split()` is covered properly in the String Methods lecture.)

## 6. Adding elements: append, extend, insert

Two new labourers, Ram and Mohan, join the site. Lists have built-in **methods** for this; type `labour_names.` in VS Code to see them all.

| Method | What it does | Example | Result |
| --- | --- | --- | --- |
| `append(x)` | Adds **one** item at the end | `labour_names.append("Ram")` | `[..., 'Sumesh', 'Ram']` |
| `extend(iterable)` | Adds **every item** of another list at the end | `labour_names.extend(["Ram", "Mohan"])` | `[..., 'Sumesh', 'Ram', 'Mohan']` |
| `insert(i, x)` | Adds one item **at index `i`**, shifting the rest right | `labour_names.insert(0, "Ram")` | `['Ram', 'Ramesh', ...]` |

```python
labour_names = ["Ramesh", "Mahesh", "Mithilesh", "Sumesh"]

labour_names.append("Ram")
labour_names.append("Mohan")     # one append per item: tedious for 20 or 50 people

new_labour = ["Ram", "Mohan"]
labour_names.extend(new_labour)  # adds them all in one call

labour_names.insert(2, "Ram")    # index 2 = third position
```

These methods change the list **in place** and return `None`, so write `labour_names.append("Ram")`, not `labour_names = labour_names.append("Ram")` (that would replace your list with `None`).

**Two classic traps (verified by running)**

```python
labour_names.append(["Ram", "Mohan"])
# ['Ramesh', 'Mahesh', 'Mithilesh', 'Sumesh', ['Ram', 'Mohan']]  ← a list inside the list, length 5

labour_names.extend("Ram")
# [..., 'Sumesh', 'R', 'a', 'm']  ← a string is iterable, so extend adds each character
```

`insert` with an index past the end doesn't error; it just appends: `insert(100, "Ram")` puts Ram last.

## 7. Multi-dimensional lists

A multi-dimensional list is a **list inside a list**. Use it when each item has several pieces of information, like each labourer's name and daily wage.

```python
labour_with_wage = [
    ["Mahesh", 500],
    ["Ramesh", 400],
    ["Mithilesh", 450],
    ["Sumesh", 300],
]
```

**Read it like a table** (rows and columns, like a matrix from school maths): the first index picks the row (inner list), the second picks the column inside it.

|  | column 0 (name) | column 1 (wage, ₹/day) |
| --- | --- | --- |
| row 0 | `[0][0]` Mahesh | `[0][1]` 500 |
| row 1 | `[1][0]` Ramesh | `[1][1]` 400 |
| row 2 | `[2][0]` Mithilesh | `[2][1]` 450 |
| row 3 | `[3][0]` Sumesh | `[3][1]` 300 |

```python
labour_with_wage[0]       # ['Mahesh', 500]  (the whole inner list)
labour_with_wage[0][1]    # 500
labour_with_wage[-1][1]   # 300  (negative indexing works at every level)

logger.info(f"Labour {labour_with_wage[0][0]} is charging a total wage of {labour_with_wage[0][1]} per day")
# Labour Mahesh is charging a total wage of 500 per day
```

**Where this goes next:** with a loop you'll total everyone's daily wage (500 + 400 + 450 + 300 = ₹1,650/day), then multiply by the days of construction. This list-of-rows shape is also exactly how rows of a table or CSV look when loaded into Python.

## 8. Interview questions

1. What is a list in Python? Can it hold different data types?
2. Is a Python list ordered? Is it mutable?
3. What does zero-based indexing mean? What are the valid indexes of a 6-element list?
4. What causes `IndexError: list index out of range`, and how do you prevent it?
5. What is negative indexing? Give a real use case.
6. Difference between `append()` and `extend()`? What happens if you `append` a list?
7. How do you add an element at the start of a list?
8. What does `labour_names.append("Ram")` return? *(None: it modifies the list in place.)*
9. How do you access the wage of the second labourer in a list of `[name, wage]` lists?
10. Why can a Python list store mixed types when an array in C can't? *(It stores references.)*

## 9. Practice questions

No loops yet: solve these with indexing, methods and `if`. Use `logger.info`.

**Easy**

- [ ] E1. Create a list of the 2 mistris and 4 labourers (6 names). Log the first, last and third names using positive indexes, then the same using negative indexes.
- [ ] E2. Start with an empty list `site_workers = []`. Add 3 names one at a time, then log the list and its length.
- [ ] E3. Predict, then run: `[1, 2].append([3, 4])` vs `[1, 2].extend([3, 4])`. What's the final list and length in each case?
- [ ] E4. Replace the second labourer's name with "Suresh" and log the list.

**Medium**

- [ ] M1. Insert "Ram" at the start, "Mohan" in the middle and "Sohan" at the end of `labour_names`, using `insert` only.
- [ ] M2. Take a list of materials `["cement", "bricks", "sand"]`. Ask the user for an index and log the material there; if the index is out of range, log a friendly message instead of crashing (use `len()` and `if`).
- [ ] M3. From `"s3://my-bucket/raw/sales/2026/11/04/orders.csv"`, extract the file name, the day and the year using `.split("/")` and negative indexes.
- [ ] M4. Build `labour_with_wage` for 4 labourers and log each person's monthly cost for 26 working days (one line each, indexing only).

**Hard**

- [ ] H1. Represent the house's rooms as a 2D list: `[["Bedroom", 12, 14], ["Kitchen", 10, 8], ...]` (name, length, breadth). Log each room's area and the total area of the first three rooms.
- [ ] H2. Write code that safely logs the last element of any list, logging "list is empty" when it is. Test it with `[]` and `["Ramesh"]`.
- [ ] H3. `a = ["Ramesh"]; b = a; b.append("Mahesh")`. What is `a` now? Explain using section 3, then find a way to make `b` a separate copy (hint: `.copy()`).

**External practice:** HackerRank Python → "Lists" (Basic Data Types section).

## 10. Your notes and corrections

Add your own notes here. When this chapter is final, tell me and I'll mark it **Final saved** in the roadmap tracker.

**Notes on the lecture**

- The lecture says list values are stored contiguously in RAM. In CPython, the contiguous part is an array of *references*; the values themselves can be anywhere (section 3).
- The wages for Mithilesh and Sumesh were unclear in the auto-generated transcript; 450 and 300 are illustrative values. Mahesh (500) and Ramesh (400) are from the lecture.
- His Azure URL example didn't come through clearly in the transcript, so section 5 uses a storage path example showing the same idea.
- He mentions "four data types" (int, str, bool, float); `None` and `complex` exist too (chapter 1).

**Added beyond the lecture:** `len()`, changing an element by index, `append` vs `extend` traps, methods returning `None`, the reference/copy note.

**Sources:** Manish Kumar, [List in Python | Lec-8](https://youtu.be/3v9f_KCCkrM) (transcript); [Python tutorial: lists](https://docs.python.org/3/tutorial/introduction.html#lists); [Python docs: more on lists](https://docs.python.org/3/tutorial/datastructures.html#more-on-lists).
