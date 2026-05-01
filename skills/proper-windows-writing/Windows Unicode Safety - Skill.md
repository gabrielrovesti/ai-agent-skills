Windows Unicode Safety Skill

Purpose:
Use this skill only when working on Windows and there is any risk of breaking accents, apostrophes, quotes, dashes, or other Unicode characters during file reads, edits, or writes.

Core rules:
1. Default to minimal-diff mode.
2. Preserve existing text exactly unless I explicitly ask for rewriting.
3. Change only the minimum required lines. Do not rewrite whole files if a targeted patch is enough.
4. Do not normalize or “beautify” quotes, apostrophes, dashes, accented letters, or human-language strings.
5. Do not silently convert Unicode text into degraded ASCII forms.
6. When creating new files, write them as UTF-8.
7. When editing existing files, preserve the current visible text exactly, especially Italian words with accents.
8. If a file already contains non-ASCII text, do not touch unrelated lines.
9. If a rewrite may risk corrupting Unicode, stop and produce a minimal diff proposal instead.
10. If many non-ASCII literals are involved, prefer one of these safer strategies:
   - keep existing literals unchanged and edit only surrounding code
   - use ASCII-only placeholders and tell me exactly what must be replaced manually
   - use explicit Unicode escapes only when appropriate for the language and only if safer than raw literals

Operational rules:
- Prefer targeted patches over full rewrites.
- Prefer preserving existing literals over regenerating them.
- For code comments, test strings, sample text, and UI copy, use plain ASCII only if I am generating new text and real accented prose is not required.
- If existing text already contains accents or Unicode punctuation, preserve it exactly.
- If uncertain whether a method will preserve Unicode correctly, reject that method and switch to a safer one.

Mandatory behavior before risky edits:
- If editing an existing file containing non-ASCII characters, first show the exact lines to be changed, then apply a minimal patch.
- Never silently “fix” accented prose.
- Never replace words like:
  - perché -> perche'
  - città -> citta'
  - più -> piu'
  - l’unità -> l'unita'

Failure mode:
If you are not confident the edit will preserve Unicode correctly, do not rewrite the file. Output only a minimal diff or a safe replacement plan.

How to invoke this skill:
“Read file WINDOWS_UNICODE_SAFETY_SKILL.md and follow it strictly for this task.”
