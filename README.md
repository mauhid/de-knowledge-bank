# Data Engineering Knowledge Bank

Text-first notes for learning data engineering, built lecture by lecture while following
Manish Kumar's roadmap. These notes will become the content for a free learning website:
chapters, practice questions and interview questions, tagged by topic and level.

## Structure

```text
roadmap/   the full roadmap, organised from Manish Kumar's "data engineer roadmap 2026" video
notes/     one chapter per lecture, grouped by topic (python/, sql/, spark/, ...)
transcripts/  auto-generated lecture transcripts, one file per video (private study copy only)
scripts/   fetch_transcripts.py downloads transcripts (run on a home connection; YouTube blocks cloud servers)
CLAUDE.md  workflow instructions so any Claude session (desktop or mobile) can continue the work
```

## Progress

The study plan we follow is the DataPathsala IC (2-4 YOE) roadmap:
https://datapathsala.com/study-plans/data-engineering-roadmap-ic

| Day | Topic | Lecture | Chapter | Status |
| --- | --- | --- | --- | --- |
| 1 | Python setup & first script | — | not written yet | To do |
| 2 | Variables & data types | Lec-4 | [01](notes/python/01-variables-and-data-types.md) | Draft |
| 3 | print, string formatting, escape sequences, logging | Lec-5 | [02](notes/python/02-print-string-formatting-logging.md) | Draft |
| 4 | Operators, type casting, user input | Lec-6 | [03](notes/python/03-operators-type-casting-user-input.md) | Draft |
| 5 | if / elif / else | Lec-7 | [04](notes/python/04-if-elif-else.md) | Draft |
| 6 | Lists, part 1 | [Lec-8](https://youtu.be/3v9f_KCCkrM) | [05](notes/python/05-lists-part-1.md) | Draft |
| 7 | Lists, part 2 | [Lec-9](https://youtu.be/f5OHlEfKqeI) | [06](notes/python/06-lists-part-2.md) | Draft |
| 8 | For loops, part 1 (+ DSA: check if array is sorted) | [Lec-10](https://youtu.be/5J_xH6X-qGw) | — | Next |

Status values: **To do** → **Draft** (written, not yet reviewed) → **Final** (reviewed and corrected).

## Chapter format

Every chapter follows the same template so it can be turned into a web page later:

1. Concept sections with tested code examples (the house-building example runs through the series)
2. Interview questions
3. Practice questions (easy / medium / hard) with checkboxes
4. Notes and corrections: where the lecture was imprecise, what was added beyond it, and sources

## Credits

Roadmap and lecture content: [Manish Kumar](https://www.youtube.com/@manish_kumar_1) and
[DataPathsala](https://datapathsala.com). These are personal study notes written from his lectures;
the transcripts in `transcripts/` are a private study copy and must not be published.
