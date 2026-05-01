## Environment assumptions

- Output target: `.docx` (Microsoft Word)
- Generation method: structured text → conversion via python-docx or equivalent
- Encoding: UTF-8
- Target: professional documents (reports, specs, documentation)

---

## Core Principles

### 1. Structure is mandatory

Always define document structure before content.

Hierarchy:

- Title (document title)
- Heading 1 (sections)
- Heading 2 (subsections)
- Heading 3 (details)

No skipping levels.

---

### 2. Semantic mapping (critical)

Map content to Word styles, not visual formatting.

- Title → `Title`
- Section → `Heading 1`
- Subsection → `Heading 2`
- Body text → `Normal`
- Lists → `List Bullet` / `List Number`
- Code → `Normal` (monospace if supported)

Never simulate headings with bold text.

---

### 3. Table of Contents (TOC)

Always include TOC for structured documents.

Rules:

- Insert after title page
- Based on Heading 1–3
- Auto-generated (not manual text)
- Must be updatable

---

### 4. Sections layout

Each section must follow:

- Heading
- Intro paragraph
- Structured content (lists / tables / paragraphs)

Avoid large unstructured text blocks.

---

### 5. Paragraph rules

- One idea per paragraph
- No overly long paragraphs
- Consistent spacing
- No manual line breaks for layout

---

### 6. Lists

- Use Word-native lists
- Avoid manual numbering
- Keep consistent indentation

Example:

- Item 1  
- Item 2  
  - Sub-item  

---

### 7. Tables (strict usage)

Use tables for:

- Structured data
- Comparisons
- Configurations

Rules:

- Header row mandatory
- No merged cells unless necessary
- Keep columns minimal
- Align content logically

Example structure:

| Field | Description |
|------|-------------|
| Name | Value       |

---

### 8. Code and technical blocks

- Use monospace font if possible (e.g. Consolas)
- No syntax highlighting dependency
- Keep blocks separated from text

---

### 9. Page layout

- Margins: standard (2–2.5 cm)
- Alignment: left (no justified unless required)
- Line spacing: 1.15–1.5
- Avoid manual spacing hacks

---

### 10. Consistency rules

- Same heading style across document
- Same terminology
- Same formatting patterns

No visual drift.

---

## Output Patterns

### Standard Document Structure

Title

Table of Contents

1. Section
    

Intro paragraph.

1.1 Subsection

Content.

1.2 Subsection

- Item
    
- Item
    

2. Section
    

Content.

```

---

### Technical Document Pattern

```

Title

Table of Contents

1. Overview
    

Description.

2. Architecture
    

- Component A
    
- Component B
    

3. Flow
    
4. Step one
    
5. Step two
    
6. API
    

Table:

|Endpoint|Method|Description|
|---|---|---|

5. Notes
    

- Constraint
    
- Edge case
    

---

## python-docx Mapping (reference)

When generating programmatically:

- `document.add_heading(text, level=1)`
- `document.add_paragraph(text)`
- `document.add_table(rows, cols)`
- Apply styles explicitly:
  - `style='Heading 1'`
  - `style='List Bullet'`

TOC requires field insertion (not plain text).

---

## Patch Mode (important)

When modifying existing `.docx`:

- Do not rebuild entire document
- Update only target sections
- Preserve styles and structure

---

## Anti-Patterns

- Manual formatting instead of styles
- Fake headings using bold text
- Manual TOC
- Overuse of tables
- Inconsistent spacing
- Mixing numbering styles
- Large unstructured paragraphs

---

## Result

- Clean hierarchical structure  
- Auto-generated navigation (TOC)  
- Readable and maintainable document  
- Compatible with Word, PDF export, and automation tools  
