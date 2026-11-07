# Python 06: Lists (Part 2)

Nov 4, 2026 · @Mohammad Mauhid Usmani

Roadmap: DataPathsala IC study plan, Day 7. Video: [List in Python Part-2 | Lec-9](https://youtu.be/f5OHlEfKqeI).

## 1. Features of a list

| Feature | Meaning | Example |
| --- | --- | --- |
| Allows duplicates | The same value can appear many times | `[1, 2, 3, 3, 3, 3, 4, 8, 9, 5]` is valid |
| Mutable | Can be changed after creation (like a virus that mutates) | append, change `[1]` to 7, remove |
| Stores mixed types | int, str, float, bool together | `[1, "Manish", 2.5, True]` |
| Ordered | Keeps items in insertion order | see Lists Part 1, section 4 |

**Is a list a data type or a data structure?** Both, and an interviewer may ask this. In Python, everything has a type, so `type([])` is `<class 'list'>`: technically a data type. But conceptually a list is a **data structure**, because it organises a collection of values in a particular form (like an array in other languages).

A clean interview answer: *"`int`, `float`, `str` and `bool` are the basic (primitive) data types. A list is a built-in data type that works as a data structure, because it holds and organises a collection of values."*

## 2. Combining lists: append vs extend vs +

```python
labour_names = ["Ramesh", "Mahesh", "Mithilesh", "Sumesh"]
new_labour = ["Ram", "Mohan"]

print(labour_names.append(new_labour))   # None  ← append returns nothing
print(labour_names)
# ['Ramesh', 'Mahesh', 'Mithilesh', 'Sumesh', ['Ram', 'Mohan']]  ← nested, not merged
```

`append` adds its argument as **one** item, so a list goes in as a list. To merge, use `extend` or `+` (starting again from the original 4 names):

```python
labour_names = labour_names + new_labour
# ['Ramesh', 'Mahesh', 'Mithilesh', 'Sumesh', 'Ram', 'Mohan']
```

Manish says `+` is what you'll mostly see in production code. One real difference:

|  | `a.extend(b)` | `a + b` |
| --- | --- | --- |
| Changes `a` in place? | Yes | No: builds a **new** list |
| Returns | `None` | the new list |
| Need to reassign? | No | Yes: `a = a + b` (or `a += b`) |

**VS Code / Windows tip from the lecture:** `Win + V` opens clipboard history, so you can copy several snippets and paste whichever you need.

## 3. Slicing: getting a range with a colon

Slicing returns a **new list** with part of the original. Syntax: `my_list[start:end:step]`.

- `start` is **included**; default is 0 (the beginning).
- `end` is **excluded**; default is the end of the list. Think "up to, but not including".
- `step` is how far to jump; default 1.

In this lecture's list, names come first and wages after (Mahesh 500, Ramesh 400, Mithilesh 400, Sumesh 300; note the duplicate 400):

```python
labour_total = ["Mahesh", "Ramesh", "Mithilesh", "Sumesh", 500, 400, 400, 300]
#  index:          0         1          2           3       4    5    6    7
```

| &#32;Slice | Reads as | Result |
| --- | --- | --- |
| `labour_total[1:]` | index 1 to the end | `['Ramesh', 'Mithilesh', 'Sumesh', 500, 400, 400, 300]` |
| `labour_total[1:3]` | indexes 1 and 2 (3 excluded) | `['Ramesh', 'Mithilesh']` |
| `labour_total[0:1]` | index 0 only | `['Mahesh']` |
| `labour_total[0:2]` | indexes 0 and 1 | `['Mahesh', 'Ramesh']` |
| `labour_total[:4]` | start to index 3 | all four names |
| `labour_total[4:]` | index 4 to the end | `[500, 400, 400, 300]` (all wages) |
| `labour_total[-3:]` | last three | `[400, 400, 300]` |
| `labour_total[::2]` | every second item | `['Mahesh', 'Mithilesh', 500, 400]` |
| `labour_total[::-1]` | step backwards | the whole list reversed |
| `labour_total[15:]` | start beyond the end | `[]`, **no error** |

**Element number vs index:** Manish deliberately trips you up here. "From the 2nd element onwards" is `[1:]`, because the 2nd element is at index 1. In code, always think in indexes.

**Slicing never raises `IndexError`.** `labour_total[15]` crashes, but `labour_total[15:]` quietly returns `[]`. Handy, but it can hide bugs: an empty result may mean your indexes were wrong.

**Interview favourite: reverse a list.** `my_list[::-1]` returns a reversed copy. `my_list.reverse()` reverses the list in place and returns `None`.

## 4. len() and insert()

**`len(list)`** counts the elements. It counts normally (1, 2, 3, …), not by index:

```python
len(labour_total)   # 8  → valid indexes are 0 to 7
```

You'll use `len()` constantly: checking a list isn't empty, checking an index is in range, and in loops.

**`insert(index, value)`** puts a value at a given position. The values you pass to a method are called **arguments** (or parameters); `insert` takes two.

New labourer Ram joins at ₹600/day. To keep all names together, insert his name at index 4 (right after the names), then append his wage at the end:

```python
labour_total.insert(4, "Ram")
labour_total.append(600)
# ['Mahesh', 'Ramesh', 'Mithilesh', 'Sumesh', 'Ram', 500, 400, 400, 300, 600]
```

**Design note:** this flat layout only works if you keep names and wages perfectly in step; one wrong insert and every wage belongs to the wrong person. A list of `[name, wage]` pairs (Lists Part 1, section 7), or a dictionary (Day 12), is safer.

## 5. Removing items: pop, remove, del

| Tool | Removes | Returns | If it fails |
| --- | --- | --- | --- |
| `pop()` | the **last** item | the removed item | `IndexError` on an empty list |
| `pop(i)` | the item **at index i** | the removed item | `IndexError` if `i` is out of range |
| `remove(x)` | the **first occurrence** of value `x` | `None` | `ValueError: list.remove(x): x not in list` |
| `del my_list[i]` | the item at index i (or a slice) | nothing (it's a statement) | `IndexError` |
| `del my_list` | the **whole variable** | nothing | later use → `NameError` |

**pop returns what it removed**, so you can store and use it:

```python
labour_total = ["Mahesh", "Ramesh", "Mithilesh", "Sumesh", 500, 400, 400, 300]

wage = labour_total.pop()     # removes and returns 300
if wage < 500:
    logger.info("Cheaper labour")

name = labour_total.pop(2)    # removes and returns 'Mithilesh'
name < 500
# TypeError: '<' not supported between instances of 'str' and 'int'
```

That `TypeError` is common: comparing a string with a number. When you see it, check what type your variable actually holds.

**Where pop is really used: stacks and queues.** A **stack** is LIFO (last in, first out), like a pile of plates: `append()` to push, `pop()` to take the top. A **queue** is FIFO (first in, first out), like a ticket line: take from the front. `pop(0)` works for that, but it shifts every other item, so it's slow on big lists; real code uses `collections.deque` and its `popleft()`.

**remove deletes only the first match:**

```python
labour_total.remove(400)    # only the first 400 goes; the second one stays
labour_total.remove(1000)   # ValueError: list.remove(x): x not in list
```

Check first with `if 1000 in labour_total:` to avoid crashing (the `in` operator from chapter 3).

**Read the traceback.** When code crashes ("the code burst", as Manish says), the last lines tell you the file, the line number, the line of code and the error type. Read them before anything else.

**del removes the variable itself:**

```python
del labour_total
print(labour_total)     # NameError: name 'labour_total' is not defined
print("labour_total")   # fine: that's just a string
```

## 6. Changing values

Like correcting a misspelt name on an Aadhaar card: find the item's index, then assign the new value. The list is updated **in place**.

```python
labour_names = ["Mahe", "Rame", "Mithile", "Sume"]
labour_names[0] = "Mahesh"
# ['Mahesh', 'Rame', 'Mithile', 'Sume']
```

**Change several at once with slice assignment:**

```python
labour_names[0:4] = ["Mahesh", "Ramesh", "Mithilesh", "Sumesh"]   # end 4 is excluded, so 0–3
```

**Careful: the new list doesn't have to be the same length.** Slice assignment *removes* the slice and *inserts* the new items, so the list can shrink or grow:

```python
wages_and_names = ["Mahe", "Rame", "Mithile", "Sume", 500]
wages_and_names[0:4] = ["Mahesh"]
# ['Mahesh', 500]  ← four names replaced by one; the list shrank from 5 to 2
```

That's a silent data-loss bug: no error, just missing rows. Match the lengths unless you mean to resize.

## 7. split(): turning a string into a list

You rarely receive a ready-made list. You build one, often by splitting a string. `text.split(separator)` cuts the string wherever the separator appears and returns a list of the pieces.

```python
hash_code = "xyz-abc-51b-986-541-a21"
parts = hash_code.split("-")
# ['xyz', 'abc', '51b', '986', '541', 'a21']
parts[-1]   # 'a21'
```

**Real use: pulling values out of an API endpoint.** Cloud URLs follow a fixed pattern, so after splitting on `/` each piece sits at a predictable position:

```python
api_endpoint = ("https://management.azure.com/subscriptions/1234-abcd/resourceGroups/rg-dev"
                "/providers/Microsoft.Compute/virtualMachines/test-vm/getCredentials")
new_api_list = api_endpoint.split("/")

new_api_list[-1]   # 'getCredentials'  (the action)
new_api_list[-2]   # 'test-vm'         (the VM name)
new_api_list[4]    # '1234-abcd'       (the subscription ID)
```

Count from the back for things at the end, from the front for things at the start. Once it's a list, every method in this chapter applies. `split()` with no argument splits on any whitespace; it's covered fully in the String Methods lecture (Day 16).

## 8. Assignment: sort() and sorted()

Manish's assignment was to explore the sort method yourself. Try it before reading this section.

```python
wages = [500, 400, 400, 300]

print(wages.sort())            # None  ← sorts in place, returns nothing
print(wages)                   # [300, 400, 400, 500]

wages.sort(reverse=True)       # [500, 400, 400, 300]  (descending)

wages = [500, 400, 400, 300]
print(sorted(wages))           # [300, 400, 400, 500]  ← a NEW sorted list
print(wages)                   # [500, 400, 400, 300]  ← original unchanged
```

|  | `my_list.sort()` | `sorted(my_list)` |
| --- | --- | --- |
| Changes the original? | Yes, in place | No |
| Returns | `None` | a new list |
| Works on | lists only | any iterable (lists, tuples, strings, ...) |

**Gotchas (verified):**

- Mixed types can't be sorted: `["Mahesh", 500].sort()` → `TypeError: '<' not supported between instances of 'int' and 'str'`.
- Strings sort by character code, so uppercase comes before lowercase: `["mahesh", "Ramesh", "Sumesh"]` sorts to `['Ramesh', 'Sumesh', 'mahesh']`. Use `key=str.lower` to ignore case.

## 9. List cheat sheet (Parts 1 and 2)

Manish's estimate: these cover about 95% of what you'll do with lists.

| Task | Code | Changes list? | Returns |
| --- | --- | --- | --- |
| Add one item at end | `lst.append(x)` | Yes | `None` |
| Add many at end | `lst.extend(other)` or `lst + other` | extend: yes; +: no | `None` / new list |
| Add at position | `lst.insert(i, x)` | Yes | `None` |
| Remove last / at index | `lst.pop()` / `lst.pop(i)` | Yes | removed item |
| Remove by value | `lst.remove(x)` | Yes | `None` |
| Delete item / variable | `del lst[i]` / `del lst` | Yes | — |
| Change item | `lst[i] = x` | Yes | — |
| Get item | `lst[i]`, `lst[-1]` | No | the item |
| Get range | `lst[start:end:step]` | No | new list |
| Reverse | `lst[::-1]` / `lst.reverse()` | no / yes | new list / `None` |
| Sort | `sorted(lst)` / `lst.sort()` | no / yes | new list / `None` |
| Count items | `len(lst)` | No | int |
| Check membership | `x in lst` | No | bool |
| String to list | `text.split(sep)` | — | new list |

**Pattern to remember:** methods that change the list in place return `None`. Never write `lst = lst.append(x)` or `lst = lst.sort()`.

## 10. Interview questions

1. What are the main features of a Python list?
2. Is a list a data type or a data structure? Justify your answer.
3. Difference between `append()`, `extend()` and `+`?
4. How do you reverse a list? Give two ways and say how they differ.
5. What does `lst[2:5]` return? Is index 5 included?
6. Why does `lst[100]` raise an error but `lst[100:]` doesn't?
7. Difference between `pop()`, `remove()` and `del`?
8. What does `pop()` return? Why is that useful?
9. What is a stack? A queue? Which list operations implement each?
10. Difference between `sort()` and `sorted()`?
11. What happens with `lst[0:4] = ["x"]`?
12. How would you get the last part of a URL in Python?

## 11. Practice questions

Still no loops: use indexing, slicing, methods and `if`. Use `logger.info`.

**Easy**

- [x] E1. From `["Mahesh", "Ramesh", "Mithilesh", "Sumesh", 500, 400, 400, 300]`, get just the names and just the wages using slicing.
- [x] E2. Reverse the list `[1, 2, 3, 4, 5]` two ways. Log the original after each and explain the difference.
- [x] E3. Remove the duplicate `400` so only one remains, using `remove`. Log the list.
- [x] E4. Fix the typo list `["Mahe", "Rame"]` to `["Mahesh", "Ramesh"]` with one slice assignment.

**Medium**

- [x] M1. Sort `[500, 400, 600, 300]` ascending and descending using both `sort()` and `sorted()`.
- [x] M2. Pop the last wage from the list; log "costly labour" if it's over 500, otherwise "affordable".
- [x] M3. Ask the user for a value to remove from a list. Remove it if present, otherwise log "not found" (no crash).
- [x] M4. From `"s3://datalake/raw/orders/year=2026/month=11/day=05/part-0001.parquet"`, extract the bucket name, the file name and the `month=11` part.

**Hard**

- [ ] H1. Simulate a stack of 3 task names: push them with `append`, then pop them one by one, logging each. Note the order. Then simulate a queue with `collections.deque`.
- [ ] H2. Given `[3, 1, 4, 1, 5, 9, 2, 6]`, log the second-largest value using sorting and indexing.
- [ ] H3. Without running it, predict the list after `a = [1, 2, 3, 4, 5]; a[1:4] = [9]`. Then run it and explain.

**External practice:** HackerRank Python → "Lists", "Find the Runner-Up Score!".

## 12. Your notes and corrections

Add your own notes here. When this chapter is final, tell me and I'll mark it **Final saved** in the roadmap tracker.

**Notes on the lecture**

- In the slice-assignment demo (`[0:4] = ["Mahesh"]`), the lecture says Python "replaced only index 0 and ignored the rest". Running it shows the four items are *removed* and replaced by one, so the list shrinks (section 6).
- The lecture mixes up the queue terms. A stack is LIFO; a queue is FIFO (section 5).
- His `[-2]` "subscription ID" example depends on the exact URL; with the standard Azure resource URL in section 7, the subscription ID is at a fixed front index instead.
- He says `+` is more common than `extend` in production; that's his experience. Note that `+` builds a new list while `extend` changes the existing one.
- Ram's wage came out as "00" in the auto-generated transcript; 600 is used here.

**Added beyond the lecture:** slice `step`, `reverse()`, slicing returns a copy, `del lst[i]`, `collections.deque`, the `in` check before `remove`, `sorted()` vs `sort()` details, case-sensitive sorting, and the cheat sheet.

**Sources:** Manish Kumar, [List in Python Part-2 | Lec-9](https://youtu.be/f5OHlEfKqeI) (transcript); [Python docs: more on lists](https://docs.python.org/3/tutorial/datastructures.html#more-on-lists); [Python docs: sorting HOW TO](https://docs.python.org/3/howto/sorting.html).
