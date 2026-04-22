# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Russian-language Q&A knowledge base for Java Developer interview preparation ("Вопросы для собеседования на Java Developer"). It is a content-only project — no application to build or deploy.

## Repository Structure

- **Root `.md` files** — Each file covers a topic area (oop.md, core.md, jcf.md, java8.md, concurrency.md, jvm.md, io.md, serialization.md, servlets.md, db.md, sql.md, jdbc.md, kafka.md, reactive.md, patterns.md, test.md, log.md, uml.md, xml.md, html.md, css.md, web.md). Content is in Q&A format with Markdown headers (`##`) as question anchors.
- **README.md** — Table of contents linking to questions in topic files. Each topic section lists questions as links to anchors in the corresponding `.md` file.
- **`examples/`** — Maven project (Java 8) with code examples referenced by the Q&A content. Uses Google Checkstyle, JUnit 4, Mockito.
- **`book/`** — PDF compilation of the Q&A content.
- **`con4md.jar`** + `mcon.sh`/`mcon.bat` — Table-of-contents generator. Run `./mcon.sh <file>` (or `mcon.bat` on Windows) to regenerate TOC using `##` markers.

## Commands

### Building examples
```bash
cd examples && mvn compile
```

### Running checkstyle on examples
```bash
cd examples && mvn checkstyle:check
```

### Generating table of contents for a markdown file
```bash
./mcon.sh <filename>.md          # Linux/Mac
mcon.bat <filename>.md           # Windows
```

## Content Conventions

- All Q&A content is written in Russian.
- Questions use `##` headers as anchors. Answers follow directly under each header.
- README.md mirrors the full question list with deep links (`file.md#anchor`).
- When adding a new question: add the Q&A to the topic file, then add a corresponding link in README.md under the appropriate section.
- The `[к оглавлению]` ("back to TOC") links at the end of each README section point back to the top-level heading.

## Last session
```
claude --resume f32809e5-4748-4f12-819c-b6829df7dfab
```