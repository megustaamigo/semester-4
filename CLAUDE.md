# Obsidian Vault — Semester 4

This is an Obsidian vault for university coursework in semester 4.

## Courses

- **Web Engineering** → `web-engineering/` — Active
- **Kommunikationssysteme** → `kommunikationssysteme/` — Active
- **Numerik** → `numerik/` — Active
- **C#** → `csharp/` — Active
- **Introduction to Data Science** → `data-science/` — Active
- **Large Scale IT and Cloud Computing** → `large-scale-it/` — Block course, starts in ~8 weeks

## Lecture Numbering vs. Weeks (IMPORTANT — read before creating lecture notes)

Especially relevant for **Web Engineering** and **Kommunikationssysteme**, where one week often spans several slide decks:

- **Lecture-note numbers track the slide deck / content unit, NOT the calendar week.** Example: `webeng-12-javascript-fetch.md` and `webeng-13-restful-services.md` are slide decks 12 and 13, but both are taught in **week 8** → they live in `lectures/08/` and both carry `lecture: 8`.
- **A single week can hold multiple decks** — multiple `webeng-NN-*` / `komsys-NN-*` notes can share the same `lectures/<week>/` folder.
- **`lernziele/` and `selbstkontrolle/` files are numbered by WEEK, not by deck.** So `webeng-lernziele-08.md` (week 8) covers *all* decks taught that week. Link every lecture note of a week to the same week-numbered lernziele/selbstkontrolle file, and split that file's cards across the notes when the week has several decks (e.g. cards 1–5 → Fetch note, cards 6–15 → REST note).
- **The `lecture:` frontmatter value = the week number** (it matches the `lectures/<NN>/` folder), never the deck number.
- **The user always tells you which week** a provided deck belongs to. Use that week for (1) the `lectures/<NN>/` folder, (2) the `lecture:` frontmatter, and (3) the lernziele/selbstkontrolle link. **Do not infer the week from the deck number.**

## Folder Structure

Each course has:
- A **course index file** using the course prefix (e.g., `kommunikationssysteme/komsys-index.md`) — this is the hub that links to all notes in that course
- `lernziele/` — Learning objectives per lecture (web-engineering)
- `selbstkontrolle/` — Self-check questions per lecture (kommunikationssysteme, data-science)
- `lectures/` — Lecture notes and summaries, organized in subfolders **per week** (e.g., `lectures/01/`). A week folder may contain several deck-numbered notes — see **Lecture Numbering vs. Weeks** above.
- `exercises/` — Exercises organized in subfolders per sheet (e.g., `exercises/01/`). Each subfolder contains an `exercise-XX.md` (task description), a `solution-XX.md` (compact code solutions), and a `solution-XX.pdf` (generated via pandoc). When creating solution files, extract code from the repo and compress it to be as compact as possible while remaining readable.
- `resources/` — PDFs, slides, images, and other provided materials
- `repos/` — Git repositories for practical coursework (web-engineering, kommunikationssysteme, csharp, numerik). Repos are cloned/created directly under this folder (e.g., `web-engineering/repos/p01-NDSS/`). Do NOT move repo files into the vault — reference them in-place from exercise notes.
  - **Numerik** uses a local-only repo at `numerik/repos/numerik/` (no remote). Code is organized per sheet in `blattXX/` subfolders, each with `aufgabeN.py` files and a `main.py` runner. Uses a Python venv (`.venv/`) with numpy.
- `artifacts/` — Interactive HTML artifacts (only `numerik/` has one currently). Registered in `<course>/exam_artifacts.md`. See the **HTML Artifact Workflow** section below.

The vault root contains:
- `00 - Dashboard.md` — Main entry point with Dataview dynamic queries
- `templates/` — Templater templates for lecture notes, exercises, solutions, selbstkontrolle

## Installed Plugins

- **Dataview** — Dynamic queries for Dashboard and course index files
- **Templater** — Enhanced templates with dynamic variables (replaces core Templates)
- **Spaced Repetition** — Flashcard review system using `#flashcards/<course>` tags

## Workflow: When the User Provides a File

