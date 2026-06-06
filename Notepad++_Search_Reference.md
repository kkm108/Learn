# Notepad++ — Search, Replace & Mark Reference for Large-Scale Projects

> **Source:** [Official Notepad++ User Manual — Searching](https://npp-user-manual.org/docs/searching/)  
> **Applies to:** Notepad++ v8.x (all features noted by version where relevant)

---

## Table of Contents

1. [Overview of Search Mechanisms](#1-overview-of-search-mechanisms)
2. [Find Tab (`Ctrl+F`)](#2-find-tab-ctrlf)
3. [Replace Tab (`Ctrl+H`)](#3-replace-tab-ctrlh)
4. [Find in Files Tab (`Ctrl+Shift+F`)](#4-find-in-files-tab-ctrlshiftf)
5. [Find in Projects Tab](#5-find-in-projects-tab)
6. [Mark Tab (`Ctrl+M`)](#6-mark-tab-ctrlm)
7. [Search Modes Explained](#7-search-modes-explained)
8. [Practical Usage Examples](#8-practical-usage-examples)
9. [Caveats for Very Large Files](#9-caveats-for-very-large-files)
10. [Better Alternatives for Extreme Scale](#10-better-alternatives-for-extreme-scale)

---

## 1. Overview of Search Mechanisms

Notepad++ provides **three built-in search mechanisms**:

| Mechanism | Description |
|---|---|
| **Dialog-based** | Find / Replace / Find in Files / Find in Projects / Mark tabs in one unified dialog |
| **Dialog-free** | Next/Previous navigation shortcuts without opening a dialog |
| **Incremental Search** | Live search-as-you-type (via `Ctrl+Alt+I`) |

All keyboard shortcuts are reconfigurable via **Settings > Shortcut Mapper**.

---

## 2. Find Tab (`Ctrl+F`)

**Purpose:** Search a single open document for matches; count them; list them.

### Key Options

| Option | Behaviour |
|---|---|
| **Find what** | Text or pattern to search (up to 16,383 bytes as of v8.8.4) |
| **Match whole word only** | Skips partial-word hits (e.g. won't find "it" inside "hitch") |
| **Match case** | Case-sensitive search |
| **Wrap around** | Continues from top when search reaches end of file |
| **Backward direction** | Searches upward from current cursor position |
| **In selection** | Restricts Count/Mark to selected text only |
| **Search Mode** | Normal / Extended / Regular Expression (see §7) |

### Key Buttons

- **Find Next** — jumps to the next hit
- **Count** — shows total matches in the status bar
- **Find All in Current Document** — opens a Search Results window listing every hit with line numbers
- **Find All in All Opened Documents** — same, across every open tab

### Tips for Large Projects

- Use **Find All in Current Document** to audit a huge file before replacing anything.
- The **Find what** history dropdown (`Alt+↓`) recalls your last 10 (configurable) search terms across sessions.
- Selecting `> 1024 characters` of text before opening Find automatically ticks **In selection** — useful for scoped bulk searches inside a long function block.

---

## 3. Replace Tab (`Ctrl+H`)

**Purpose:** Find and optionally replace matches — one at a time or all at once.

### Key Buttons

| Button | Action |
|---|---|
| **Replace** | Replaces the currently highlighted match; behaves like Find Next if nothing is selected |
| **Replace All** | Replaces every occurrence (scope depends on Wrap Around / Backward Direction / In Selection) |
| **Replace All in All Opened Documents** | Replaces across every file currently open in Notepad++ |
| **⇅ Swap** (v8.2.1+) | Swaps Find and Replace values — handy for reversing a substitution |

### Replace All Scope Table

| Wrap Around | Backward | In Selection | Range Affected |
|---|---|---|---|
| ON | — | OFF | Entire file (top to bottom) |
| OFF | OFF | OFF | Caret position → end of file |
| OFF | ON | OFF | Start of file → caret position |
| — | — | ON | Selected text only |

### Clipboard-based Replacement (Practical Tip)

Notepad++ does not have a native "replace with clipboard contents" button, but you can:

1. Copy your replacement text to the clipboard.
2. Open `Ctrl+H`, click inside **Replace with**, and press `Ctrl+V` to paste it.
3. Click **Replace All**.

For repeated clipboard-driven replacements across many files, use a macro (`Ctrl+Shift+R` to record, `Ctrl+Shift+P` to play) or a scripting plugin (see §10).

---

## 4. Find in Files Tab (`Ctrl+Shift+F`)

**Purpose:** Search and/or replace across **all files in a directory** (or directory tree) — without opening them individually.

### Unique Fields

| Field | Description |
|---|---|
| **Directory** | Root folder to search in. Sub-folders are included unless you uncheck **In all sub-folders** |
| **Filters** | Comma-separated glob patterns to restrict file types, e.g. `*.js, *.ts` or `*.php` |
| **☐ In all sub-folders** | Recurse into child directories |
| **☐ In hidden folders** | Also search hidden directories |
| **☐ Follow current doc** | Auto-fills Directory with the folder of the currently active file |

### Key Buttons

- **Find All** — produces a Search Results window with file paths, line numbers, and match previews
- **Replace in Files** — performs the replacement directly on disk across all matching files

### Filter Examples

```
*.py                          # Python files only
*.js, *.ts, *.jsx             # All JavaScript / TypeScript variants
*.html, *.htm                 # HTML files
*config*                      # Any file with "config" in the name
```

> **Important:** Files do **not** need to be open in Notepad++. The operation reads files directly from disk and writes changes back — be sure to have backups or version control before using **Replace in Files** on a large directory.

---

## 5. Find in Projects Tab

**Purpose:** Same as Find in Files, but scoped to the files listed in an active **Project Panel** rather than a directory path.

### How to Access

Right-click the **top-level node** of a Project Panel → **Find in Projects**.

### Behaviour

- Only currently **open** Project Panels can be searched; closed panels are greyed out.
- The **Filters** field works identically to Find in Files.
- Results appear in the same **Search Results** window.
- Ideal for monorepo setups where you have a curated project list and don't want to scan an entire directory tree.

---

## 6. Mark Tab (`Ctrl+M`)

**Purpose:** Visually highlight all occurrences of a pattern in the current document, and/or place **bookmarks** on matching lines.

### How It Works

- **Mark All** — highlights every match using the style defined in `Settings > Style Configurator > Global Styles > Find Mark Style`.
- **☐ Bookmark line** — drops a bookmark (blue circle in the gutter) on every line that contains a match. If a match spans multiple lines, each of those lines gets a bookmark.
- Up to **5 simultaneous highlight colours** can be active at once (Style 1–5, each with its own colour).
- Marks persist in the document until you use **Search > Clear All Marks** or close the file.

### Bookmark Operations (Search > Bookmark menu)

| Action | Description |
|---|---|
| Toggle Bookmark | Add/remove bookmark on current line |
| Next / Previous Bookmark | Navigate between bookmarked lines |
| Clear All Bookmarks | Remove all bookmarks from the document |
| Cut Bookmarked Lines | Move all bookmarked lines to clipboard |
| Copy Bookmarked Lines | Copy all bookmarked lines to clipboard |
| Paste to (Replace) Bookmarked Lines | Replace each bookmarked line with clipboard content |
| Delete Bookmarked Lines | Delete all bookmarked lines |

### Large-Project Workflow with Mark

1. Open a large log or source file.
2. Use **Mark All** on a pattern (e.g. `ERROR`) to colour every error line.
3. Tick **Bookmark line** and click **Mark All** again.
4. Use **Search > Bookmark > Copy Bookmarked Lines** to extract only those lines.
5. Paste into a new document for further analysis.

---

## 7. Search Modes Explained

All search tabs share the same **Search Mode** selector:

### Normal
Literal text match. What you type is exactly what is searched. No special characters.

### Extended (`\n`, `\r`, `\t`, `\0`, `\x…`)
Interprets escape sequences for non-printable characters:

| Escape | Meaning |
|---|---|
| `\n` | Line Feed (Unix newline) |
| `\r` | Carriage Return |
| `\r\n` | Windows CRLF |
| `\t` | Tab |
| `\0` | Null byte |
| `\xHH` | Hex character code |

Use this mode when searching for or replacing newlines, tabs, or binary sequences.

### Regular Expression (Regex)
Uses the **Boost regex engine** (similar to PCRE). Full regex syntax is available.

Key metacharacters:

| Pattern | Meaning |
|---|---|
| `.` | Any character (except newline unless `. matches newline` is ticked) |
| `\d` | Any digit |
| `\w` | Word character (letter, digit, underscore) |
| `\s` | Whitespace |
| `^` / `$` | Start / end of line |
| `*`, `+`, `?` | Quantifiers (greedy) |
| `*?`, `+?` | Non-greedy quantifiers |
| `(…)` | Capture group |
| `$1`, `\1` | Backreference to group 1 in replacement |
| `(?i)` | Case-insensitive flag (inline) |
| `(?s)` | Dot-matches-newline flag (inline) |

---

## 8. Practical Usage Examples

### Example A — Replace 3 (or more) Consecutive Line Breaks with 2

**Use case:** Compressing excess blank lines in a log file or generated document.

| Setting | Value |
|---|---|
| **Tab** | Replace (`Ctrl+H`) |
| **Search Mode** | Regular Expression |
| **Find what** | `(\r?\n){3,}` |
| **Replace with** | `\n\n` |
| **Wrap around** | ✅ |

Click **Replace All**. This collapses any run of 3 or more blank lines down to exactly 2.

To reduce to a **single** line break instead:

| Find what | `(\r?\n){2,}` |
|---|---|
| **Replace with** | `\n` |

> **Note:** If your file uses Windows CRLF (`\r\n`), use `(\r\n){3,}` and replace with `\r\n\r\n` to preserve the line ending style.

---

### Example B — Replace a Search Term with the Current Clipboard Contents

Notepad++ has no dedicated "paste clipboard as replacement" button, but the workflow is straightforward:

1. Copy your desired replacement text (e.g. a new function body) to the clipboard.
2. Press `Ctrl+H` to open Replace.
3. Type your search term in **Find what**.
4. Click inside the **Replace with** box and press `Ctrl+V`.
5. Choose **Replace All** (or **Replace** to step through one at a time).

**Scripted variant (PythonScript plugin):**

```python
import Npp, ctypes

editor = Npp.editor
clipboard_text = Npp.notepad.getClipboardText()  # or use win32clipboard
editor.rereplace("YOUR_SEARCH_TERM", clipboard_text)
```

This is useful when the clipboard content itself changes between iterations (e.g. in a macro loop).

---

### Example C — Replace a Code Snippet Across All Files in a Directory Using Filters

**Use case:** Rename a function or update an import path across an entire codebase.

1. Press `Ctrl+Shift+F` to open **Find in Files**.
2. Fill in:

| Field | Value |
|---|---|
| **Find what** | `require\(['"]lodash['"]\)` |
| **Replace with** | `require('lodash-es')` |
| **Filters** | `*.js, *.ts, *.jsx, *.tsx` |
| **Directory** | `C:\Projects\MyApp\src` |
| **In all sub-folders** | ✅ |
| **Search Mode** | Regular Expression |

3. Click **Find All** first to **preview** every match in the Search Results window.
4. Confirm the results look correct.
5. Click **Replace in Files** to apply the change on disk across all matched files.

> ⚠️ **Always commit or back up before Replace in Files.** Changes are written directly to disk and cannot be undone within Notepad++.

---

### Example D — Mark All Error Lines and Extract Them

1. Open a large log file.
2. Press `Ctrl+M` (Mark tab).
3. **Find what:** `\[ERROR\]`
4. **Search Mode:** Regular Expression
5. Tick **Bookmark line**.
6. Click **Mark All** — every error line gets highlighted and bookmarked.
7. Go to **Search > Bookmark > Copy Bookmarked Lines**.
8. Open a new tab (`Ctrl+N`) and paste — you now have a clean error-only view.

---

### Example E — Find a Multi-word Snippet Across a Project

1. Set up a Project Panel listing your project files.
2. Right-click the project root node → **Find in Projects**.
3. **Find what:** `deprecated_function\(`
4. **Filters:** `*.py`
5. Click **Find All** — the Search Results window shows file path, line number, and context for every call site.

---

## 9. Caveats for Very Large Files

- **Regex on files > ~50 MB** can be slow or fail. Notepad++ loads the entire file into memory; regex backtracking on huge inputs is expensive.
- **Replace in Files** rewrites each matched file to disk atomically per file — but there is no transaction rollback if something goes wrong mid-batch.
- The **Find what** field is limited to **16,383 bytes** (v8.8.4+). Multi-line paste into the field is supported from v8.8.4.
- Regex does **not** support cross-file pattern matching (e.g. matching a token in file A against a list in file B). For that, use scripting plugins or external tools (see §10).

---

## 10. Better Alternatives for Extreme Scale

If your project is very large (millions of lines, hundreds of files, binary files, or cross-file logic), consider these tools alongside or instead of Notepad++:

| Tool | Best For |
|---|---|
| **`grep` / `ripgrep` (`rg`)** | Blazing-fast search across massive codebases from the command line; `rg` is 10–100× faster than grep on most workloads |
| **`sed`** | Stream-based find-and-replace on files of any size; no memory limit |
| **`awk`** | Field-aware text processing; ideal for structured data |
| **VS Code** (with multi-root workspaces) | GUI-based Find in Files with live preview, regex, and git-aware scoping |
| **JetBrains IDEs** | Structural Search & Replace — understands code semantics, not just text |
| **PowerShell `Get-Content` / `-replace`** | Windows-native scripted bulk replacement |
| **Python `pathlib` + `re`** | Fully scriptable; handles encoding, binary files, complex logic |
| **`fd` + `xargs` + `sed`** | UNIX pipeline for filtered, recursive replacement with fine control |

**Recommended ripgrep example** (equivalent to Notepad++ Find in Files with filters):

```bash
# Search only .js and .ts files recursively, show context
rg "require\('lodash'\)" --type js -C 2

# Replace across files (pipe to sed)
rg -l "require\('lodash'\)" --type js | xargs sed -i "s/require('lodash')/require('lodash-es')/g"
```

---

## Quick Keyboard Reference

| Shortcut | Action |
|---|---|
| `Ctrl+F` | Open Find tab |
| `Ctrl+H` | Open Replace tab |
| `Ctrl+Shift+F` | Open Find in Files tab |
| `Ctrl+M` | Open Mark tab |
| `F3` | Find Next |
| `Shift+F3` | Find Previous |
| `Ctrl+Alt+I` | Incremental Search |
| `Ctrl+Shift+R` | Start/stop macro recording |
| `Ctrl+Shift+P` | Play back macro |
| `F2` | Next Bookmark |
| `Shift+F2` | Previous Bookmark |
| `Ctrl+F2` | Toggle Bookmark on current line |

---

*Last updated from official source: [npp-user-manual.org/docs/searching](https://npp-user-manual.org/docs/searching/)*
