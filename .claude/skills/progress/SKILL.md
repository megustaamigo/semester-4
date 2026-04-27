---
name: progress
description: Check the progress of a specific course in the Obsidian vault
disable-model-invocation: true
allowed-tools: Bash Read Glob Grep
argument-hint: <course-name>
---

Analyze the progress for the course: $ARGUMENTS

Do the following checks and present a structured overview:

## 1. Identify the Course
Map the argument to one of the valid courses in CLAUDE.md:
- `web-engineering`, `kommunikationssysteme`, `numerik`, `csharp`, `data-science`, `large-scale-it`
If the argument doesn't match, suggest the closest match.

## 2. Resources Coverage
- List all files in `<course>/resources/`
- For each resource (PDF/slide), check if a corresponding lecture note exists in `<course>/lectures/`
- Report: total resources, how many have matching notes, which ones are **missing notes**

## 3. Lecture Notes
- Count lecture subfolders in `<course>/lectures/` (each subfolder = 1 week/lecture)
- List all lecture note files with their `lecture` number and `date` from frontmatter
- Report the total number of lectures covered

## 4. Lernziele / Selbstkontrolle Coverage
- For web-engineering: check `<course>/lernziele/` — list which lernziele files exist, whether they have `lecture` and `date` frontmatter, and whether the corresponding lecture notes have a "Bezug zu Lernzielen" section
- For kommunikationssysteme: check `<course>/selbstkontrolle/` — list which selbstkontrolle files exist, whether they have `lecture` and `date` frontmatter, and whether the corresponding lecture notes have a "Fragen zur Selbstkontrolle" section
- For data-science: check `<course>/selbstkontrolle/` — list which selbstkontrolle files exist, whether they have `lecture` and `date` frontmatter, and whether the corresponding lecture notes have a "Fragen zur Selbstkontrolle" section
- For other courses: skip this check

## 5. Exercises
- Count exercise subfolders in `<course>/exercises/`
- For each exercise subfolder, check if both the exercise and solution notes exist and whether they have `lecture` and `date` frontmatter
- Report: total exercises, how many have solutions, which are **missing solutions**

## 6. Course Index File
- Read the course index file (`<course>/<prefix>-index.md`)
- Check if all lectures, exercises, lernziele/selbstkontrolle, and resources are linked
- Report any notes that exist on disk but are **not linked** in the index

## 7. Repos (if applicable)
- Check if `<course>/repos/` exists and list any cloned repositories

## Output Format
Present the results as a clean summary table/report:

```
Course: <name>
Weeks of material: X lectures
Resources: X total, Y with notes, Z missing
Lernziele/Selbstkontrolle: X of Y covered
Exercises: X total, Y with solutions
Index completeness: X of Y items linked
Repos: X repositories
```

Then list specific gaps (missing notes, missing solutions, unlinked items) so the user knows exactly what still needs to be done.
