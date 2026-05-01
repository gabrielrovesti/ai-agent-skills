## Environment assumptions

- Output target: `.md` files  
- Rendering target: GitHub / GitLab / VS Code / Confluence-compatible Markdown  
- Encoding: UTF-8 (no BOM)  

---

## Core Rules

### 1. Structure first, content after

- Define heading hierarchy before writing content  
- Use ordered levels only: `#` → `##` → `###`  
- Do not skip levels  

---

### 2. Minimal, consistent syntax

- Headings: `#`, `##`, `###`  
- Lists: `-` only  
- Code blocks: triple backticks with language  
- Inline code: single backticks  
- Emphasis: `**bold**`  

---

### 3. Whitespace discipline

- One blank line:
  - after headings  
  - before and after code blocks  
  - between sections  
- No trailing spaces  
- No multiple blank lines  

---

### 4. Code blocks (strict)

- Always specify language:

```

```java
code here
```

- No explanations inside code blocks  
- No manual indentation  

---

### 5. Lists formatting

- Prefer flat lists  
- Nested lists max depth: 2  
- Indentation: 2 spaces  

Example:

```
- Item
  - Sub-item
```

---

### 6. Tables (only if needed)

- Use pipe format  
- Keep simple  
- Align columns  

---

### 7. Links

- Format: `[text](url)`  
- Avoid raw URLs  

---

### 8. Line length

- Soft limit: ~100 characters  
- Break long lines manually  

---

### 9. No auto-formatting

- Do not reflow existing text  
- Do not change punctuation unless requested  

---

### 10. Atomic edits

- Modify only required sections  
- Preserve untouched formatting  

---

## Output Patterns

### Standard Document Template

```
# Title

## Section

Short paragraph.

## Section

- Item 1
- Item 2

## Code Example

```language
code here
```
```

---

### Technical Documentation Pattern

```
# Feature Name

## Overview

Concise description.

## Architecture

- Component A
- Component B

## Flow

1. Step one
2. Step two

## API

```http
GET /endpoint
```

## Notes

- Constraint
- Edge case
```

---

### Patch Mode (for Codex)

```
CHANGE:
- Replace section "X" with:

## X
New content
```

No full file rewrite.

---

## Anti-Patterns

- Mixed list markers (`-`, `*`, `+`)  
- Missing language in code blocks  
- Heading level jumps  
- Inline HTML (unless required)  
- Full file rewrites for small changes  
- Smart quotes or non-ASCII punctuation  

---

## Optional Extension (Confluence-like systems)

- Avoid tables if possible  
- Prefer simple lists  
- Avoid deep nesting  
- Avoid HTML  

---

## Result

- Deterministic output  
- Minimal diffs  
- Format stability  
- Broad Markdown compatibility  
```
