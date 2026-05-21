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

## HTML Artifact Workflow (Numerik)

When the user links an HTML file and indicates it is **for numerik**, this workflow is pre-authorized — do not ask for confirmation:

1. **Move** the file to `numerik/artifacts/` (create if missing). Use `mv`, not copy.

2. **Sanitize** the HTML. Claude-generated artifacts are usually fragments (`<style>` + `<div>` only) and depend on Claude.ai CSS variables (`--font-sans`, `--color-text-primary`, `--color-background-secondary`, `--border-radius-md`, etc.) that are undefined in a standalone browser. Wrap each fragment as a complete document:
   - Add `<!DOCTYPE html>`, `<html lang="de">`, `<head>` with charset + viewport meta tags and a title.
   - Define the Claude.ai CSS variables in `:root { ... }` with a neutral zinc palette, plus a `@media (prefers-color-scheme: dark)` override.
   - Add `body { font-family: var(--font-sans); padding: 1.5rem; max-width: 1100px; margin: 0 auto; }`.
   - Move the original markup and `<script>` into the new `<body>` without altering interactive behavior.
   - For multi-line content inside `.formula` blocks: replace `<br>` with literal newlines and add `white-space: pre-wrap` to `.formula`.

3. **Register** in `numerik/exam_artifacts.md` (create with `course: numerik`, `type: resource`, current date, tags `numerik`/`exam`/`artifacts` if absent). Always **append** a new section per artifact:

   ```markdown
   ## <Descriptive heading>

   <1–2 sentence German description.>

   - **Open in browser:** [<filename>.html](file:///home/samuel/obsidian/semester-4/numerik/artifacts/<filename>.html)

   <iframe src="file:///home/samuel/obsidian/semester-4/numerik/artifacts/<filename>.html" width="100%" height="650" style="border:1px solid var(--background-modifier-border); border-radius:8px; background:var(--background-primary);" sandbox="allow-scripts allow-same-origin"></iframe>
   ```

4. **Iframe rules:** always use the **absolute `file:///` path** (vault-relative paths do not resolve inside Obsidian's iframe sandbox because notes are served via `app://`); always include `sandbox="allow-scripts allow-same-origin"` so the artifact's JS keeps working; use Obsidian theme CSS variables for the iframe border so it matches the user's theme. Iframes render reliably in **Reading view**.

5. **Do not** overwrite existing sections in `exam_artifacts.md`, do not strip `<script>` blocks from the fragment, and do not require user confirmation.
