# Semester 4 Vault

Private Obsidian vault for semester 4 coursework.

This repository is primarily a personal knowledge base, but collaborators may contribute notes, fixes, and improvements through branches and pull requests.

## Purpose

- Central place for lecture notes, exercises, solutions, and learning materials
- Structured per course for Obsidian use
- Optimized for markdown-based collaboration with Git

## Courses

- `web-engineering/`
- `kommunikationssysteme/`
- `numerik/`
- `csharp/`
- `data-science/`
- `large-scale-it/`

## Vault Structure

- `00 - Dashboard.md`: main entry point
- `<course>/<course>.md`: course index
- `<course>/lectures/`: lecture notes
- `<course>/exercises/`: exercise sheets and solutions
- `<course>/resources/`: PDFs, slides, and source material
- `<course>/repos/`: local coursework repos
- `templates/`: Templater templates

## Obsidian Setup

Open this repository as an Obsidian vault.

### Required Community Plugins

These are actively used in the vault:

- `Dataview`
- `Templater`
- `Spaced Repetition`

### Important Note About `.obsidian/`

The `.obsidian/` folder is gitignored in this repository. That means collaborators will usually need to enable plugins manually in their own local vault setup.

At minimum:

1. Install the three community plugins listed above.
2. Enable community plugins in Obsidian.
3. Open the vault root as the Obsidian vault.

## Writing Conventions

- Use kebab-case filenames.
- Use Obsidian wikilinks like `[[note-name]]`.
- Update the relevant course index whenever you add a note.
- Link source PDFs from the note body.
- Use frontmatter on notes.
- Add an `## Inhaltsverzeichnis` section when a note has more than two headings.

### Naming Patterns

- Lectures: `<prefix>-<NN>-<topic>.md`
- Exercises: `<prefix>-exercise-<NN>.md`
- Solutions: `<prefix>-solution-<NN>.md`
- Selbstkontrolle: `<prefix>-selbstkontrolle-<NN>.md`
- Lernziele: `<prefix>-lernziele-<NN>.md`

### Course Prefixes

- `webeng-` for Web Engineering
- `komsys-` for Kommunikationssysteme
- `num-` for Numerik
- `cs-` for C#
- `ds-` for Data Science
- `lsit-` for Large Scale IT

### Frontmatter Shape

```yaml
---
course: <course-tag>
type: lecture | exercise | solution | resource | selbstkontrolle | lernziele
date: YYYY-MM-DD
tags:
  - <course-tag>
  - <type>
  - <topic-specific-tags>
  - flashcards/<course-tag>  # only for flashcard notes
source: <original-filename-if-applicable>
---
```

## Collaboration Workflow

Desired workflow:

1. Create a branch from `main`
2. Make focused changes
3. Open a pull request
4. Wait for review before merge

Branch naming suggestions:

- `feat/webeng-lecture-08`
- `fix/komsys-index-links`
- `docs/readme-vault-setup`

### Important

This repo is intended to keep `main` curated and stable. Do not push directly to `main`.

At the moment, this is a convention unless GitHub branch protection is enabled for the repository.

## GitHub Permissions and Protection

If this repository remains private on a personal GitHub account, GitHub branch protection for `main` requires GitHub Pro or another paid plan that supports private-repo protection.

Once that is available, the target setup should be:

- collaborators can push to their own branches
- direct pushes to `main` are blocked
- pull requests are required for `main`
- at least one approval is required before merge

## AI Assistant Usage

AI tools such as Codex or Claude can be useful for:

- converting slide decks or PDFs into structured notes
- generating lecture summaries
- drafting Lernziele or Selbstkontrolle notes
- updating course indexes consistently
- cleaning up formatting, frontmatter, and links

## Repo-Local AI Skills

This repository includes repo-local assistant guidance so collaborators can reuse the same vault-specific workflows.

Current repo-local assistant files:

- [AGENTS.md](/home/samuel/obsidian/semester-4/AGENTS.md): shared operating rules for agents working in this vault
- [.codex/skills/progress/SKILL.md](/home/samuel/obsidian/semester-4/.codex/skills/progress/SKILL.md): Codex skill for course progress and coverage checks
- [.claude/skills/progress/SKILL.md](/home/samuel/obsidian/semester-4/.claude/skills/progress/SKILL.md): Claude skill for the same course progress workflow

These files should stay in the repository so contributors get the same assistant behavior after cloning.

### Current Skill Capabilities

The current `progress` skill can inspect a course and report:

- resource coverage and which PDFs or slide decks are still missing notes
- lecture coverage and existing lecture-note dates
- Lernziele or Selbstkontrolle coverage where applicable
- exercise coverage and missing solutions
- course-index completeness and unlinked notes
- existing repos inside a course

Supported course names:

- `web-engineering`
- `kommunikationssysteme`
- `numerik`
- `csharp`
- `data-science`
- `large-scale-it`

### Codex and Claude Notes

- Codex can use repo-local skills from `.codex/skills/`
- Claude can use repo-local skills from `.claude/skills/`
- Keep the skill logic aligned across both directories when the same capability exists
- Avoid duplicate command wrappers when a proper skill already exists

### Rules for AI-Assisted Changes

- Follow the repository conventions in `AGENTS.md`
- Use repo-local skills from `.codex/` or `.claude/` when they match the task
- Preserve existing note structure and naming
- Do not invent sources that are not present in the vault
- Always update the corresponding course index
- Keep flashcards concise
- Keep lecture notes more detailed than flashcard files
- Prefer wiki links and consistent tags

### Good Prompting Guidance

When using Codex, Claude, or similar tools, specify:

- the course
- the lecture or exercise number
- the source file to use
- whether a Lernziele or Selbstkontrolle file must also be updated
- whether the course index should be updated

Example:

```txt
Use web-engineering/resources/08-JS-Basic.pdf and create lecture 04 notes, update Lernziele 04, and add links to the web-engineering index.
```

## Repository Hygiene

- Do not commit `.obsidian/` workspace-specific state unless the repo policy changes.
- Treat `*/repos/` as separately tracked nested repositories.
- Keep pull requests focused on one course or one logical change.

## Questions

If you are unsure about naming, placement, or formatting, check `AGENTS.md` first. It contains the working rules for this vault.