1. **Ask the user** which course it belongs to and which lecture/exercise/homework number it is (if not obvious from the filename or context)
2. **Move the file** (not copy) into the correct course's `resources/` folder
3. **Read the file** and extract all relevant information
4. **Create a structured note** in the appropriate lecture subfolder (e.g., `lectures/01/01-einfuehrung-web.md`)
   - Add proper YAML frontmatter (see conventions below)
   - Link to the original file in `resources/` using `**Folien:**` or `**Quelle:**` wikilink
   - Link to the corresponding lernziele file if one exists
   - Extract and organize all content from the provided material
   - **Add Mermaid diagrams** for key concepts, especially those tied to Lernziele/Selbstkontrolle questions (see Mermaid section below)
   - **Use Obsidian callouts** for Merke, Achtung, Best Practice, etc. (see Callouts section below)
5. **Update the course index file** — add a wikilink to the new note under the appropriate section (Lectures, Exercises, Homework, Resources)

## Workflow: When the User Provides an Exercise PDF

When an exercise sheet PDF is provided (e.g., `NumI-02.pdf`):
1. **Move the PDF** into the course's `resources/` folder
2. **Create `exercise-XX.md`** in `<course>/exercises/XX/` — extract all tasks from the PDF into a structured markdown note with frontmatter, linking back to the PDF in resources
3. **Create `solution-XX.md`** in the same folder — solve **all** tasks (both coding and hand/theory tasks). For coding tasks: extract compact code from the repo (or write it fresh), include test results and observations. For hand/theory tasks: provide full step-by-step mathematical solutions with intermediate results. Link to both the exercise PDF and the solution PDF
4. **Generate `solution-XX.pdf`** via `pandoc solution-XX.md -o solution-XX.pdf` — place alongside the markdown. Avoid Unicode symbols (use text like "(korrekt)" instead of checkmarks) for LaTeX compatibility
5. **Update the course index file** — add wikilinks to both the exercise and solution notes

## Workflow: Lernziele (Learning Objectives)

Lernziele content lives in **two places** serving different purposes:
- **Lernziele file** (`<prefix>-lernziele-XX.md`) → compact `::` flashcards for spaced repetition
- **Lecture note** → full, nicely formatted answers with diagrams for studying/reading

When the user provides Lernziele for a lecture:
1. **Create the lernziele file** in `<course>/lernziele/<prefix>-lernziele-<number>.md`
   - Include `#flashcards/<course>` in the frontmatter tags
   - Convert each lernziel into a **single-line flashcard**: `Question::Concise answer`
2. Link it from the course index file under a "Lernziele" section
3. When creating lecture notes, add a **"Bezug zu Lernzielen"** section at the end that:
   - Links to the lernziele file
   - For each relevant lernziel: state which lernziel is addressed, then provide a **detailed, nicely formatted answer** with bullet points, examples, and Mermaid diagrams where helpful

## Workflow: Fragen zur Selbstkontrolle (Kommunikationssysteme)

Same dual-location principle:
- **Selbstkontrolle file** → compact `::` flashcards
- **Lecture note** → full formatted answers for studying

1. **Create the selbstkontrolle file** in `kommunikationssysteme/selbstkontrolle/komsys-selbstkontrolle-<number>.md`
   - Include `#flashcards/kommunikationssysteme` in the frontmatter tags
   - Add each question as a **single-line flashcard**: `Question::Concise answer`
2. Link it from the course index file under a "Selbstkontrolle" section
3. When creating lecture notes, add a **"Fragen zur Selbstkontrolle"** section at the end that:
   - Links to the selbstkontrolle file
   - For each relevant question: state the question in bold, then provide a **detailed answer based on the lecture material**

## Workflow: Rueckblick / Selbstkontrolle (Data Science)

Same dual-location principle. Questions are found at the **end of the lecture PDF slides** under "Rueckblick":
1. **Create the selbstkontrolle file** in `data-science/selbstkontrolle/ds-selbstkontrolle-<number>.md`
   - Include `#flashcards/data-science` in the frontmatter tags
   - Add each question as a **single-line flashcard**: `Question::Concise answer`
2. Link it from the course index file under a "Selbstkontrolle" section
3. When creating lecture notes, add a **"Fragen zur Selbstkontrolle"** section at the end that:
   - Links to the selbstkontrolle file
   - For each question: state the question in bold, then provide a **detailed answer based on the lecture material**

## Numerik HTML Artifacts

