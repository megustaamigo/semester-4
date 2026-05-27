---
name: correct-homework
description: Correct Linear Algebra student homework solutions for a given week. Reads the exercise sheet (tasks 5-8 only), the per-task sample solutions, and every student PDF, then writes one concise correction Markdown per student that pinpoints exactly where each error occurs and grades each task 0-3. Use when asked to "correct homework", "/correct-homework", or to grade the linear-algebra-homework student solutions for a week.
allowed-tools: Bash Read Write Glob
argument-hint: <week-number>   e.g. 7
---

Correct the Linear Algebra homework for **week $ARGUMENTS**.

The course lives at `/home/samuel/obsidian/semester-4/linear-algebra-homework/`. The target week folder is `linear-algebra-homework/week-$ARGUMENTS/` with these subfolders:

- `exercises/` — the exercise sheet PDF (e.g. `LA2Blatt07.pdf`)
- `sample-solutions/` — one PDF per corrected task (`..._A5_Lsg.pdf`, `A6`, `A7`, `A8`)
- `student-solutions/` — one PDF per student (`la2_<surname>_<firstname>_h<NN>.pdf`)
- `corrections/` — **output**: one Markdown per student (created by this skill)

## Scope

**Only tasks 5, 6, 7, and 8 are corrected.** Ignore tasks 1-4 entirely, even if a student solved them.

## Procedure

1. **Locate inputs.** `ls` the three input folders. If the exercise sheet or any of the four sample solutions is missing, stop and tell the user what is missing — do not guess the task content.

2. **Read the ground truth first.** Read the exercise sheet (tasks 5-8 only) and all four sample solutions (`A5`-`A8`). These define the correct method and the correct final result for each task. PDFs may be over 10 pages — read in page-range chunks (max 20 pages/request).

3. **Read each student PDF.** Student solutions are usually **scanned/handwritten** — read them as images and work carefully through each task's steps. A large PDF may need several page-range reads.

4. **Per student, evaluate each task (5-8):**
   - Is the **final outcome** correct (matches the sample solution)?
   - Is the **way / method** correct (valid approach, correct intermediate steps)?
   - If there is an error, **pinpoint it exactly** — name the step, line, or computation where it first goes wrong (e.g. "Task 6, beim Einsetzen in die zweite Gleichung: $3\cdot4=12$ statt korrekt $9$, dadurch falsches $x_2$"). This precise localization is the most important part of the output.

5. **Grade each task 0-3** (max 12 per sheet):
   - **3** — outcome *and* way fully correct.
   - **2** — way/approach correct but wrong outcome (e.g. a late arithmetic slip), or essentially correct with a minor gap.
   - **1** — only the outcome is right but the way is missing/wrong, *or* only partial progress on the way.
   - **0** — not attempted, or fundamentally wrong.
   - Use judgement for in-between cases; be reasonable and consistent across students.

6. **Write one correction file** per student to `corrections/<same-basename-as-pdf>.md`. Be **precise, not verbose** — the value is in the exact error location, not in restating the solution. Use this template:

```markdown
# Korrektur — <Vorname Nachname> (Woche $ARGUMENTS)

**Gesamt: X / 12 Punkte**

| Aufgabe | Punkte | Weg | Ergebnis |
|---|---|---|---|
| 5 | x/3 | korrekt / fehlerhaft | korrekt / falsch |
| 6 | x/3 | ... | ... |
| 7 | x/3 | ... | ... |
| 8 | x/3 | ... | ... |

## Aufgabe 5 — x/3
<1-3 sentences. If correct: "Korrekt." If error: state EXACTLY where the mistake is — step/line/computation — and what the correct value/step would be.>

## Aufgabe 6 — x/3
...

## Aufgabe 7 — x/3
...

## Aufgabe 8 — x/3
...
```

Keep each task comment short. Only expand when needed to localize the error unambiguously. Write maths in `$...$` LaTeX so it renders in Obsidian.

7. **If a student PDF is unreadable** (corrupt, blank, wrong sheet), still create the file noting the problem and leave grading open — do not invent a grade. List such cases in the final summary.

8. **When all students are done, notify the user** with a compact summary: a table of student → total points (X/12), plus any students that need manual attention (unreadable PDF, ambiguous case). Do not push or commit unless asked.

## Notes

- Process students in a stable order (alphabetical by filename).
- Do not modify the exercise sheet, sample solutions, or student PDFs — only write into `corrections/`.
- One correction file per student; overwrite if re-run for the same week.
