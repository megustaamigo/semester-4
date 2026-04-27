---
tags: [dashboard, semester-4]
---
# Semester 4 Dashboard

## Courses

- [[web-engineering/webeng-index|Web Engineering]]
- [[kommunikationssysteme/komsys-index|Kommunikationssysteme]]
- [[numerik/num-index|Numerik]]
- [[csharp/cs-index|C#]]
- [[data-science/ds-index|Introduction to Data Science]]
- [[large-scale-it/lsit-index|Large Scale IT and Cloud Computing]]

---

> [!info] Dashboard Shape
> This note is best as the curated landing page.
> Use Dataview here for fixed overviews.
> Use Obsidian Bases separately when you want interactive filtering and sorting across all notes.

## All Notes Base

![[00 - All Notes.base#All notes]]

---

## Recently Updated

```dataview
TABLE WITHOUT ID
file.link AS "Note",
course AS "Course",
type AS "Type",
date AS "Date",
file.mtime AS "Updated"
FROM ""
WHERE type != null AND !contains(file.path, "templates")
SORT file.mtime DESC
LIMIT 12
```

---

## Course Overview

```dataviewjs
const pages = dv.pages()
  .where(p => p.course && !p.file.path.includes("templates"));

const grouped = [...pages.groupBy(p => p.course)]
  .sort((a, b) => a.key.localeCompare(b.key));

dv.table(
  ["Course", "Lectures", "Exercises", "Selbstkontrolle", "Lernziele", "Resources", "Last update"],
  grouped.map(group => [
    group.key,
    group.rows.where(p => p.type === "lecture").length,
    group.rows.where(p => p.type === "exercise").length,
    group.rows.where(p => p.type === "selbstkontrolle").length,
    group.rows.where(p => p.type === "lernziele").length,
    group.rows.where(p => p.type === "resource").length,
    group.rows
      .map(p => p.file.mtime)
      .sort((a, b) => b.ts - a.ts)[0]
  ])
);
```

---

## Recent Study Material

```dataview
TABLE WITHOUT ID
file.link AS "Deck",
course AS "Course",
type AS "Type",
date AS "Date",
file.mtime AS "Updated"
FROM ""
WHERE type = "selbstkontrolle" OR type = "lernziele"
SORT file.mtime DESC
LIMIT 10
```

---

## Recent Lectures

```dataview
TABLE WITHOUT ID
file.link AS "Lecture",
course AS "Course",
date AS "Date",
file.mtime AS "Updated"
FROM ""
WHERE type = "lecture"
SORT file.mtime DESC
LIMIT 10
```

---

## Metadata Gaps

```dataview
TABLE WITHOUT ID
file.link AS "Note",
course AS "Course",
type AS "Type"
FROM ""
WHERE type != null AND !date AND !contains(file.path, "templates")
SORT course ASC, file.name ASC
```

---

## Flashcard Files

```dataview
TABLE WITHOUT ID
file.link AS "Deck",
course AS "Course",
type AS "Type"
FROM #flashcards
SORT course ASC, file.name ASC
```
