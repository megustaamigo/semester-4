---
course: <% tp.system.prompt("Course tag?") %>
type: selbstkontrolle
date: <% tp.date.now("YYYY-MM-DD") %>
tags:
  - <% tp.system.prompt("Course tag?") %>
  - selbstkontrolle
  - flashcards/<% tp.system.prompt("Course tag?") %>
---

# <% tp.file.title %>

