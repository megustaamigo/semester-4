# Obsidian Vault — Semester 4

This is an Obsidian vault for university coursework in semester 4.

## Courses

- `web-engineering` -> `web-engineering/`
- `kommunikationssysteme` -> `kommunikationssysteme/`
- `numerik` -> `numerik/`
- `csharp` -> `csharp/`
- `data-science` -> `data-science/`
- `large-scale-it` -> `large-scale-it/`

## Vault Structure

Each course may contain:

- `<prefix>-index.md` as the course index file
- `lectures/` for lecture notes
- `exercises/` for exercise sheets and solutions
- `resources/` for PDFs, slides, and source material
- `repos/` for coursework repositories
- `lernziele/` for web-engineering learning objectives
- `selbstkontrolle/` for kommunikationssysteme and data-science flashcard questions

Vault root:

- `00 - Dashboard.md` is the main entry point
- `templates/` contains Templater templates

## Plugins and Note Style

- Dataview is used for dashboard and course indexes
- Templater is used instead of core templates
- Spaced Repetition uses `#flashcards/<course>` tags

Use Obsidian callouts where appropriate:

- `> [!tip] Merke`
- `> [!warning] Achtung`
- `> [!info] Hinweis`
- `> [!example] Beispiel`
- `> [!quote] Definition`
- `> [!success] Best Practice`

Prefer inline Mermaid diagrams in lecture notes for flows, comparisons, architectures, and concepts tied to Lernziele or Selbstkontrolle.

## When Working With Provided Files

When the user provides a lecture PDF, slide deck, or other source file:

1. Identify the course and lecture or exercise number from context; ask only if it is not discoverable.
2. Move the file into the correct `<course>/resources/` directory.
3. Read the material and extract the relevant content.
4. Create the corresponding structured note in the appropriate course folder.
5. Update the course index file with wikilinks to the new note and resource.

When the user provides an exercise PDF:

1. Move it into `<course>/resources/`.
2. Create `<course>/exercises/XX/exercise-XX.md` with the extracted tasks.
3. Create `<course>/exercises/XX/solution-XX.md` with complete solutions.
4. Generate `solution-XX.pdf` with `pandoc` when requested or when the workflow depends on it.
5. Update the course index file with links to the exercise and solution.

For coding exercises, keep solutions compact but readable and pull code from the course repo when it exists. For theory exercises, include full intermediate steps.

## Lernziele and Selbstkontrolle

These exist in two forms:

- A compact flashcard file using single-line `Question::Answer`
- A lecture note section with full formatted explanations

Rules:

- `web-engineering/lernziele/` stores Lernziele files
- `kommunikationssysteme/selbstkontrolle/` stores Selbstkontrolle files
- `data-science/selbstkontrolle/` stores Rueckblick/Selbstkontrolle files
- Flashcard files include `flashcards/<course>` in frontmatter tags
- Lecture notes should link back to the flashcard file and expand each item in prose

Do not use the multi-line `?` flashcard format.

## Naming and Frontmatter

Use kebab-case filenames.

Every note, including course index files, should start with a course prefix. The only root-level exception is `00 - Dashboard.md`:

- `webeng-` for Web Engineering
- `komsys-` for Kommunikationssysteme
- `num-` for Numerik
- `cs-` for C#
- `ds-` for Data Science
- `lsit-` for Large Scale IT

Patterns:

- Course indexes: `<prefix>-index.md`
- Lectures: `<prefix>-<NN>-<topic>.md`
- Exercises: `<prefix>-exercise-<NN>.md`
- Solutions: `<prefix>-solution-<NN>.md`
- Selbstkontrolle: `<prefix>-selbstkontrolle-<NN>.md`
- Lernziele: `<prefix>-lernziele-<NN>.md`

Every note should include frontmatter in this shape:

```yaml
---
course: <course-tag>
type: lecture | exercise | solution | resource | selbstkontrolle | lernziele
lecture: <NN>  # required for lecture-tied notes such as lectures, exercises, solutions, lernziele, selbstkontrolle
date: YYYY-MM-DD
tags:
  - <course-tag>
  - <type>
  - <topic-specific-tags>
  - flashcards/<course-tag>  # only for flashcard notes
source: <original-filename-if-applicable>
---
```

Rules:

- `date` is required for every typed note.
- `lecture` is required for notes that refer to a specific lecture or sheet: `lecture`, `exercise`, `solution`, `lernziele`, and `selbstkontrolle`.
- `lecture` is not required for course index files or general resource notes that do not map to a single lecture.

Valid course tags:

- `web-engineering`
- `kommunikationssysteme`
- `numerik`
- `csharp`
- `data-science`
- `large-scale-it`

## Linking and TOC

- Use `[[wikilinks]]` between related notes.
- Always link source files from the note body.
- Always add new notes to the relevant course index file.
- If a markdown file has more than two headings, add an `## Inhaltsverzeichnis` section after the title and metadata links with `[[#Heading|Heading]]` entries for all `##` headings.

## Course-Specific Repo Note

`numerik/repos/numerik/` is a local-only repo. Work is organized by sheet in `blattXX/` directories with `aufgabeN.py` files and a `main.py` runner. A `.venv/` with `numpy` is used there.

## Numerik HTML Artifacts

Interactive HTML exam-prep artifacts (step-tabbed visualizations of LU, Jacobi, Gauß-Seidel, etc.) live in `numerik/artifacts/` and are registered in `numerik/exam_artifacts.md`.

**Create new artifacts via the `/numerik-artifact` skill** (`~/.claude/skills/numerik-artifact/SKILL.md`). The skill is the single source of truth. It asks the caller (1) which Numerik method and (2) for example numbers, then writes a complete standalone HTML file directly into the vault — KaTeX-rendered LaTeX formulas, color-traced calculations via `\cblue{...}` / `\cteal{...}` macros, step-tabbed matrix/vector visualizations, per-row symbol legends (one row per symbol, never inline) — and appends the iframe entry to `exam_artifacts.md`.

Do **not** accept pre-generated HTML artifacts to "move and sanitize" — that workflow has been retired. If the user pastes an HTML file path and mentions Numerik, ask whether they want to rebuild it cleanly via `/numerik-artifact` from a method + example description, or keep the file outside the artifacts folder.

Canonical reference artifacts (read before producing new ones):

- `numerik/artifacts/lu_zerlegung_3x3.html`
- `numerik/artifacts/gauss_seidel_komplett.html`
- `numerik/artifacts/jacobi_2x2_gleich_gauss_seidel.html`

Defer to the skill file for all structural / styling / color / KaTeX / legend rules — do not duplicate them here.

