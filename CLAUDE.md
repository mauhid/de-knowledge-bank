# Instructions for Claude

This repo is Mohsin's data engineering knowledge bank. We go through the DataPathsala study plan
one lecture at a time and write one chapter per lecture. The chapters will later become a free
learning website, so keep them consistent.

## Source of truth for the order

- Study plan: https://datapathsala.com/study-plans/data-engineering-roadmap-ic (180 days).
  Each day lists a YouTube video (Manish Kumar, in Hindi) and sometimes a DSA problem on
  datapathsala.com. Follow the days in order; the README progress table shows where we are.
- The page is client-rendered: plain web fetch returns nothing useful. Open it in a browser
  if one is available, click the day, and read the WATCH and SOLVE links.

## Getting the lecture content

1. Preferred: the YouTube auto-generated Hindi transcript ("Show transcript" under the video
   description). It only loads when the browser is visible.
2. If no transcript can be fetched, ask Mohsin to paste it, or (with his OK) draft the chapter
   from the day's topic and mark it clearly as "written without the transcript" so it can be
   checked against the lecture later.
3. Never commit transcripts to the repo (they are Manish's content). Commit only our notes.

## Writing a chapter

- File: `notes/<topic>/<NN>-<slug>.md`, numbered in order (python chapters are 01, 02, ...).
- Header: title `# Python NN: <Topic>`, then a line with the roadmap day and the video link.
- Keep Manish's running example: building a 4BHK house on a 100 × 100 ft plot, mistris
  Jagmohan and Ram Pyare, labourers Ramesh, Mahesh, Mithilesh, Sumesh, wages, bricks at 10.5.
- Use `logger.info` (loguru) instead of `print` in examples, as the lectures do from chapter 3 on.
- Run every code example and paste the real output; never guess outputs.
- Correct the lecture where it is imprecise, but say so in the "Notes and corrections" section.
  If the transcript is garbled, say a value is illustrative rather than inventing it as his.
- Sections: concepts → interview questions → practice questions (easy/medium/hard, checkboxes,
  no answers) → notes and corrections → sources.
- Mohsin learns by doing: don't solve practice questions for him unless he asks; review his
  solutions when he shares them.

## After each chapter

- Add or update the row in the README progress table (status Draft).
- When Mohsin says a chapter is final, change its status to Final.
- Commit with a short message, e.g. `Add Python 07: For Loops (Part 1)`.
