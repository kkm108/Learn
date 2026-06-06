# ripgrep (`rg`) — Complete Practical Guide

> **Primary source:** [BurntSushi/ripgrep GUIDE.md](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md)
> and [Official User Manual](https://iepathos.github.io/ripgrep/)
> **Latest stable release:** 14.x

---

## Table of Contents

1. [What is ripgrep?](#1-what-is-ripgrep)
2. [Installation](#2-installation)
3. [Basics — First Searches](#3-basics--first-searches)
4. [Recursive Search](#4-recursive-search)
5. [Automatic Filtering](#5-automatic-filtering)
6. [Manual Filtering: Globs (`-g`)](#6-manual-filtering-globs--g)
7. [Manual Filtering: File Types (`-t` / `-T`)](#7-manual-filtering-file-types--t---t)
8. [Case Sensitivity](#8-case-sensitivity)
9. [Context Lines (`-A`, `-B`, `-C`)](#9-context-lines--a--b--c)
10. [Output Modes & Formatting](#10-output-modes--formatting)
11. [Replacements (`-r` / `--replace`)](#11-replacements--r----replace)
12. [Multiline Search (`-U`)](#12-multiline-search--u)
13. [PCRE2 Mode (`-P`) — Advanced Regex](#13-pcre2-mode--p--advanced-regex)
14. [Search Across Compressed Files (`-z`)](#14-search-across-compressed-files--z)
15. [Binary File Handling](#15-binary-file-handling)
16. [File Encoding (`-E`)](#16-file-encoding--e)
17. [JSON Output (`--json`)](#17-json-output---json)
18. [Statistics (`--stats`)](#18-statistics---stats)
19. [Configuration File (`RIPGREP_CONFIG_PATH`)](#19-configuration-file-ripgrep_config_path)
20. [Piping & Integration with Other Tools](#20-piping--integration-with-other-tools)
21. [Practical Real-World Examples](#21-practical-real-world-examples)
22. [Quick Flag Reference](#22-quick-flag-reference)

---

## 1. What is ripgrep?

ripgrep (`rg`) is a line-oriented search tool that recursively searches your current directory for a regex pattern. It is built on Rust's regex engine (finite automata + SIMD + aggressive literal optimizations), making it typically 10–100× faster than `grep` on large codebases.

**Key design decisions:**

- Respects `.gitignore`, `.ignore`, and `.rgignore` automatically
- Skips hidden files and binary files by default
- Supports Unicode out of the box
- Never modifies files — `--replace` only affects output, not disk

---

## 2. Installation

```bash
# macOS (Homebrew)
brew install ripgrep

# Ubuntu / Debian (official package)
curl -LO https://github.com/BurntSushi/ripgrep/releases/download/14.1.1/ripgrep_14.1.1-1_amd64.deb
sudo dpkg -i ripgrep_14.1.1-1_amd64.deb

# Ubuntu 18.10+ (apt)
sudo apt-get install ripgrep

# Windows (winget)
winget install BurntSushi.ripgrep.MSVC

# Windows (Chocolatey)
choco install ripgrep

# Rust (from source, with PCRE2)
cargo install ripgrep --features pcre2

# Verify
rg --version
```

Shell completions are included in every binary release:

```bash
# Bash
cp rg.bash $XDG_CONFIG_HOME/bash_completion/

# Zsh
cp _rg /usr/local/share/zsh/site-functions/

# Fish
cp rg.fish ~/.config/fish/completions/
```

---

## 3. Basics — First Searches

### Literal search in a single file

```bash
rg fast README.md
# Output: line_number:matched_line
```

### Regex search in a single file

```bash
# Match "fast" followed by 1+ word characters (e.g. "faster")
rg 'fast\w+' README.md

# Match "fast" followed by 0+ word characters (matches "fast" too)
rg 'fast\w*' README.md
```

### Literal string search (disable regex)

```bash
# -F / --fixed-strings treats the pattern as a plain string
rg -F 'fn write(' src/
```

### Whole-word matching

```bash
# Matches "error" but not "errors" or "terrible"
rg -w error
```

### Count matches only

```bash
rg -c 'TODO' src/
# Output: filename:count (one line per file)
```

### Count total matches across all files

```bash
rg --count-matches 'TODO' src/
```

---

## 4. Recursive Search

Without a path argument, `rg` searches the current directory recursively:

```bash
rg 'fn write\('         # searches ./ recursively
rg 'fn write\(' src/    # restricts to src/ subtree
rg 'fn write\(' src/ tests/   # multiple paths
```

### Limit recursion depth

```bash
rg pattern --max-depth 2
```

### Search a specific set of files

```bash
rg pattern file1.txt file2.txt file3.log
```

---

## 5. Automatic Filtering

By default, ripgrep **ignores**:

| Category | What is ignored |
|---|---|
| gitignore rules | `.gitignore`, `$GIT_DIR/info/exclude`, `core.excludesFile` |
| `.ignore` files | App-agnostic ignore rules (override `.gitignore`) |
| `.rgignore` files | ripgrep-specific rules (highest precedence) |
| Hidden files | Anything starting with `.` |
| Binary files | Files containing a NUL byte |
| Symlinks | Not followed by default |

### Override filtering flags

```bash
rg pattern --no-ignore          # ignore all .gitignore / .ignore rules
rg pattern --hidden             # include hidden files and dirs (alias: -.)
rg pattern --text               # search binary files as text (alias: -a)
rg pattern --follow             # follow symbolic links (alias: -L)
rg pattern -u                   # disable .gitignore handling
rg pattern -uu                  # -u + search hidden files
rg pattern -uuu                 # -uu + search binary files
```

### Override .gitignore for a specific directory

Create a `.ignore` file alongside your `.gitignore`:

```
# .ignore — whitelist the log/ directory that .gitignore excludes
!log/
```

### Add ripgrep-specific ignore rules

```
# .rgignore — skip generated output even during rg searches
dist/
*.min.js
*.map
```

---

## 6. Manual Filtering: Globs (`-g`)

```bash
# Only search TOML files
rg clap -g '*.toml'

# Exclude TOML files
rg clap -g '!*.toml'

# Only search files in a specific subdirectory
rg pattern -g 'src/**'

# Multiple globs (later overrides earlier)
rg pattern -g '*.{js,ts}' -g '!*.test.ts'

# Exclude an entire directory by name
rg pattern -g '!node_modules'
```

**Tip:** Always quote glob patterns in single quotes (`'*.toml'`) to prevent your shell from expanding `*` before ripgrep sees it.

---

## 7. Manual Filtering: File Types (`-t` / `-T`)

File types are named aliases for one or more glob patterns.

```bash
# Include only Rust files
rg 'fn run' -t rust       # longhand: --type rust
rg 'fn run' -trust        # shorthand

# Exclude Rust files
rg pattern -T rust        # longhand: --type-not rust
rg pattern -Trust         # shorthand

# Multiple types
rg pattern -t js -t ts

# See all built-in type definitions
rg --type-list

# Common built-in types
# py, js, ts, rust, go, c, cpp, java, html, css, json, yaml, toml, md, sql, sh
```

### Define a custom type

```bash
# For a single command
rg --type-add 'web:*.{html,css,js}' -t web 'viewport'

# For all commands — add to your config file or shell alias
alias rg="rg --type-add 'web:*.{html,css,js}'"
```

### The special `all` type

```bash
rg pattern --type all     # match any file with a recognised extension
rg pattern --type-not all # match files with NO recognised extension
```

---

## 8. Case Sensitivity

```bash
rg pattern           # case-sensitive (default)
rg pattern -i        # case-insensitive (--ignore-case)
rg pattern -s        # force case-sensitive even if config says otherwise (--case-sensitive)
rg pattern -S        # smart-case: insensitive unless pattern has an uppercase letter (--smart-case)
```

Smart-case example:

```bash
rg -S 'error'    # matches: error, Error, ERROR
rg -S 'Error'    # matches: Error only (uppercase present → case-sensitive)
```

---

## 9. Context Lines (`-A`, `-B`, `-C`)

```bash
rg pattern -A 3    # 3 lines After each match
rg pattern -B 3    # 3 lines Before each match
rg pattern -C 3    # 3 lines before and after (--context 3)
```

When two match windows overlap, ripgrep merges them (no duplicate lines, no `--` separator between them).

### Passthrough mode — show entire file with matches highlighted

```bash
rg pattern --passthru    # prints every line; matched lines are highlighted
```

### Show only filenames

```bash
rg pattern -l            # --files-with-matches
rg pattern -L            # --files-without-match
```

### Limit matches per file

```bash
rg pattern -m 5          # --max-count: stop after 5 matches per file
```

---

## 10. Output Modes & Formatting

### Show only the matching portion (not the whole line)

```bash
rg 'fast\w+' README.md -o   # --only-matching
```

### Suppress filenames (useful when searching a single file)

```bash
rg pattern file.txt --no-filename    # alias: -I
```

### Show filenames even when searching a single file

```bash
rg pattern file.txt --with-filename  # alias: -H
```

### Show 0-based byte offset of each match

```bash
rg pattern -b    # --byte-offset
```

### Null-separated output (safe for `xargs`)

```bash
rg pattern -l --null | xargs -0 wc -l
```

### Sort output (disables parallelism)

```bash
rg pattern --sort path       # alphabetical by file path
rg pattern --sort modified   # by last modification time
rg pattern --sortr path      # reverse alphabetical
```

### Column number of match start

```bash
rg pattern --column
```

### Print each match on a separate line (even multiple matches per line)

```bash
rg pattern --vimgrep    # format: file:line:col:text — ideal for Vim quickfix
```

### Hyperlinks (clickable paths in supported terminals)

```bash
rg pattern --hyperlink-format vscode    # opens file in VS Code on click
rg pattern --hyperlink-format file      # standard file:// URL
```

---

## 11. Replacements (`-r` / `--replace`)

> ⚠️ ripgrep **never modifies files on disk**. `--replace` changes output only.
> To apply replacements on disk, pipe to `sed -i` or use a shell loop (see §20).

### Simple literal replacement

```bash
rg fast README.md -r FAST
# Output shows matched text replaced; file is unchanged
```

### Replace an entire matching line

```bash
rg '^.*fast.*$' README.md -r '[redacted]'
```

### Replace with a capture group (backreference)

```bash
# Pattern: fast followed by whitespace and a word
# Replacement: join them with a dash
rg 'fast\s+(\w+)' README.md -r 'fast-$1'

# Named capture group
rg 'fast\s+(?P<word>\w+)' README.md -r 'fast-$word'
```

### Replace only the matched portion, suppress context

```bash
rg fast README.md -or FAST    # -o (only-matching) + -r (replace)
```

### Apply replacement to disk using a pipeline

```bash
# Single file
rg 'require\("lodash"\)' src/app.js -r "require('lodash-es')" \
  | sponge src/app.js            # requires moreutils

# All .js files (via sed)
rg -l 'require\("lodash"\)' --type js \
  | xargs sed -i "s/require(\"lodash\")/require('lodash-es')/g"

# Cross-platform (using Perl)
rg -l 'require\("lodash"\)' --type js \
  | xargs perl -pi -e 's/require\("lodash"\)/require("lodash-es")/g'
```

### Multiline replacement (with `--passthru`)

```bash
# Compress 3+ consecutive blank lines to 1, output to stdout
rg --passthru -U '(\n){3,}' -r $'\n\n' file.txt

# CRLF-aware version
printf 'line1\r\nline2\r\n' | rg --passthru --crlf '\w+$' -r 'xyz'
```

---

## 12. Multiline Search (`-U`)

By default, `.` never matches newlines. `-U` enables multiline mode so patterns can span lines.

```bash
# Find a function header followed by its opening brace on the next line
rg -U 'fn \w+\(.*\)\s*\n\s*\{'

# Match "ERROR" on one line and a stack trace on the next
rg -U 'ERROR.*\n.*stack trace'

# (?s) flag: make . match newlines (multiline dotall)
rg -U '(?s)start.*end'

# Match across CRLF line endings
rg -U --crlf 'pattern'
```

---

## 13. PCRE2 Mode (`-P`) — Advanced Regex

ripgrep's default engine is Rust's `regex` crate (no backtracking = very fast). Use `-P` for PCRE2 when you need features unavailable in the default engine.

```bash
# Look-ahead: find a function that calls a specific API
rg -P 'fn \w+(?=.*my_api)'

# Look-behind: find imports not preceded by a comment
rg -P '(?<!#\s)^import'

# Backreferences: find duplicate words
rg -P '\b(\w+)\s+\1\b'

# Named backreference in same pattern
rg -P '(?P<tag><\w+>).*(?P=tag)'

# Multiline + PCRE2
rg -PU 'class \w+ \{.*?^\}' --multiline-dotall
```

Check if your build includes PCRE2:

```bash
rg --pcre2-version    # prints version or error if not compiled in
```

---

## 14. Search Across Compressed Files (`-z`)

```bash
rg pattern -z             # --search-zip
# Supports: gzip, bzip2, xz, lzma, lz4, Brotli, Zstd
# Requires the corresponding decompression binaries on PATH
```

**Note:** Archive formats (`.tar.gz`, `.zip`) are not searched — only individually compressed files.

---

## 15. Binary File Handling

ripgrep has three modes for binary files:

| Mode | Flag | Behaviour |
|---|---|---|
| Default | (none) | Stops searching as soon as a NUL byte is found; prints a warning |
| Binary mode | `--binary` | Continues searching but stops at first match; avoids dumping raw bytes |
| Text mode | `-a` / `--text` | Searches all bytes as if they were text |

```bash
rg pattern --binary    # discover if binary files have matches
rg pattern -a          # treat every file as text (careful with terminals)
```

---

## 16. File Encoding (`-E`)

```bash
rg pattern -E utf-16       # transcode UTF-16 → UTF-8 before searching
rg pattern -E latin1       # search latin-1 encoded files
rg pattern -E none         # disable all encoding logic; search raw bytes
```

ripgrep auto-detects UTF-16 BOM by default. For other encodings, specify explicitly. Full list of supported encodings follows the [Encoding Standard](https://encoding.spec.whatwg.org/).

---

## 17. JSON Output (`--json`)

Machine-readable output — each line is a newline-delimited JSON object.

```bash
rg pattern --json
```

Message types emitted:

| Type | Fields | Description |
|---|---|---|
| `begin` | `path` | Start of results for a file |
| `match` | `path`, `lines`, `line_number`, `absolute_offset`, `submatches` | A match |
| `context` | `path`, `lines`, `line_number` | A context line |
| `end` | `path`, `binary_offset`, `stats` | End of results for a file |
| `summary` | `elapsed_total`, `stats` | Final summary across all files |

```bash
# Pretty-print with jq
rg pattern --json | jq .

# Extract only file paths of matches
rg pattern --json | jq -r 'select(.type=="match") | .data.path.text'

# Count matches per file
rg pattern --json | jq -r 'select(.type=="match") | .data.path.text' | sort | uniq -c | sort -rn
```

---

## 18. Statistics (`--stats`)

```bash
rg pattern --stats
# Prints at the end:
# N matches
# N matched lines
# N files contained matches
# N files searched
# N bytes printed
# N bytes searched
# Elapsed: X.XXXs
```

Combine with `--json` for structured stats:

```bash
rg pattern --json --stats | jq 'select(.type=="summary")'
```

---

## 19. Configuration File (`RIPGREP_CONFIG_PATH`)

ripgrep does not look in a default location. You must set an environment variable:

```bash
export RIPGREP_CONFIG_PATH="$HOME/.ripgreprc"
```

### Example `~/.ripgreprc`

```
# Truncate very long lines in terminal output; show a preview
--max-columns=150
--max-columns-preview

# Smart case by default
--smart-case

# Always show line numbers
--line-number

# Custom "web" file type
--type-add
web:*.{html,css,js,ts,jsx,tsx}

# Exclude generated/vendor directories from every search
--glob=!node_modules
--glob=!dist
--glob=!.git
--glob=!vendor
--glob=!*.min.js
--glob=!*.map

# Nicer colours
--colors=match:bg:yellow
--colors=match:fg:black
--colors=line:style:bold
```

### Overriding config at the command line

Config is prepended to command-line arguments, so flags given on the command line win:

```bash
rg pattern -M0    # override --max-columns from config (0 = unlimited)
rg pattern --no-config   # ignore the config file entirely this time
```

Debug which config is being read:

```bash
rg pattern --debug 2>&1 | head -20
```

---

## 20. Piping & Integration with Other Tools

### Feed results to `xargs`

```bash
# Word-count every file that matches
rg pattern -l --null | xargs -0 wc -l

# Open every matching file in Vim
rg pattern -l | xargs vim
```

### Pipeline with `sed` for in-place replacement

```bash
rg -l 'oldFunction' --type js \
  | xargs sed -i 's/oldFunction/newFunction/g'
```

### Combine `fd` (fast find) + `rg`

```bash
# Find all Python files modified in the last day, search them
fd -e py --changed-within 1d | xargs rg 'TODO'
```

### Feed into `fzf` for interactive selection

```bash
rg pattern --vimgrep | fzf --delimiter ':' --nth 4..
```

### Use as a Vim/Neovim `:grep` backend

```vim
set grepprg=rg\ --vimgrep\ --smart-case
set grepformat=%f:%l:%c:%m
```

### Sort results for deterministic output

```bash
rg pattern --sort path    # sorted but single-threaded
```

### Preprocessor — run a command on each file before searching

```bash
# Search inside PDFs by piping through pdftotext
rg pattern --pre pdftotext
```

---

## 21. Practical Real-World Examples

### Find all TODO/FIXME/HACK comments in a project

```bash
rg -i 'TODO|FIXME|HACK|XXX' --type-add 'code:*.{js,ts,py,go,rs,java}' -t code
```

### Find all function definitions in Python

```bash
rg '^def \w+' -t py
```

### Find all class definitions across multiple languages

```bash
rg '^\s*(public\s+)?(class|struct|interface) \w+' -t java -t ts -t go
```

### Rename a symbol across an entire codebase (preview first)

```bash
# Step 1: preview
rg 'OldClassName' --type ts

# Step 2: apply with sed
rg -l 'OldClassName' --type ts \
  | xargs sed -i 's/OldClassName/NewClassName/g'
```

### Find all files importing a specific module

```bash
rg "from 'react'" -t ts -l
rg "import React" -t ts -l
```

### Search for a multi-word string across all log files

```bash
rg -F 'Connection refused' /var/log/ -t log -g '*.log' --sort modified
```

### Find duplicate words in prose

```bash
rg -P '\b(\w+)\s+\1\b' -i *.md
```

### Find lines longer than N characters

```bash
rg '.{120,}' --type md    # lines > 120 chars in Markdown files
```

### Find all IP addresses in config files

```bash
rg '\b\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}\b' -g '*.conf' -g '*.yaml' -g '*.toml'
```

### Find hardcoded credentials (basic scan)

```bash
rg -i '(password|secret|api_key|token)\s*[:=]\s*["\x27][^\s"]{6,}' \
   --type-not md --type-not txt
```

### Extract all URLs from source files

```bash
rg -o 'https?://[^\s"'"'"'<>)]+' --type js --type html | sort -u
```

### Find all `console.log` calls and list the files

```bash
rg 'console\.log' -t js -t ts -l
```

### Search compressed log archives

```bash
rg 'OutOfMemoryError' /var/log/app/ -z
```

### Count occurrences of each HTTP status code in a log

```bash
rg -o 'HTTP/[0-9.]+" [0-9]{3}' access.log \
  | rg -o '[0-9]{3}$' | sort | uniq -c | sort -rn
```

### Find all environment variable usages

```bash
rg 'process\.env\.\w+' -t js -t ts -o | sort -u
```

### Search only recently changed files (combine with `git`)

```bash
git diff --name-only HEAD~5 | xargs rg 'pattern'
```

### Find all TODO items with the author name (if using VSCode-style annotations)

```bash
rg 'TODO\((\w+)\)' -r 'TODO by $1' --type py
```

---

## 22. Quick Flag Reference

### Search behaviour

| Flag | Long form | Description |
|---|---|---|
| `-F` | `--fixed-strings` | Literal string, no regex |
| `-w` | `--word-regexp` | Match whole words only |
| `-x` | `--line-regexp` | Entire line must match |
| `-i` | `--ignore-case` | Case-insensitive |
| `-s` | `--case-sensitive` | Force case-sensitive |
| `-S` | `--smart-case` | Case-insensitive unless uppercase present |
| `-U` | `--multiline` | Allow patterns to span lines |
| `-P` | `--pcre2` | Use PCRE2 engine (look-around, backrefs) |
| `-e PAT` | `--regexp=PAT` | Specify multiple patterns |
| `-f FILE` | `--file=FILE` | Read patterns from a file |

### Output formatting

| Flag | Long form | Description |
|---|---|---|
| `-n` | `--line-number` | Show line numbers (default when tty) |
| `-N` | `--no-line-number` | Suppress line numbers |
| `-H` | `--with-filename` | Show filename even for single file |
| `-I` | `--no-filename` | Suppress filename |
| `-o` | `--only-matching` | Print only the matched part |
| `-r TEXT` | `--replace=TEXT` | Replace match in output |
| `-c` | `--count` | Print count of matching lines per file |
|  | `--count-matches` | Print count of matches (not lines) |
| `-l` | `--files-with-matches` | Print filenames with at least one match |
| `-L` | `--files-without-match` | Print filenames with no matches |
| `-m N` | `--max-count=N` | Stop after N matches per file |
| `-A N` | `--after-context=N` | N lines after match |
| `-B N` | `--before-context=N` | N lines before match |
| `-C N` | `--context=N` | N lines before and after |
|  | `--passthru` | Print every line; highlight matches |
| `-b` | `--byte-offset` | Print byte offset of match |
|  | `--column` | Print column number of match |
|  | `--vimgrep` | `file:line:col:text` format for Vim |
|  | `--json` | Machine-readable JSON output |
|  | `--stats` | Print search statistics at end |
|  | `--sort PATH` | Sort output by path (single-threaded) |

### File filtering

| Flag | Long form | Description |
|---|---|---|
| `-g GLOB` | `--glob=GLOB` | Include/exclude by glob (`!` to negate) |
| `-t TYPE` | `--type=TYPE` | Include file type |
| `-T TYPE` | `--type-not=TYPE` | Exclude file type |
|  | `--type-add` | Define a new file type |
|  | `--type-list` | List all file type definitions |
| `-u` | `--unrestricted` | Disable .gitignore (stack for more) |
|  | `--no-ignore` | Disable all ignore files |
|  | `--hidden` | Search hidden files and dirs |
| `-a` | `--text` | Search binary files as text |
| `-L` | `--follow` | Follow symbolic links |
| `-z` | `--search-zip` | Search compressed files |
| `-E ENC` | `--encoding=ENC` | Specify file encoding |
|  | `--max-depth=N` | Limit directory recursion depth |
|  | `--pre CMD` | Preprocess each file with CMD |

### Miscellaneous

| Flag | Description |
|---|---|
| `--debug` | Show debug info (what config/ignores are loaded) |
| `--no-config` | Ignore `RIPGREP_CONFIG_PATH` |
| `--null` / `-0` | NUL-separated output (for xargs -0) |
| `--line-buffered` | Flush output per line (useful in pipelines) |
| `--hyperlink-format` | Make file paths clickable in terminal |
| `--pcre2-version` | Check if PCRE2 is compiled in |
| `--type-list` | List all built-in file types |

---

## Appendix: Regex Quick Reference (Default Engine)

| Pattern | Matches |
|---|---|
| `.` | Any character except newline |
| `\d` | Digit `[0-9]` |
| `\D` | Non-digit |
| `\w` | Word char `[a-zA-Z0-9_]` (Unicode-aware) |
| `\W` | Non-word char |
| `\s` | Whitespace |
| `\S` | Non-whitespace |
| `^` | Start of line |
| `$` | End of line |
| `\b` | Word boundary |
| `*` | 0 or more (greedy) |
| `+` | 1 or more (greedy) |
| `?` | 0 or 1 (greedy) |
| `*?` `+?` | Non-greedy quantifiers |
| `{n,m}` | Between n and m repetitions |
| `(abc)` | Capture group (ref as `$1` in `-r`) |
| `(?P<name>…)` | Named capture group (ref as `$name`) |
| `[abc]` | Character class |
| `[^abc]` | Negated character class |
| `a\|b` | Alternation (a or b) |
| `(?i)` | Inline case-insensitive flag |
| `(?s)` | Inline dot-matches-newline flag |
| `(?-u)` | Disable Unicode for this sub-expression |

Full regex syntax: <https://docs.rs/regex/latest/regex/#syntax>

---

*Sources: [BurntSushi/ripgrep GUIDE.md](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md) · [ripgrep User Manual](https://iepathos.github.io/ripgrep/) · [Debian man page](https://manpages.debian.org/testing/ripgrep/rg.1.en.html) · [learnbyexample substitution guide](https://learnbyexample.github.io/substitution-with-ripgrep/)*