Interactive HTML exam-prep artifacts (step-tabbed visualizations of LU, Jacobi, Gauß-Seidel, etc.) live in `numerik/artifacts/` and are registered in `numerik/exam_artifacts.md`.

**To create a new artifact, invoke the `/numerik-artifact` skill** (`~/.claude/skills/numerik-artifact/SKILL.md`). The skill is the single source of truth for the artifact format. It asks the caller (1) which Numerik method and (2) for example numbers, then writes a complete standalone HTML file directly into the vault — KaTeX-rendered LaTeX formulas, color-traced calculations via `\cblue{...}` / `\cteal{...}` macros, step-tabbed matrix/vector visualizations, and per-row symbol legends (one entry per row, never inline) — and appends the iframe entry to `exam_artifacts.md`.

**Do not** accept pre-generated HTML artifacts to "move and sanitize" — that workflow has been retired. If the user pastes an HTML path and mentions Numerik, ask whether they want to rebuild it cleanly via `/numerik-artifact` based on a method + example description, or keep the file outside the artifacts folder.

Canonical reference artifacts (read before producing new ones — they encode the conventions):
- `numerik/artifacts/lu_zerlegung_3x3.html`
- `numerik/artifacts/gauss_seidel_komplett.html`
- `numerik/artifacts/jacobi_2x2_gleich_gauss_seidel.html`

All structural / styling / color / KaTeX / legend rules live in the skill file. When the skill is updated, defer to it instead of duplicating its rules here.

## Callouts in Lecture Notes

Use Obsidian callout syntax instead of plain blockquotes. These are the six callout types used in this vault:

| Callout | Syntax | Use Case |
|---|---|---|
| Merke | `> [!tip] Merke` | Key facts to remember for exams |
| Achtung | `> [!warning] Achtung` | Common pitfalls, gotchas |
| Hinweis | `> [!info] Hinweis` | Supplementary context, background info |
| Beispiel | `> [!example] Beispiel` | Worked examples |
| Definition | `> [!quote] Definition` | Formal definitions, protocol specs |
| Best Practice | `> [!success] Best Practice` | Recommended patterns |

Guidelines:
- Use callouts for all "Merke", "Achtung", "Wichtig", "Best Practice", "Hinweis", and definition blockquotes
- Content goes on lines after the header, each prefixed with `> `
- Use foldable callouts (`> [!tip]- Title`) for answers that should be hidden by default
- Do NOT use callouts inside code blocks or for regular paragraph text
- Additional types (`> [!warning] Wichtig`, `> [!tip] Regel`, `> [!danger]`) may be used when semantically appropriate

## Spaced Repetition

Lecture notes and selbstkontrolle files use the Spaced Repetition plugin for active recall.

### Flashcard Tags (in frontmatter)
- `#flashcards/web-engineering`
- `#flashcards/kommunikationssysteme`
- `#flashcards/data-science`
- `#flashcards/csharp`
- `#flashcards/numerik`

### Card Formats

**Single-line** (primary format for all flashcards):
```
Question::Concise answer
```

**Cloze deletion** (for formulas, key terms):
```
The default access modifier for classes is =={internal}==.
```

Do NOT use the multi-line `?` separator format — it renders as an ugly standalone question mark in reading mode.

### Where to Place Cards
- **Selbstkontrolle/Lernziele files:** Primary flashcard source — use single-line `Question::Answer` format, tag with `#flashcards/<course>`
- **Lecture notes:** Contain full, formatted answers for studying (NOT flashcard format). Do NOT put `#flashcards` tags on lecture notes

## Mermaid Diagrams in Lecture Notes

When creating lecture notes, **add inline Mermaid diagrams** to visualize key concepts. Prioritize concepts that:
- Are directly relevant to **Lernziele** or **Selbstkontrolle/Rueckblick** questions
- Involve flows, processes, or sequences (e.g., pipelines, request/response, protocol handshakes)
- Compare or contrast models (e.g., OSI vs TCP/IP, classical vs ML)
- Show hierarchies or layered architectures (e.g., network layers, encapsulation)
- Illustrate structural relationships (e.g., box model, URI/URL/URN)

Guidelines:
- Place diagrams **inline** right after the relevant text section — not in separate files
- Use `flowchart`, `sequenceDiagram`, `graph` types as appropriate
- Keep diagrams focused and readable — one concept per diagram
- Use German labels when the lecture content is in German
- Use colors/styles sparingly for emphasis (e.g., highlight a trailer in red)

