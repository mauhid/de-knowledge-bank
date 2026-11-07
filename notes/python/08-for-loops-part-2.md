# Python 08: For Loops (Part 2)

Nov 7, 2026 · @Mohammad Mauhid Usmani

Roadmap: DataPathsala IC study plan, Day 9. Video: [For Loop in Python Part-2 | Lec-11](https://youtu.be/2pYUYDblNpk).

This lecture has no new syntax beyond `continue`, `break` and `+=`. It's about **applying** the `for` loop to two real problems, and about the habit Manish keeps pushing: **write the algorithm on paper first, then code it.**

## 1. Problem 1: how many times does "the" appear in a paragraph?

**Why it matters:** in real life nobody hands you a ready-made list. Data arrives as a string, a file, an API response, and you first have to *turn it into* a list you can loop over.

**The algorithm (plan before code):**

1. Create a `count` variable, starting at 0 (if "the" never appears, the answer should be 0).
2. Assign the paragraph to a variable (a string).
3. Convert the string into a list of words.
4. Loop over the list.
5. For each word, check: is it "the"? If yes, increase `count`. If no, do nothing.
6. After the loop, report `count`.

Manish first forgot step 1 and only noticed it when he reached "increase count". That's normal: you go back and add the missing step. Plans get fixed as you write them.

The lecture used a paragraph about Ralph Kimball (a pioneer of data warehousing) shown on screen. It isn't in the transcript, so the paragraph below is **illustrative**, written in our house-building story. Like his, it has 8 "the"s, one of them written "The".

```python
from loguru import logger

count = 0
paragraph = ("Jagmohan laid the foundation of the house in March. The walls came next, "
             "built by Ram Pyare with the bricks that the labourers carried. Mahesh mixed "
             "the cement and Ramesh checked the plan every evening with the owner.")

paragraph_lst = paragraph.split(" ")
logger.info(paragraph_lst)

for word in paragraph_lst:
    if word == "the":
        count = count + 1
    else:
        continue

logger.info(f"Total count for the article is {count}")
```

```text
INFO     | __main__:<module>:9 - ['Jagmohan', 'laid', 'the', 'foundation', 'of', 'the', 'house', 'in', 'March.', 'The', 'walls', 'came', 'next,', 'built', 'by', 'Ram', 'Pyare', 'with', 'the', 'bricks', 'that', 'the', 'labourers', 'carried.', 'Mahesh', 'mixed', 'the', 'cement', 'and', 'Ramesh', 'checked', 'the', 'plan', 'every', 'evening', 'with', 'the', 'owner.']
INFO     | __main__:<module>:17 - Total count for the article is 7
```

Notes on the code:

- `split(" ")` cuts the string at every space and returns a list (chapter 6, section 7).
- **Log the intermediate result.** Manish logs the list straight after splitting to confirm it looks the way he expects. Do this every time you transform data.
- `==` compares, `=` assigns (chapter 3).
- The list gets its own name, `paragraph_lst`, so the code says what each variable holds.

**The bug: we expected 8 and got 7.** Look at the logged list: one word is `'The'` with a capital T. Python is **case-sensitive**: `"The" == "the"` is `False`, so that word was skipped. (In the lecture Manish had miscounted by hand too, expecting 7. His program printed 7 and seemed right until he recounted and found 8. A matching answer doesn't prove the code is right.)

### Fix 1: add another condition

```python
for word in paragraph_lst:
    if word == "the":
        count += 1
    elif word == "The":
        count += 1
# Total count for the article is 8
```

`count += 1` is shorthand for `count = count + 1`. You'll see it everywhere.

It works, but what about `"THE"`, `"tHe"`, `"thE"`...? Eight spellings, eight conditions. Writing an `elif` for each would take all day.

### Fix 2: make everything lowercase first

```python
paragraph_lst = paragraph.lower().split(" ")
logger.info(paragraph_lst[:12])

for word in paragraph_lst:
    if word == "the":
        count += 1

logger.info(f"Total count for the article is {count}")
```

```text
INFO     | __main__:<module>:9 - ['jagmohan', 'laid', 'the', 'foundation', 'of', 'the', 'house', 'in', 'march.', 'the', 'walls', 'came']
INFO     | __main__:<module>:15 - Total count for the article is 8
```

**Order matters:** `.lower()` is a **string** method, so call it before `split`. Manish first tried it on the list and got an error (verified):

```python
paragraph_lst.lower()
# AttributeError: 'list' object has no attribute 'lower'
```

`paragraph.lower().split(" ")` is **method chaining**: `lower()` returns a new string, and `split` runs on that.

The lesson: there are usually several solutions. When one needs a new line for every variation, look for one that removes the variation (here, normalising the case).

## 2. continue and break

Two keywords that control a loop from inside:

| Keyword | Effect |
| --- | --- |
| `continue` | skip the **rest of this iteration** and go to the next item |
| `break` | stop the **whole loop** immediately; the program continues after the loop |

```python
wages = [500, 400, 0, 300, 600]

for wage in wages:
    if wage == 0:
        continue                      # skip just this one
    logger.info(f"Paying {wage}")

for wage in wages:
    if wage == 0:
        logger.info("Found a missing wage, stopping")
        break                         # leave the loop entirely
    logger.info(f"Paying {wage}")
logger.info("After the loop")
```

```text
INFO     | __main__:<module>:7 - Paying 500
INFO     | __main__:<module>:7 - Paying 400
INFO     | __main__:<module>:7 - Paying 300
INFO     | __main__:<module>:7 - Paying 600
INFO     | __main__:<module>:13 - Paying 500
INFO     | __main__:<module>:13 - Paying 400
INFO     | __main__:<module>:11 - Found a missing wage, stopping
INFO     | __main__:<module>:14 - After the loop
```

In Problem 1, the `else: continue` does nothing useful: `continue` was already the last statement of the iteration, so removing the `else` gives the same result. That's why Fix 2 drops it.

## 3. Problem 2: insert into a sorted list, without built-ins

We have a list sorted in increasing order and a new number. Put the number in so the list **stays sorted**:

```text
lst = [5, 18, 77, 108, 930], number_to_insert = 100
expected: [5, 18, 77, 100, 108, 930]
```

**The shortcut an interviewer will reject:**

```python
lst.append(100)
lst.sort()
# [5, 18, 77, 100, 108, 930]
```

It gives the right answer, and at work, without a strict code review, it might pass. But in an interview you'll be told: no `sort()`, no `insert()`, no libraries. Manish's warning: people who lean on built-in functions get stuck exactly there. It's also wasteful: sorting the whole list costs more than it needs to (he promises time complexity later: sorting is O(n log n); the loop approach below is O(n)).

**Think in memory slots.** Picture the list in RAM as five boxes: `5 | 18 | 77 | 108 | 930`. 100 belongs at index 3, but if you just write `lst[3] = 100` you **overwrite** 108. So:

1. Add one empty box at the end.
2. Move 930 one box right, then 108 one box right. Index 3 is now free.
3. Write 100 into index 3.

**The algorithm:**

- **Part 1, find the index:** create `index = 0`. Loop over the list. If the current number is bigger than `number_to_insert`, we've found the spot: stop. Otherwise add 1 to `index`.
- **Part 2, shift and write:** append an empty slot. Loop **backwards** from the last index down to (but not including) `index`, copying each item from its left neighbour. Then write the new number at `index`.

```python
from loguru import logger

lst = [5, 18, 77, 108, 930]
number_to_insert = 100

# Part 1: find the index where the new number belongs
index = 0
for number in lst:
    if number > number_to_insert:
        break
    else:
        index = index + 1
logger.info(f"index = {index}")

# Part 2: make room at the end, shift items right, write the new value
lst.append(None)
logger.info(lst)
for i in range(len(lst) - 1, index, -1):
    lst[i] = lst[i - 1]
    logger.info(f"i = {i}: {lst}")
lst[index] = number_to_insert
logger.info(lst)
```

```text
INFO     | __main__:<module>:13 - index = 3
INFO     | __main__:<module>:17 - [5, 18, 77, 108, 930, None]
INFO     | __main__:<module>:20 - i = 5: [5, 18, 77, 108, 930, 930]
INFO     | __main__:<module>:20 - i = 4: [5, 18, 77, 108, 108, 930]
INFO     | __main__:<module>:22 - [5, 18, 77, 100, 108, 930]
```

### Dry run of Part 1

A **dry run** means tracing the code by hand, one iteration at a time, writing down each variable's value. Manish does it on screen; do it on paper.

| Iteration | `number` | `number > 100`? | Action | `index` after |
| --- | --- | --- | --- | --- |
| 1 | 5 | False | `index + 1` | 1 |
| 2 | 18 | False | `index + 1` | 2 |
| 3 | 77 | False | `index + 1` | 3 |
| 4 | 108 | True | `break` | 3 |

**Why `break`?** The list is sorted, so once we see a number bigger than 100, every number after it is bigger too. Without `break` the result would still be 3 (the `if` branch doesn't change `index`), but the loop would keep comparing for nothing. Every comparison costs CPU time, and at a company, CPU time is server cost.

### Dry run of Part 2

After `append(None)` the list has 6 items, so `len(lst) - 1` is 5, the last index. `range(5, 3, -1)` counts down and stops **before** 3: it gives `5, 4`. (Start included, end excluded, as in chapter 7; step `-1` goes backwards.)

| `i` | Line runs as | List after |
| --- | --- | --- |
| 5 | `lst[5] = lst[4]` (930) | `[5, 18, 77, 108, 930, 930]` |
| 4 | `lst[4] = lst[3]` (108) | `[5, 18, 77, 108, 108, 930]` |
| then | `lst[3] = 100` | `[5, 18, 77, 100, 108, 930]` |

**Reading `lst[i] = lst[i - 1]`:** the left side is the box being **written**; the right side is where the value is **read** from. So `lst[4] = lst[3]` copies 108 *into* index 4; it doesn't move index 4's value anywhere. Manish flags this as a common confusion.

**Why backwards?** If you shifted from the left, `lst[4] = lst[3]` would run first and overwrite 930 before it was copied. Going from the end, each value is copied before its box is reused.

### Edge cases (corner cases)

Interviewers often ask: "Should I handle edge cases?" If yes, think about unusual inputs. Running the same code with other inputs (verified):

| Input list | Insert | Result | Comment |
| --- | --- | --- | --- |
| `[5, 18, 77, 108, 930]` | 1077 | `[5, 18, 77, 108, 930, 1077]` | bigger than everything |
| `[5, 18, 77, 108, 930]` | 3 | `[3, 5, 18, 77, 108, 930]` | smaller than everything |
| `[]` | 100 | `[100]` | empty list |
| `[5, 18, 77, 108, 930]` | 77 | `[5, 18, 77, 77, 108, 930]` | duplicate: goes after the existing 77 |
| `[-40, -5, 0, 12]` | -10 | `[-40, -10, -5, 0, 12]` | negative numbers |

The code already gives the right answer in all of these. Manish's point about edge cases is **efficiency**: for 1077, compare with the last item first; if it's bigger, just append, without looping through the whole list. For 3, it belongs at index 0 straight away. Checking the cheap special cases first saves work on big lists.

## 4. Syntax vs real-world programming

Manish closes by saying the earlier lectures taught syntax; this one is the start of real programming: plan, dry run, test with odd inputs, and think about cost. Time complexity comes later in the course.

## 5. Interview questions

1. What's the difference between `break` and `continue`?
2. Why did `word == "the"` miss `"The"`? How do you make a comparison case-insensitive?
3. Why does `paragraph_lst.lower()` fail but `paragraph.lower().split()` work?
4. What does `count += 1` mean?
5. Insert a number into a sorted list without using `sort()`, `insert()` or any library. Walk through your approach.
6. When shifting items to make room, why loop from the end instead of the start?
7. What does `range(5, 3, -1)` produce?
8. What is a dry run? What is an edge case? Give three edge cases for inserting into a sorted list.
9. Why is `append` + `sort` a poor answer for inserting into a sorted list, even though it works?
10. Why might `split(" ")` be a bad choice for real text? (Hint: newlines, double spaces, punctuation.)

## 6. Practice questions

Use `for` loops; plan on paper first and dry run before you execute. Use `logger.info`. No built-ins that do the whole job for you (`sum`, `max`, `sort`, `count`, ...) unless the question allows it.

**Easy**

- [ ] E1. Count how many labourers in `["Mahesh", "Ramesh", "Mithilesh", "Sumesh"]` have a name ending in `"esh"`.
- [ ] E2. Count the words in a sentence of your choice using `split` and a loop (no `len`).
- [ ] E3. From `[500, 400, 0, 300, 0, 600]`, log only the non-zero wages using `continue`.
- [ ] E4. Find the first wage above 450 in `[300, 400, 500, 600]`, log it, and stop the loop with `break`.

**Medium**

- [ ] M1. Count how many times "brick" appears in a paragraph, ignoring case. Make your paragraph include `"Brick"` and `"BRICKS"`; decide whether "bricks" should count and explain your choice.
- [ ] M2. Find the largest daily wage in `[500, 400, 650, 300]` without `max()` or sorting.
- [ ] M3. Given a sorted list, find the index where a new number should go (Part 1 only), and log it. Test with the edge cases from section 3.
- [ ] M4. Add the edge-case shortcuts to the insert program: if the list is empty or the number is bigger than the last item, append directly; if smaller than the first, put it at index 0 (still without `insert()`).

**Hard**

- [ ] H1. Insert into a list sorted in **decreasing** order, e.g. `[930, 108, 77, 18, 5]` with 100, without built-ins.
- [ ] H2. Delete the value at a given index from a list by shifting items left, then removing the last slot with `pop()` (no `del`, no `remove`). Dry run it first.
- [ ] H3. Count "the" in a multi-line paragraph (triple quotes) that also contains "the," and "the." and "there". Get the right answer and explain each problem you hit.

**External practice:** HackerRank Python → "Loops", "Find a string". LeetCode 35 "Search Insert Position" (Part 1 of Problem 2).

## 7. Notes and corrections

Add your own notes here. When this chapter is final, tell me and I'll mark it **Final** in the README.

**Notes on the lecture**

- He describes `continue` as "carry on, there's no obstacle, go ahead and run the lines after it". It's the opposite: `continue` **skips** the remaining lines of the current iteration and jumps to the next item. In his program it sat at the end of the `else`, so it had no visible effect (section 2).
- **His edge cases aren't bugs in this code.** He presents 1077 (bigger than all), 3 (smaller than all) and the empty list as cases the program must handle. Running it shows it already handles them correctly; checking them first is an *efficiency* improvement, not a correctness fix (section 3).
- In Part 1 he writes `index = index` in the `if` branch before adding `break`. That line does nothing; `break` alone is enough.
- He uses `number` as the loop variable in Part 1, then switches to `i` in Part 2 because `number` would be confusing for an index. Good instinct: name variables by what they hold.
- His time-complexity remark ("sorting is O(n log n), ours is O(n)") is previewed only; it's covered later in the course.
- The paragraph is illustrative (see section 1). His live-stream announcement is left out.

**Pitfalls of `split(" ")` (beyond the lecture, verified):**

```python
text = """Ramesh carried the
bricks to the site. Then he went home, tired, after the"""

text.split(" ")
# ['Ramesh', 'carried', 'the\nbricks', 'to', 'the', 'site.', 'Then', ...]   ← 'the\nbricks' is one "word"
text.lower().split()
# ['ramesh', 'carried', 'the', 'bricks', 'to', 'the', 'site.', 'then', ...]  ← no-argument split handles newlines
text.lower().count("the")
# 4   ← counts "then" too: str.count matches substrings, not words
```

- `split()` with **no argument** splits on any run of whitespace (spaces, tabs, newlines) and is usually what you want for text. Triple-quoted paragraphs contain newlines, so `split(" ")` glues words across lines.
- Punctuation sticks to words: `"the,"` ≠ `"the"`. `word.strip(".,!?")` removes it from both ends.
- `str.count("the")` is not a word count; it finds "the" inside "then", "there", "other".

**Added beyond the lecture:** the `break`/`continue` wage example, the backwards-shift explanation, the duplicate and negative-number tests, the `split(" ")` pitfalls, LeetCode 35. In real code (not interviews), Python's `bisect.insort(lst, 100)` inserts into a sorted list for you.

**Sources:** Manish Kumar, [For Loop in Python Part-2 | Lec-11](https://youtu.be/2pYUYDblNpk) (transcript); [Python docs: break and continue](https://docs.python.org/3/tutorial/controlflow.html#break-and-continue-statements); [Python docs: str.split](https://docs.python.org/3/library/stdtypes.html#str.split); [Python docs: bisect](https://docs.python.org/3/library/bisect.html).
