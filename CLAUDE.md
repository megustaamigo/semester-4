# Obsidian Vault — Semester 4

This is an Obsidian vault for university coursework in semester 4.

## Courses

- **Web Engineering** → `web-engineering/` — Active
- **Kommunikationssysteme** → `kommunikationssysteme/` — Active
- **Numerik** → `numerik/` — Active
- **C#** → `csharp/` — Active
- **Introduction to Data Science** → `data-science/` — Active
- **Large Scale IT and Cloud Computing** → `large-scale-it/` — Block course, starts in ~8 weeks

## Folder Structure

Each course has:
- A **course index file** named after the course (e.g., `kommunikationssysteme/kommunikationssysteme.md`) — this is the hub that links to all notes in that course
- `lernziele/` — Learning objectives per lecture (web-engineering)
- `selbstkontrolle/` — Self-check questions per lecture (kommunikationssysteme, data-science)
- `lectures/` — Lecture notes and summaries, organized in subfolders per lecture (e.g., `lectures/01/`)
- `exercises/` — Exercises organized in subfolders per sheet (e.g., `exercises/01/`). Each subfolder contains an `exercise-XX.md` (task description) and optionally a `solution-XX.md` (compact code solutions). When creating solution files, extract code from the repo and compress it to be as compact as possible while remaining readable.
- `resources/` — PDFs, slides, images, and other provided materials
- `repos/` — Git repositories for practical coursework (web-engineering, kommunikationssysteme, csharp). Repos are cloned/created directly under this folder (e.g., `web-engineering/repos/p01-NDSS/`). Do NOT move repo files into the vault — reference them in-place from exercise notes.

The vault root contains:
- `00 - Dashboard.md` — Main entry point linking all courses
- `templates/lecture.md` — Reusable template for lecture notes

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
5. **Update the course index file** — add a wikilink to the new note under the appropriate section (Lectures, Exercises, Homework, Resources)

## Workflow: Lernziele (Learning Objectives)

When the user provides Lernziele for a lecture:
1. **Create the lernziele file first** in `<course>/lernziele/lernziele-<number>.md`
2. Link it from the course index file under a "Lernziele" section
3. When subsequently creating lecture notes for that lecture, **check if a lernziele file exists**
4. If lernziele exist, add a **"Bezug zu Lernzielen"** section at the end of each lecture note that:
   - Links to the lernziele file
   - For each relevant lernziel: state which lernziel is addressed, then provide a **concise summary of what the student should know/remember** based on the lecture material — not just explaining the connection but actually answering/summarizing the learning objective

## Workflow: Fragen zur Selbstkontrolle (Kommunikationssysteme)

Same principle as Lernziele but for Kommunikationssysteme:
1. **Create the selbstkontrolle file first** in `kommunikationssysteme/selbstkontrolle/selbstkontrolle-<number>.md`
2. Link it from the course index file under a "Selbstkontrolle" section
3. When creating lecture notes for that lecture, **check if a selbstkontrolle file exists**
4. If selbstkontrolle exist, add a **"Fragen zur Selbstkontrolle"** section at the end of each lecture note that:
   - Links to the selbstkontrolle file
   - For each relevant question: state the question, then provide a **concise answer based on the lecture material**

## Workflow: Rückblick / Selbstkontrolle (Data Science)

Same principle as Kommunikationssysteme Selbstkontrolle, but questions are found at the **end of the lecture PDF slides** under "Rückblick":
1. **Extract the questions** into `data-science/selbstkontrolle/selbstkontrolle-<number>.md`
2. Link it from the course index file under a "Selbstkontrolle" section
3. When creating lecture notes for that lecture, add a **"Fragen zur Selbstkontrolle"** section at the end that:
   - Links to the selbstkontrolle file
   - For each question: state the question, then provide a **concise answer based on the lecture material**

## Mermaid Diagrams in Lecture Notes

When creating lecture notes, **add inline Mermaid diagrams** to visualize key concepts. Prioritize concepts that:
- Are directly relevant to **Lernziele** or **Selbstkontrolle/Rückblick** questions
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
- Use kebab-case for file names (e.g., `lecture-01-einfuehrung.md`)
- Prefix lectures and exercises with their number for ordering (e.g., `lecture-02-schichtenmodell.md`)
- Course index files are named after the course (e.g., `kommunikationssysteme.md`, not `_index.md`)

### YAML Frontmatter
Every note must include frontmatter in this format:
```yaml
---
course: <course-tag>
type: lecture | exercise | resource
date: YYYY-MM-DD
tags:
  - <course-tag>
  - <type>
  - <topic-specific-tags>
source: <original-filename-if-applicable>
---
```

Valid course tags: `web-engineering`, `kommunikationssysteme`, `numerik`, `csharp`, `data-science`, `large-scale-it`

### Linking
- Each course has an index file (e.g., `kommunikationssysteme/kommunikationssysteme.md`) that links to all notes in that course
- The `00 - Dashboard.md` in the vault root links to all course index files
- Use `[[wikilinks]]` to connect related concepts across courses
- Always link provided resources (PDFs, slides) from within the lecture note using `[[course/resources/filename.pdf|display name]]`
- When adding a new note, always add a link in the course index file under the correct section

### Content Extraction
When creating lecture notes from slides/PDFs:
- Use the lecture's original language (German terms are fine)
- Replace umlauts in filenames (ae, oe, ue) but keep them readable in content
- Structure content with clear headings matching the slide topics
- Highlight key definitions, formulas, and "Merke" (remember) sections
- Use tables for comparisons and classifications
- Keep the note self-contained — someone reading only the note should understand the material

### PDF Generation
- **pandoc** is installed and can be used to generate PDFs from markdown files
- Command: `pandoc input.md -o output.pdf`
- Use this to generate PDF versions of solution files or any other notes when requested
- Place generated PDFs alongside their source markdown file