## Conventions

### File Naming
- Use kebab-case for all file names
- **Every note file**, including course index files, must start with a **course prefix**. The only root-level exception is `00 - Dashboard.md`:

| Course | Prefix | Example |
|---|---|---|
| Web Engineering | `webeng-` | `webeng-01-einfuehrung-web.md` |
| Kommunikationssysteme | `komsys-` | `komsys-01-einfuehrung.md` |
| Numerik | `num-` | `num-exercise-01.md` |
| C# | `cs-` | `cs-01-strukturierte-programmierung.md` |
| Data Science | `ds-` | `ds-01-ueberblick-grundbegriffe.md` |
| Large Scale IT | `lsit-` | `lsit-01-intro.md` |

- **Naming pattern:** `<prefix>-<number-or-type>-<short-topic>.md`
  - Lectures: `<prefix>-<NN>-<topic>.md` (e.g., `komsys-03-sockets.md`)
  - Exercises: `<prefix>-exercise-<NN>.md` (e.g., `num-exercise-01.md`)
  - Solutions: `<prefix>-solution-<NN>.md` (e.g., `num-solution-01.md`)
  - Selbstkontrolle: `<prefix>-selbstkontrolle-<NN>.md` (e.g., `ds-selbstkontrolle-01.md`)
  - Lernziele: `<prefix>-lernziele-<NN>.md` (e.g., `webeng-lernziele-01.md`)
- Course index files use the course prefix and the `-index.md` suffix (e.g., `komsys-index.md`)

### YAML Frontmatter
Every note must include frontmatter in this format:
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
  - flashcards/<course-tag>  # only for notes with flashcard content
source: <original-filename-if-applicable>
---
```

Valid course tags: `web-engineering`, `kommunikationssysteme`, `numerik`, `csharp`, `data-science`, `large-scale-it`

Rules:
- `date` is required for every typed note.
- `lecture` is required for notes that map to a specific lecture or sheet: `lecture`, `exercise`, `solution`, `lernziele`, and `selbstkontrolle`.
- `lecture` is not required for course index files or general resource notes that do not belong to one lecture.

### Linking
- Each course has an index file (e.g., `kommunikationssysteme/komsys-index.md`) that links to all notes in that course
- The `00 - Dashboard.md` in the vault root uses Dataview queries for dynamic content
- Use `[[wikilinks]]` to connect related concepts across courses
- Always link provided resources (PDFs, slides) from within the lecture note using `[[course/resources/filename.pdf|display name]]`
- When adding a new note, always add a link in the course index file under the correct section

### Table of Contents
Every markdown file with more than 2 headings must include a **Table of Contents** with Obsidian `[[#heading]]` jump links, placed directly after the title (and any metadata links like Folien/Quelle). Use the format:
```markdown
## Inhaltsverzeichnis

- [[#Heading 1|Heading 1]]
- [[#Heading 2|Heading 2]]
```
- List all `##` level headings (not `###` sub-headings — keep it concise)
- Use the exact heading text for both the link target and display text
- Separate the TOC from the rest with a `---` below it

### Content Extraction
When creating lecture notes from slides/PDFs:
- Use the lecture's original language (German terms are fine)
- Replace umlauts in filenames (ae, oe, ue) but keep them readable in content
- Structure content with clear headings matching the slide topics
- Use **callouts** for key definitions, formulas, and "Merke" sections (see Callouts section)
- Use tables for comparisons and classifications
- Keep the note self-contained — someone reading only the note should understand the material
- Add spaced repetition cards for exam-relevant facts (see Spaced Repetition section)

### Templates (Templater)
Available templates in `templates/`:
- `lecture.md` — Lecture note with dynamic course tag prompt and date
- `exercise.md` — Exercise task description
- `solution.md` — Exercise solution
- `selbstkontrolle.md` — Self-check questions with flashcard tag

Templates use Templater syntax (`<% tp.* %>`) for dynamic values. The templates folder is configured in Templater settings.

### PDF Generation
- **pandoc** is installed and can be used to generate PDFs from markdown files
- Command: `pandoc input.md -o output.pdf`
- Use this to generate PDF versions of solution files or any other notes when requested
- Place generated PDFs alongside their source markdown file
