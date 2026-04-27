---
name: progress
description: Check the progress and content coverage of a specific course in this Obsidian semester vault. Use when the user asks for course status, missing notes, exercise completion, index completeness, or overall progress for web-engineering, kommunikationssysteme, numerik, csharp, data-science, or large-scale-it.
---

# Progress

Analyze the progress for the course named by the user.

## Course Mapping

Map the user input to one of:

- `web-engineering`
- `kommunikationssysteme`
- `numerik`
- `csharp`
- `data-science`
- `large-scale-it`

If the input does not match exactly, suggest the closest valid course.

## Checks

Run these checks and present a structured overview.

### 1. Resources Coverage

- List files in `<course>/resources/`
- For each PDF or slide deck, check whether a corresponding lecture note exists in `<course>/lectures/`
- Report total resources, how many appear covered by notes, and which resources are missing notes

### 2. Lecture Notes

- Count lecture subfolders in `<course>/lectures/`
- List lecture note files and their `lecture` number and `date` from frontmatter
- Report the total number of lectures covered

### 3. Lernziele or Selbstkontrolle Coverage

- For `web-engineering`, inspect `<course>/lernziele/` and check whether each file has `lecture` and `date` frontmatter and whether matching lecture notes include a `Bezug zu Lernzielen` section
- For `kommunikationssysteme`, inspect `<course>/selbstkontrolle/` and check whether each file has `lecture` and `date` frontmatter and whether matching lecture notes include a `Fragen zur Selbstkontrolle` section
- For `data-science`, inspect `<course>/selbstkontrolle/` and check whether each file has `lecture` and `date` frontmatter and whether matching lecture notes include a `Fragen zur Selbstkontrolle` section
- For other courses, skip this section

### 4. Exercises

- Count exercise subfolders in `<course>/exercises/`
- For each exercise folder, check whether both the exercise note and solution note exist and whether they have `lecture` and `date` frontmatter
- Report totals and list missing solutions

### 5. Course Index Completeness

- Read `<course>/<prefix>-index.md`
- Check whether existing lectures, exercises, lernziele or selbstkontrolle files, and relevant resources are linked there
- Report items that exist on disk but are not linked in the index

### 6. Repos

- If `<course>/repos/` exists, list the repositories there

## Recommended Workflow

Prefer fast shell inspection:

- Use `rg --files` to list files
- Use `rg` to locate headings and wikilinks
- Read only the files needed to confirm gaps

Use the project rules in [AGENTS.md](../../../AGENTS.md) for course names, note conventions, and file structure.

## Output Shape

Start with a compact summary:

```text
Course: <name>
Weeks of material: X lectures
Resources: X total, Y with notes, Z missing
Lernziele/Selbstkontrolle: X of Y covered
Exercises: X total, Y with solutions
Index completeness: X of Y items linked
Repos: X repositories
```

Then list concrete gaps:

- missing notes
- missing solutions
- unlinked items
- section-specific coverage issues

If matching is ambiguous, state the assumption you used.
