# 🔬 Windows Power User Audit Guide
### Advanced: Shadow Installs · Unknown Binaries · iwr|iex Drop Zones · Nuclear Dry Audit

> **Companion to:** `WINDOWS_INSTALL_AUDIT_README.md`  
> **Level:** Power user / forensic-level auditing  
> **Shell:** PowerShell 5.1+ or PowerShell 7+ (pwsh)

---

## 📋 Table of Contents

1. [The Mental Model: How Installers Think](#1-the-mental-model-how-installers-think)
2. [The Actual Audit Zones — Definitive Map](#2-the-actual-audit-zones--definitive-map)
3. [iwr | iex — What Really Drops Where](#3-iwr--iex--what-really-drops-where)
4. [Find All CLI Tools in PATH](#4-find-all-cli-tools-in-path)
5. [Find Unknown Binaries by Age](#5-find-unknown-binaries-by-age)
6. [Detect Shadow Installs](#6-detect-shadow-installs)
7. [The Nuclear Dry Audit — Annotated & Improved](#7-the-nuclear-dry-audit--annotated--improved)
8. [Registry Audit — What Installers Register](#8-registry-audit--what-installers-register)
9. [Correlate Binary → Installer → Drop Zone](#9-correlate-binary--installer--drop-zone)
10. [Forensic Timeline: What Was Installed When](#10-forensic-timeline-what-was-installed-when)
11. [Power User One-Liner Collection](#11-power-user-one-liner-collection)
12. [The Full Power Audit Script](#12-the-full-power-audit-script)

---

## 1. The Mental Model: How Installers Think

Before hunting, understand the *decision tree* every installer follows:

```
Installer runs as...
│
├── ADMIN (elevated)?
│   ├── Drops to:  C:\Program Files\
│   │              C:\ProgramData\
│   │              HKLM registry keys
│   │              System PATH (Machine-level)
│   └── Examples:  Node.js .msi, Chocolatey, Git for Windows
│
└── USER (no elevation)?
    ├── Drops to:  %APPDATA%         (Roaming — syncs across domain PCs)
    │              %LOCALAPPDATA%    (Local — this machine only)
    │              %USERPROFILE%\.<toolname>   (dotfolders)
    │              %USERPROFILE%\bin\
    │              HKCU registry keys
    │              User PATH only
    └── Examples:  bun, deno, scoop, fnm, most iwr|iex scripts
```

> 💡 **Key rule:** If you *didn't* right-click "Run as Administrator," the install is entirely contained in your user profile. That's also why these installs are *invisible* to Add/Remove Programs.

### Why "shadow installs" happen

A shadow install is any binary reachable from the command line that:
- Was **never listed** in Add/Remove Programs
- **Bypassed** your primary package manager
- Lives in a PATH directory you **didn't knowingly add**
- Was **left behind** by an npx run, a CI script, or a forgotten one-liner

---

## 2. The Actual Audit Zones — Definitive Map

This is the authoritative drop-zone map. Every `iwr|iex`, `npm -g`, `scoop install`, etc. lands in one of these.

```
AUDIT ZONE MAP — Windows
══════════════════════════════════════════════════════════════════════

ZONE A — User Roaming Profile  (%APPDATA%)
  Roams with your account on domain-joined machines.
  Path: C:\Users\<you>\AppData\Roaming\
  ┌──────────────────────────────────────────────────────────────┐
  │  npm\               ← npm global packages + binaries         │
  │  npm-cache\         ← npm download cache (older npm)         │
  │  nvm\               ← nvm-windows versions + shims           │
  │  pnpm\              ← pnpm globals (older pnpm setups)       │
  │  Volta\             ← Volta on some configurations           │
  │  Python\            ← pip user installs sometimes land here  │
  └──────────────────────────────────────────────────────────────┘

ZONE B — User Local Profile  (%LOCALAPPDATA%)
  Local to THIS machine only. Not synced.
  Path: C:\Users\<you>\AppData\Local\
  ┌──────────────────────────────────────────────────────────────┐
  │  pnpm\              ← pnpm home (modern default)             │
  │  npm-cache\         ← npm cache (modern default)             │
  │  Volta\             ← Volta home (most installs)             │
  │  fnm\               ← fnm Node version manager               │
  │  Programs\          ← Some user-space .exe installers        │
  │  Microsoft\WindowsApps\  ← winget-installed CLIs            │
  │  node\corepack\     ← corepack download cache               │
  └──────────────────────────────────────────────────────────────┘

ZONE C — User Home Dotfolders  (%USERPROFILE%)
  Path: C:\Users\<you>\
  ┌──────────────────────────────────────────────────────────────┐
  │  .bun\bin\          ← bun runtime + bun-installed globals    │
  │  .deno\bin\         ← deno runtime                          │
  │  .cargo\bin\        ← Rust binaries (cargo install)         │
  │  .volta\            ← Volta alternate location               │
  │  .fnm\              ← fnm alternate location                 │
  │  .pnpm-store\       ← pnpm store (older default)            │
  │  .npm\              ← npm config / logs                      │
  │  scoop\             ← ALL scoop packages + shims             │
  │  scoop\shims\       ← ← THIS is what goes in PATH           │
  │  bin\               ← Manual drops, curl-downloaded bins     │
  └──────────────────────────────────────────────────────────────┘

ZONE D — System-Wide (Requires Admin)
  Path: C:\Program Files\, C:\ProgramData\
  ┌──────────────────────────────────────────────────────────────┐
  │  C:\Program Files\nodejs\    ← Official Node.js installer    │
  │  C:\Program Files\Git\       ← Git for Windows               │
  │  C:\ProgramData\chocolatey\  ← Chocolatey + all choco pkgs  │
  │  C:\ProgramData\chocolatey\bin\ ← choco shims in PATH       │
  │  C:\Program Files\PowerShell\   ← PowerShell 7               │
  └──────────────────────────────────────────────────────────────┘

ZONE E — Windows App Stores / Managed
  ┌──────────────────────────────────────────────────────────────┐
  │  %LOCALAPPDATA%\Microsoft\WindowsApps\  ← winget, MSIX apps │
  │  C:\Program Files\WindowsApps\          ← Store apps (UWP)  │
  └──────────────────────────────────────────────────────────────┘

ZONE F — PowerShell Modules
  ┌──────────────────────────────────────────────────────────────┐
  │  %USERPROFILE%\Documents\PowerShell\Modules\  ← User mods   │
  │  C:\Program Files\PowerShell\Modules\         ← System mods │
  │  C:\Windows\System32\WindowsPowerShell\v1.0\Modules\        │
  └──────────────────────────────────────────────────────────────┘
```

### Quick zone check — all at once

```powershell
$zones = [ordered]@{
    "ZONE A — npm globals"          = "$env:APPDATA\npm"
    "ZONE A — npm-cache (old)"      = "$env:APPDATA\npm-cache"
    "ZONE A — nvm-windows"          = "$env:APPDATA\nvm"
    "ZONE B — pnpm home"            = "$env:LOCALAPPDATA\pnpm"
    "ZONE B — npm-cache (new)"      = "$env:LOCALAPPDATA\npm-cache"
    "ZONE B — Volta"                = "$env:LOCALAPPDATA\Volta"
    "ZONE B — fnm"                  = "$env:LOCALAPPDATA\fnm"
    "ZONE B — WinApps (winget)"     = "$env:LOCALAPPDATA\Microsoft\WindowsApps"
    "ZONE C — bun"                  = "$env:USERPROFILE\.bun"
    "ZONE C — deno"                 = "$env:USERPROFILE\.deno"
    "ZONE C — cargo (Rust)"         = "$env:USERPROFILE\.cargo"
    "ZONE C — scoop"                = "$env:USERPROFILE\scoop"
    "ZONE C — scoop shims"          = "$env:USERPROFILE\scoop\shims"
    "ZONE C — user bin"             = "$env:USERPROFILE\bin"
    "ZONE D — Node.js (system)"     = "C:\Program Files\nodejs"
    "ZONE D — Chocolatey"           = "C:\ProgramData\chocolatey"
    "ZONE F — PS Modules (user)"    = "$env:USERPROFILE\Documents\PowerShell\Modules"
}

foreach ($label in $zones.Keys) {
    $path = $zones[$label]
    $exists = Test-Path $path
    $icon = if ($exists) { "✅" } else { "  " }
    $size = if ($exists) {
        $b = (Get-ChildItem $path -Recurse -ErrorAction SilentlyContinue | Measure-Object Length -Sum).Sum
        "{0,8:N1} MB" -f ($b/1MB)
    } else { "        —  " }
    Write-Host ("{0} {1,-35} {2}  {3}" -f $icon, $label, $size, $path)
}
```

---

## 3. iwr | iex — What Really Drops Where

### 3.1 — The anatomy of a one-liner install

```powershell
# This pattern:
iwr https://example.com/install.ps1 | iex

# Is exactly equivalent to:
$script = Invoke-WebRequest "https://example.com/install.ps1"
Invoke-Expression $script.Content

# Which means: you are running arbitrary code with YOUR permissions.
# No sandbox. No dry-run. No undo.
```

### 3.2 — Safe inspection before running

```powershell
# Download and READ before running — always
$url = "https://bun.sh/install.ps1"   # example
$script = (Invoke-WebRequest $url -UseBasicParsing).Content

# View it
$script | Out-Host

# Or save and open in editor
$script | Out-File "$env:TEMP\inspect-install.ps1"
code "$env:TEMP\inspect-install.ps1"      # VS Code
notepad "$env:TEMP\inspect-install.ps1"   # Notepad
```

### 3.3 — Real drop-zone behavior by tool

| Tool | Command | APPDATA | LOCALAPPDATA | USERPROFILE dotfolder | PATH modified |
|---|---|:---:|:---:|:---:|:---:|
| **bun** | `irm bun.sh/install.ps1 \| iex` | — | — | `.bun\bin\` | User PATH |
| **deno** | `irm deno.land/install.ps1 \| iex` | — | — | `.deno\bin\` | User PATH |
| **Volta** | winget or `iwr…\|iex` | — | `Volta\` | — | User PATH |
| **fnm** | `winget install Schniz.fnm` | — | `fnm\` | — | *(manual)* |
| **Scoop** | `irm get.scoop.sh \| iex` | — | — | `scoop\` | User PATH |
| **pnpm** (standalone) | `iwr … \| iex` | — | `pnpm\` | — | User PATH |
| **nvm-windows** | `.exe` installer | `nvm\` | — | — | System PATH |
| **rustup** | `irm … \| iex` | — | — | `.cargo\bin\` | User PATH |
| **mise** | `irm … \| iex` | — | — | `.local\bin\` | User PATH |

### 3.4 — Do NOT assume these three always get written to:

> ❌ **Common misconception:** "iwr|iex always writes to %APPDATA%, %LOCALAPPDATA%, AND %APPDATA%\npm"

**Reality:**
- `%APPDATA%\npm` is **only written to by npm** — not by curl/iex scripts
- A script running as *user* will write to **Zone B or C** (LOCALAPPDATA or USERPROFILE dotfolder)
- A script running as *admin* will write to **Zone D** (Program Files or ProgramData)
- **Most modern tools prefer Zone C** (dotfolders in USERPROFILE) — cleaner, more Unix-like

### 3.5 — What a typical iwr|iex script actually does

Most well-behaved scripts follow this pattern. You can verify this yourself:

```
1. Detect OS/arch            → picks the right binary
2. Create install dir        → usually $env:USERPROFILE\.<toolname>
3. Download binary           → single .exe or .zip
4. Extract (if zip)          → into install dir
5. Modify PATH               → writes to HKCU registry (User PATH)
6. May create %USERPROFILE%\.<toolname>rc or config file
7. Done — NO system PATH change, NO elevation needed
```

### 3.6 — Detect PATH changes made by a script

```powershell
# Snapshot PATH before install
$beforePath = $env:PATH

# ... run your installer ...

# Compare after (in same session or read from registry)
$afterPath = [Environment]::GetEnvironmentVariable("PATH", "User")

# What was added?
$before = $beforePath -split ';'
$after  = $afterPath  -split ';'
Compare-Object $before $after | Where-Object SideIndicator -eq '=>' |
    Select-Object -ExpandProperty InputObject
```

---

## 4. Find All CLI Tools in PATH

### 4.1 — Every executable in every PATH directory

```powershell
# Full inventory: name, extension, directory, size, age
$env:PATH -split ';' |
    Where-Object { $_ -ne '' -and (Test-Path $_) } |
    ForEach-Object {
        Get-ChildItem $_ -File -Include *.exe,*.cmd,*.bat,*.ps1,*.sh -ErrorAction SilentlyContinue
    } |
    Select-Object Name,
                  @{N='Directory';  E={$_.DirectoryName}},
                  @{N='Size_KB';    E={[math]::Round($_.Length/1KB,1)}},
                  @{N='Modified';   E={$_.LastWriteTime.ToString("yyyy-MM-dd")}},
                  @{N='Age_Days';   E={(New-TimeSpan $_.LastWriteTime).Days}} |
    Sort-Object Name |
    Format-Table -AutoSize
```

### 4.2 — Just the names (quick scan)

```powershell
$env:PATH -split ';' |
    Where-Object { $_ -ne '' -and (Test-Path $_) } |
    ForEach-Object { Get-ChildItem $_ -File -Include *.exe,*.cmd -ErrorAction SilentlyContinue } |
    Select-Object -ExpandProperty BaseName |
    Sort-Object -Unique
```

### 4.3 — Find duplicate command names across PATH

> This reveals **shadowing** — when one tool hides another with the same name.

```powershell
$all = $env:PATH -split ';' |
    Where-Object { $_ -ne '' -and (Test-Path $_) } |
    ForEach-Object {
        Get-ChildItem $_ -File -Include *.exe,*.cmd,*.bat,*.ps1 -ErrorAction SilentlyContinue |
        Select-Object BaseName,
                      @{N='FullPath'; E={$_.FullName}},
                      @{N='Dir';      E={$_.DirectoryName}}
    }

# Group by name — any with Count > 1 are shadowed
$all | Group-Object BaseName |
    Where-Object Count -gt 1 |
    ForEach-Object {
        Write-Host "`n⚠️  SHADOW: $($_.Name)" -ForegroundColor Yellow
        $_.Group | ForEach-Object {
            $winner = (Get-Command $_.BaseName -ErrorAction SilentlyContinue).Source
            $tag = if ($_.FullPath -eq $winner) { "← ACTIVE (wins)" } else { "  (shadowed)" }
            Write-Host ("   {0,-55} {1}" -f $_.FullPath, $tag)
        }
    }
```

### 4.4 — Find CLI tools by keyword in name

```powershell
function Find-CLITool {
    param([string]$Keyword)
    $env:PATH -split ';' |
        Where-Object { $_ -ne '' -and (Test-Path $_) } |
        ForEach-Object {
            Get-ChildItem $_ -File -ErrorAction SilentlyContinue |
            Where-Object Name -Match $Keyword
        } |
        Select-Object Name, FullName, LastWriteTime
}

# Examples:
Find-CLITool "node"
Find-CLITool "npm|pnpm|yarn|bun"
Find-CLITool "cli|tool"
```

### 4.5 — Which PATH directories have the most binaries?

```powershell
$env:PATH -split ';' |
    Where-Object { $_ -ne '' -and (Test-Path $_) } |
    ForEach-Object {
        $dir = $_
        $count = (Get-ChildItem $dir -File -Include *.exe,*.cmd,*.bat -ErrorAction SilentlyContinue).Count
        [PSCustomObject]@{ Count = $count; Directory = $dir }
    } |
    Sort-Object Count -Descending |
    Format-Table -AutoSize
```

---

## 5. Find Unknown Binaries by Age

> The goal: find things installed *recently* that you didn't consciously put there.

### 5.1 — All executables in PATH modified in the last N days

```powershell
$days = 30   # Change to 7, 14, 90 as needed

$env:PATH -split ';' |
    Where-Object { $_ -ne '' -and (Test-Path $_) } |
    ForEach-Object {
        Get-ChildItem $_ -File -Include *.exe,*.cmd,*.bat,*.ps1 -ErrorAction SilentlyContinue |
        Where-Object LastWriteTime -gt (Get-Date).AddDays(-$days)
    } |
    Select-Object Name,
                  FullName,
                  @{N='Modified';  E={$_.LastWriteTime.ToString("yyyy-MM-dd HH:mm")}},
                  @{N='Days_Old';  E={(New-TimeSpan $_.LastWriteTime).Days}} |
    Sort-Object Modified -Descending |
    Format-Table -AutoSize
```

### 5.2 — All executables in audit zones, sorted newest first

```powershell
$auditZones = @(
    "$env:APPDATA\npm",
    "$env:LOCALAPPDATA\pnpm",
    "$env:LOCALAPPDATA\Volta",
    "$env:LOCALAPPDATA\fnm",
    "$env:USERPROFILE\.bun",
    "$env:USERPROFILE\.deno",
    "$env:USERPROFILE\.cargo",
    "$env:USERPROFILE\scoop\shims",
    "C:\ProgramData\chocolatey\bin"
)

$auditZones |
    Where-Object { Test-Path $_ } |
    ForEach-Object {
        Get-ChildItem $_ -File -Recurse -ErrorAction SilentlyContinue |
        Where-Object { $_.Extension -in '.exe','.cmd','.bat','.ps1','.sh' }
    } |
    Select-Object Name,
                  @{N='Zone';     E={$_.DirectoryName}},
                  @{N='Modified'; E={$_.LastWriteTime.ToString("yyyy-MM-dd")}},
                  @{N='Days_Old'; E={(New-TimeSpan $_.LastWriteTime).Days}} |
    Sort-Object Modified -Descending |
    Format-Table -AutoSize
```

### 5.3 — Binaries older than 2 years (cleanup candidates)

```powershell
$cutoff = (Get-Date).AddYears(-2)

$env:PATH -split ';' |
    Where-Object { $_ -ne '' -and (Test-Path $_) } |
    ForEach-Object {
        Get-ChildItem $_ -File -Include *.exe,*.cmd -ErrorAction SilentlyContinue |
        Where-Object LastWriteTime -lt $cutoff
    } |
    Select-Object Name,
                  @{N='Modified'; E={$_.LastWriteTime.ToString("yyyy-MM-dd")}},
                  FullName |
    Sort-Object Modified |
    Format-Table -AutoSize
```

### 5.4 — Timeline view: what was installed when

```powershell
# Build a timeline of all executables across all audit zones
$allZones = @(
    "$env:APPDATA\npm",
    "$env:LOCALAPPDATA\pnpm",
    "$env:LOCALAPPDATA\Volta\bin",
    "$env:USERPROFILE\.bun\bin",
    "$env:USERPROFILE\.deno\bin",
    "$env:USERPROFILE\.cargo\bin",
    "$env:USERPROFILE\scoop\shims"
)

$timeline = $allZones |
    Where-Object { Test-Path $_ } |
    ForEach-Object {
        $zone = $_
        Get-ChildItem $zone -File -ErrorAction SilentlyContinue |
        Where-Object { $_.Extension -in '.exe','.cmd','.bat' } |
        Select-Object @{N='Tool';     E={$_.BaseName}},
                      @{N='Zone';     E={$zone}},
                      @{N='Date';     E={$_.LastWriteTime.Date}},
                      @{N='DateTime'; E={$_.LastWriteTime}}
    } |
    Sort-Object DateTime -Descending

# Group by date
$timeline | Group-Object Date | ForEach-Object {
    Write-Host "`n📅 $($_.Name)" -ForegroundColor Cyan
    $_.Group | ForEach-Object {
        Write-Host ("   {0,-25} {1}" -f $_.Tool, $_.Zone)
    }
}
```

### 5.5 — Find binaries with no corresponding package manager record

```powershell
# Get npm global package names
$npmPkgs = (npm list -g --depth=0 --json 2>$null | ConvertFrom-Json).dependencies.PSObject.Properties.Name

# Get binaries in npm bin dir
$npmBinDir = npm bin -g 2>$null
$npmBins = Get-ChildItem $npmBinDir -File -ErrorAction SilentlyContinue |
    Select-Object -ExpandProperty BaseName

# Find bins that don't correspond to a known package
$orphans = $npmBins | Where-Object { $_ -notin $npmPkgs }
if ($orphans) {
    Write-Host "⚠️  Orphaned binaries in npm bin dir (no matching package):" -ForegroundColor Yellow
    $orphans | ForEach-Object { Write-Host "   $_" }
} else {
    Write-Host "✅ All npm binaries have matching packages"
}
```

---

## 6. Detect Shadow Installs

> A **shadow install** is a binary that's in your PATH, executable, but installed by a mechanism you didn't use consciously — or it overrides a tool you did install intentionally.

### 6.1 — The three types of shadow installs

```
TYPE 1: OVERRIDE SHADOW
  Another tool installed the same command name earlier in PATH.
  Example: scoop's `node` shim shadows the official Node.js install.

TYPE 2: ORPHAN SHADOW
  A tool was uninstalled but its binary was left behind in a PATH dir.
  Example: global-linked .cmd files from an npm uninstall that missed cleanup.

TYPE 3: STEALTH SHADOW
  A script (npm postinstall, corepack, a CI tool) added a binary
  to a PATH directory without any UI notification.
  Example: Corepack installing yarn/pnpm shims silently.
```

### 6.2 — Detect Type 1: Override shadows

```powershell
# For each well-known tool, find all copies in PATH
$wellKnownTools = @(
    "node", "npm", "npx", "pnpm", "yarn", "bun",
    "deno", "python", "python3", "go", "cargo",
    "rustup", "git", "code", "volta"
)

foreach ($tool in $wellKnownTools) {
    $all = Get-Command $tool -All -ErrorAction SilentlyContinue
    if ($all.Count -gt 1) {
        Write-Host "`n⚠️  OVERRIDE SHADOW: '$tool' found $($all.Count) times" -ForegroundColor Yellow
        $all | ForEach-Object {
            $active = if ($_ -eq $all[0]) { " ← WINS" } else { " (shadowed)" }
            Write-Host ("   {0}{1}" -f $_.Source, $active)
        }
    } elseif ($all.Count -eq 1) {
        Write-Host "✅ $tool → $($all[0].Source)"
    } else {
        Write-Host "   $tool not found" -ForegroundColor DarkGray
    }
}
```

### 6.3 — Detect Type 2: Orphan shadows (left-behind binaries)

```powershell
# Find .cmd and .bat files in npm global bin that point to missing targets
$npmBinDir = npm bin -g 2>$null

Get-ChildItem $npmBinDir -Filter *.cmd -ErrorAction SilentlyContinue | ForEach-Object {
    $content = Get-Content $_.FullName -Raw
    # Extract the target path from the .cmd shim
    if ($content -match '"%~dp0\\node_modules\\([^\\]+)') {
        $pkgName = $matches[1]
        $pkgPath = Join-Path (npm root -g) $pkgName
        if (-not (Test-Path $pkgPath)) {
            Write-Host "⚠️  ORPHAN SHIM: $($_.Name) → $pkgPath (missing!)" -ForegroundColor Yellow
        }
    }
}
```

### 6.4 — Detect Type 3: Stealth shadows (unexpected PATH additions)

```powershell
# Compare your PATH against a known-good baseline
# Step 1: Save baseline (run this once when everything is clean)
$env:PATH | Out-File "$env:USERPROFILE\path-baseline.txt"

# Step 2: Later, compare to detect additions
$baseline = Get-Content "$env:USERPROFILE\path-baseline.txt" -ErrorAction SilentlyContinue
if ($baseline) {
    $baselineEntries = $baseline -split ';'
    $currentEntries  = $env:PATH -split ';'
    $added   = $currentEntries | Where-Object { $_ -notin $baselineEntries -and $_ -ne '' }
    $removed = $baselineEntries | Where-Object { $_ -notin $currentEntries -and $_ -ne '' }
    
    if ($added) {
        Write-Host "`n🆕 PATH entries ADDED since baseline:" -ForegroundColor Green
        $added | ForEach-Object { Write-Host "   + $_" }
    }
    if ($removed) {
        Write-Host "`n🗑️  PATH entries REMOVED since baseline:" -ForegroundColor Red
        $removed | ForEach-Object { Write-Host "   - $_" }
    }
    if (-not $added -and -not $removed) {
        Write-Host "✅ PATH unchanged from baseline"
    }
} else {
    Write-Host "No baseline found. Run this to create one:"
    Write-Host '  $env:PATH | Out-File "$env:USERPROFILE\path-baseline.txt"'
}
```

### 6.5 — Detect stealth PATH entries via registry diff

```powershell
# The registry is the ground truth for persistent PATH
# Read directly — this won't be affected by current session overrides

$userPath   = [Environment]::GetEnvironmentVariable("PATH", "User")   -split ';'
$systemPath = [Environment]::GetEnvironmentVariable("PATH", "Machine") -split ';'
$sessionPath = $env:PATH -split ';'

# Session PATH = User + System + any in-session additions
$combined = ($userPath + $systemPath) | Where-Object { $_ -ne '' }
$sessionOnly = $sessionPath | Where-Object { $_ -notin $combined -and $_ -ne '' }

if ($sessionOnly) {
    Write-Host "⚠️  PATH entries in THIS SESSION only (not persisted):" -ForegroundColor Yellow
    $sessionOnly | ForEach-Object { Write-Host "   $_" }
} else {
    Write-Host "✅ No session-only PATH entries found"
}
```

### 6.6 — Corepack shadow detection

```powershell
# Corepack installs shims that intercept yarn/pnpm calls
# Detect if corepack is intercepting your tools:

$corepkgTools = @("yarn", "pnpm", "npm")
foreach ($t in $corepkgTools) {
    $src = (Get-Command $t -ErrorAction SilentlyContinue).Source
    if ($src) {
        $content = Get-Content $src -Raw -ErrorAction SilentlyContinue
        if ($content -match "corepack") {
            Write-Host "⚠️  COREPACK SHIM: '$t' at $src is a corepack shim" -ForegroundColor Yellow
        } else {
            Write-Host "✅ '$t' is NOT a corepack shim → $src"
        }
    }
}
```

### 6.7 — npm postinstall shadow detection

```powershell
# Some npm packages install extra global binaries via postinstall scripts
# These won't show up as their own npm list -g entry

# Get all binaries in npm bin dir
$npmBinDir = npm bin -g 2>$null
$actualBins = Get-ChildItem $npmBinDir -File -ErrorAction SilentlyContinue |
    Where-Object { $_.Extension -in '.cmd','.ps1','.exe','' } |
    Select-Object -ExpandProperty BaseName |
    Sort-Object -Unique

# Get all officially listed package names and their bins
$listedBins = npm list -g --depth=0 --json 2>$null |
    ConvertFrom-Json |
    ForEach-Object { $_.dependencies.PSObject.Properties.Name } |
    ForEach-Object {
        npm info $_ bin --json 2>$null | ConvertFrom-Json
    } |
    ForEach-Object { if ($_) { $_.PSObject.Properties.Name } }

$stealth = $actualBins | Where-Object { $_ -notin $listedBins }
if ($stealth) {
    Write-Host "⚠️  Binaries in npm bin dir NOT declared in any package's bin field:"
    $stealth | ForEach-Object { Write-Host "   $_" }
}
```

---

## 7. The Nuclear Dry Audit — Annotated & Improved

### 7.1 — Your original command, explained

```powershell
Get-ChildItem $env:USERPROFILE -Recurse -Depth 4 -ErrorAction SilentlyContinue |
    Where-Object Name -Match "cli|tool|node|pnpm|npm"
```

**What it does well:**
- Searches your entire user profile (Zone C)
- `-Depth 4` keeps it from taking forever
- Catches both files AND directories named with those keywords

**What it misses:**
- Zone A (`%APPDATA%`) and Zone B (`%LOCALAPPDATA%`) — those are *under* USERPROFILE but the depth-4 limit may not reach them when starting from USERPROFILE root
- Zone D (system-wide, `C:\Program Files`)
- Doesn't distinguish executables from config files
- No size or timestamp context

### 7.2 — The improved nuclear audit

```powershell
# ── NUCLEAR DRY AUDIT v2 ─────────────────────────────────────────────────
# Searches all audit zones, shows executables only, with age + size
# Safe read-only — changes nothing

$keywords = "cli|tool|node|pnpm|npm|bun|deno|yarn|volta|fnm|nvm|cargo|rustup|scoop"

$searchRoots = @(
    @{ Path = $env:USERPROFILE;    Depth = 4; Label = "USERPROFILE"    },
    @{ Path = $env:APPDATA;        Depth = 4; Label = "APPDATA"        },
    @{ Path = $env:LOCALAPPDATA;   Depth = 4; Label = "LOCALAPPDATA"   },
    @{ Path = "C:\Program Files";  Depth = 3; Label = "Program Files"  },
    @{ Path = "C:\ProgramData";    Depth = 3; Label = "ProgramData"    }
)

$results = foreach ($root in $searchRoots) {
    if (-not (Test-Path $root.Path)) { continue }
    Get-ChildItem $root.Path -Recurse -Depth $root.Depth -ErrorAction SilentlyContinue |
        Where-Object { $_.Name -match $keywords } |
        Select-Object @{N='Zone';     E={$root.Label}},
                      @{N='Type';     E={if($_.PSIsContainer){'DIR'}else{'FILE'}}},
                      @{N='Name';     E={$_.Name}},
                      @{N='Modified'; E={$_.LastWriteTime.ToString("yyyy-MM-dd")}},
                      @{N='Size_KB';  E={if(-not $_.PSIsContainer){[math]::Round($_.Length/1KB,1)}else{$null}}},
                      @{N='FullPath'; E={$_.FullName}}
}

$results | Sort-Object Zone, Type, Name | Format-Table -AutoSize
Write-Host "`nTotal matches: $($results.Count)"
```

### 7.3 — Nuclear audit — executables only (sharper signal)

```powershell
$keywords = "cli|tool|node|pnpm|npm|bun|deno|yarn|volta|fnm|nvm|cargo"
$exts     = @('.exe', '.cmd', '.bat', '.ps1', '.sh')

$searchRoots = @(
    $env:USERPROFILE,
    $env:APPDATA,
    $env:LOCALAPPDATA,
    "C:\Program Files",
    "C:\ProgramData"
)

$searchRoots |
    Where-Object { Test-Path $_ } |
    ForEach-Object {
        Get-ChildItem $_ -Recurse -Depth 4 -File -ErrorAction SilentlyContinue |
        Where-Object { $_.Name -match $keywords -and $_.Extension -in $exts }
    } |
    Select-Object @{N='Modified'; E={$_.LastWriteTime.ToString("yyyy-MM-dd")}},
                  @{N='Ext';      E={$_.Extension}},
                  Name,
                  @{N='Size_KB';  E={[math]::Round($_.Length/1KB,1)}},
                  FullName |
    Sort-Object Modified -Descending |
    Format-Table -AutoSize
```

### 7.4 — Nuclear audit — directories only (find install roots fast)

```powershell
# Find all directories whose names suggest dev tools
$keywords = "node|npm|pnpm|yarn|bun|deno|volta|fnm|nvm|cargo|scoop|chocolatey|corepack|rustup"

$env:USERPROFILE, $env:APPDATA, $env:LOCALAPPDATA |
    Where-Object { Test-Path $_ } |
    ForEach-Object {
        Get-ChildItem $_ -Recurse -Depth 3 -Directory -ErrorAction SilentlyContinue |
        Where-Object Name -Match $keywords |
        Select-Object @{N='Modified'; E={$_.LastWriteTime.ToString("yyyy-MM-dd")}},
                      Name,
                      FullName
    } |
    Sort-Object Modified -Descending |
    Format-Table -AutoSize
```

### 7.5 — Quiet version (just paths, no table — pipe-friendly)

```powershell
$keywords = "cli|tool|node|pnpm|npm|bun|deno|volta"

$env:USERPROFILE, $env:APPDATA, $env:LOCALAPPDATA |
    Where-Object { Test-Path $_ } |
    ForEach-Object {
        Get-ChildItem $_ -Recurse -Depth 4 -ErrorAction SilentlyContinue |
        Where-Object Name -Match $keywords
    } |
    Select-Object -ExpandProperty FullName |
    Sort-Object -Unique
```

---

## 8. Registry Audit — What Installers Register

> The registry is the most complete record of what was intentionally installed. Learn to read it.

### 8.1 — Full uninstall registry sweep (all scopes)

```powershell
$regPaths = @(
    # 64-bit system installs
    "HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall",
    # 32-bit on 64-bit Windows
    "HKLM:\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall",
    # Per-user installs (NO admin needed to write here)
    "HKCU:\Software\Microsoft\Windows\CurrentVersion\Uninstall"
)

$allInstalls = $regPaths | ForEach-Object {
    Get-ItemProperty "$_\*" -ErrorAction SilentlyContinue |
    Where-Object DisplayName -ne $null |
    Select-Object DisplayName, DisplayVersion, Publisher, InstallDate,
                  InstallLocation,
                  @{N='Scope'; E={ if ($_ -match 'HKCU') {'User'} else {'System'} }}
}

$allInstalls | Sort-Object DisplayName | Format-Table -AutoSize
```

### 8.2 — Search registry for dev-tool-related entries

```powershell
$pattern = "node|npm|pnpm|yarn|bun|deno|volta|nvm|scoop|chocolatey|rust|cargo|python"

@(
    "HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall",
    "HKCU:\Software\Microsoft\Windows\CurrentVersion\Uninstall"
) | ForEach-Object {
    Get-ItemProperty "$_\*" -ErrorAction SilentlyContinue |
    Where-Object { $_.DisplayName -match $pattern }
} | Select-Object DisplayName, DisplayVersion, InstallLocation, InstallDate |
    Format-Table -AutoSize
```

### 8.3 — Find PATH entries written to registry (persistent)

```powershell
# These are what SURVIVE a reboot
Write-Host "── USER PATH (persists):" -ForegroundColor Cyan
[Environment]::GetEnvironmentVariable("PATH", "User") -split ';' |
    Where-Object { $_ -ne '' } |
    ForEach-Object { Write-Host "  $_" }

Write-Host "`n── SYSTEM PATH (persists, all users):" -ForegroundColor Cyan
[Environment]::GetEnvironmentVariable("PATH", "Machine") -split ';' |
    Where-Object { $_ -ne '' } |
    ForEach-Object { Write-Host "  $_" }
```

### 8.4 — Find what modified PATH in the registry recently

```powershell
# Check the last-write time of the registry key that holds User PATH
$key = [Microsoft.Win32.Registry]::CurrentUser.OpenSubKey(
    "Environment", $false
)
Write-Host "User PATH key last modified: $($key.GetValue('Path'))"
# Note: registry key timestamps require additional tooling (e.g., NtQueryKey)
# This shows you the current value — forensic timestamps need Sysinternals
```

---

## 9. Correlate Binary → Installer → Drop Zone

> You found a binary. Now: what installed it? Where did it come from?

### 9.1 — Full correlation function

```powershell
function Get-BinaryOrigin {
    param([string]$BinaryName)

    $cmd = Get-Command $BinaryName -ErrorAction SilentlyContinue
    if (-not $cmd) {
        Write-Host "❌ '$BinaryName' not found in PATH"
        return
    }

    $path = $cmd.Source
    Write-Host "`n🔍 Binary: $BinaryName"
    Write-Host "   Path   : $path"
    Write-Host "   Age    : $((New-TimeSpan (Get-Item $path).LastWriteTime).Days) days old"
    Write-Host "   Size   : $([math]::Round((Get-Item $path).Length/1KB, 1)) KB"

    # Determine zone
    $zone = switch -Wildcard ($path) {
        "*\AppData\Roaming\npm*"        { "Zone A — npm global" }
        "*\AppData\Local\pnpm*"         { "Zone B — pnpm" }
        "*\AppData\Local\Volta*"        { "Zone B — Volta" }
        "*\AppData\Local\fnm*"          { "Zone B — fnm" }
        "*\.bun\*"                      { "Zone C — bun" }
        "*\.deno\*"                     { "Zone C — deno" }
        "*\.cargo\*"                    { "Zone C — cargo (Rust)" }
        "*\scoop\*"                     { "Zone C — scoop" }
        "*\Program Files\nodejs*"       { "Zone D — Node.js (official)" }
        "*\ProgramData\chocolatey*"     { "Zone D — Chocolatey" }
        "*\WindowsApps*"                { "Zone E — winget / Store" }
        default                         { "Unknown zone" }
    }
    Write-Host "   Zone   : $zone"

    # Check if it's a shim
    $content = Get-Content $path -Raw -ErrorAction SilentlyContinue
    if ($content -match "node_modules") { Write-Host "   Type   : npm shim (.cmd wrapper)" }
    elseif ($content -match "corepack") { Write-Host "   Type   : corepack shim" }
    elseif ($content -match "scoop")    { Write-Host "   Type   : scoop shim" }
    elseif ($path -match "\.exe$")      { Write-Host "   Type   : native binary (.exe)" }

    # Cross-reference with package managers
    Write-Host "`n   Cross-reference:"
    $npmMatch = npm list -g --depth=0 2>$null | Select-String $BinaryName
    if ($npmMatch) { Write-Host "   ✅ Found in npm globals" } else { Write-Host "   — Not in npm globals" }

    if (Get-Command pnpm -ErrorAction SilentlyContinue) {
        $pnpmMatch = pnpm list -g --depth=0 2>$null | Select-String $BinaryName
        if ($pnpmMatch) { Write-Host "   ✅ Found in pnpm globals" } else { Write-Host "   — Not in pnpm globals" }
    }
}

# Usage:
Get-BinaryOrigin "node"
Get-BinaryOrigin "yarn"
Get-BinaryOrigin "prettier"
```

---

## 10. Forensic Timeline: What Was Installed When

### 10.1 — System-wide install timeline from registry

```powershell
@(
    "HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall",
    "HKCU:\Software\Microsoft\Windows\CurrentVersion\Uninstall"
) | ForEach-Object {
    Get-ItemProperty "$_\*" -ErrorAction SilentlyContinue |
    Where-Object { $_.DisplayName -and $_.InstallDate } |
    Select-Object DisplayName, DisplayVersion, InstallDate,
                  @{N='Scope'; E={ if ($_ -match 'HKCU') {'User'} else {'System'} }}
} |
Sort-Object InstallDate -Descending |
Format-Table -AutoSize
```

### 10.2 — Binary-age timeline across all zones

```powershell
$allZones = @(
    "$env:APPDATA\npm",
    "$env:LOCALAPPDATA\pnpm",
    "$env:LOCALAPPDATA\Volta\bin",
    "$env:USERPROFILE\.bun\bin",
    "$env:USERPROFILE\.deno\bin",
    "$env:USERPROFILE\.cargo\bin",
    "$env:USERPROFILE\scoop\shims",
    "C:\ProgramData\chocolatey\bin"
)

$allZones | Where-Object { Test-Path $_ } | ForEach-Object {
    $zone = $_
    Get-ChildItem $zone -File -ErrorAction SilentlyContinue |
    Where-Object { $_.Extension -in '.exe','.cmd','.bat' } |
    Select-Object @{N='Name';     E={$_.BaseName}},
                  @{N='Zone';     E={$zone -replace [regex]::Escape($env:USERPROFILE),'~' `
                                           -replace [regex]::Escape($env:APPDATA),'%APPDATA%' `
                                           -replace [regex]::Escape($env:LOCALAPPDATA),'%LOCALAPPDATA%'}},
                  @{N='Installed'; E={$_.LastWriteTime.ToString("yyyy-MM-dd")}}
} | Sort-Object Installed -Descending | Format-Table -AutoSize
```

### 10.3 — PowerShell command history for install-related commands

```powershell
$histFile = (Get-PSReadLineOption).HistorySavePath
if (Test-Path $histFile) {
    $installKeywords = "install|iex|irm|iwr|curl|wget|choco|scoop|winget|npm.*-g|pnpm.*-g|add.*global|setup|init"
    
    Get-Content $histFile |
        Where-Object { $_ -match $installKeywords } |
        ForEach-Object { Write-Host "  $_" }
} else {
    Write-Host "No PSReadLine history file found."
}
```

---

## 11. Power User One-Liner Collection

> Copy-paste ready. Each does exactly one useful thing.

```powershell
# ── DISCOVERY ────────────────────────────────────────────────────────────────

# Every executable currently in PATH (sorted)
$env:PATH -split ';' | ? { $_ -and (Test-Path $_) } | % { gci $_ -File -Include *.exe,*.cmd -EA 0 } | select -Exp BaseName | sort -Unique

# All dev tools, check if present + version
"node","npm","npx","pnpm","yarn","bun","deno","volta","fnm","cargo","go","python","git" | % { $v = & $_ --version 2>$null; "{0,-15} {1}" -f $_, $v }

# Full PATH, one per line, with existence check
$env:PATH -split ';' | ? { $_ } | % { "{0} {1}" -f (?: (Test-Path $_) "✅" "⚠️"), $_ }

# Biggest npm global packages
npm list -g --depth=0 --json | ConvertFrom-Json | % { $_.dependencies.PSObject.Properties } | sort { (gci (join-path (npm root -g) $_.Name) -Recurse -EA 0 | measure Length -Sum).Sum } -Desc | select -f 10 | % { $_.Name }

# ── SHADOW DETECTION ─────────────────────────────────────────────────────────

# Find all tools in PATH that appear more than once (shadows)
$env:PATH -split ';' | ? { $_ -and (Test-Path $_) } | % { gci $_ -File -Include *.exe,*.cmd -EA 0 } | Group Name | ? Count -gt 1 | % { "⚠️ SHADOW: $($_.Name)"; $_.Group | % { "   $($_.FullName)" } }

# Which 'node' wins?
Get-Command node -All 2>$null | select Source | % { "  $($_.Source)" }

# ── AGE-BASED HUNTING ────────────────────────────────────────────────────────

# Everything installed in last 7 days (across all PATH dirs)
$env:PATH -split ';' | ? { $_ -and (Test-Path $_) } | % { gci $_ -File -Include *.exe,*.cmd -EA 0 | ? LastWriteTime -gt (Get-Date).AddDays(-7) } | select Name, LastWriteTime, FullName | sort LastWriteTime -Desc | ft -Auto

# ── ZONE INVENTORY ───────────────────────────────────────────────────────────

# Count files in each audit zone
"$env:APPDATA\npm","$env:LOCALAPPDATA\pnpm","$env:USERPROFILE\.bun","$env:USERPROFILE\.deno","$env:USERPROFILE\.cargo","$env:USERPROFILE\scoop\shims" | % { $c = (gci $_ -File -EA 0).Count; "{0,4} files  {1}" -f $c, $_ }

# ── CLEANUP HELPERS ──────────────────────────────────────────────────────────

# Which npm globals haven't been updated in >180 days?
npm list -g --depth=0 --json 2>$null | ConvertFrom-Json | % { $_.dependencies.PSObject.Properties } | % { $pkg = join-path (npm root -g) $_.Name; $age = (New-TimeSpan (gci $pkg -EA 0).LastWriteTime).Days; if ($age -gt 180) { "{0,-30} {1} days old" -f $_.Name, $age } }

# How much is the npm cache using?
"{0:N1} MB" -f ((gci (npm config get cache) -Recurse -EA 0 | measure Length -Sum).Sum / 1MB)

# ── ENVIRONMENT ──────────────────────────────────────────────────────────────

# Only dev-related env vars
gci Env: | ? Name -Match "NODE|NPM|PNPM|YARN|BUN|DENO|NVM|VOLTA|CARGO|GO|PATH|HOME|PREFIX" | ft Name, Value -Auto

# PATH entries that don't exist on disk (cleanup candidates)
$env:PATH -split ';' | ? { $_ -and !(Test-Path $_) } | % { "⚠️  GHOST: $_" }
```

---

## 12. The Full Power Audit Script

> Save as `power-audit.ps1`. Run it. Save the output.

```powershell
# ================================================================
# power-audit.ps1 — Windows Developer Tools Forensic Audit
# Covers: PATH, binaries by age, shadows, zones, registry, history
# Usage: .\power-audit.ps1 | Tee-Object audit-$(Get-Date -f yyyyMMdd).txt
# ================================================================

$w = 70
function H1($t) { Write-Host "`n$('═'*$w)`n  $t`n$('═'*$w)" -ForegroundColor Cyan }
function H2($t) { Write-Host "`n  ── $t" -ForegroundColor Yellow }
function OK($t) { Write-Host "  ✅ $t" -ForegroundColor Green }
function WN($t) { Write-Host "  ⚠️  $t" -ForegroundColor Yellow }
function NA($t) { Write-Host "  ·  $t" -ForegroundColor DarkGray }

# ────────────────────────────────────────────────────────────────
H1 "1. RUNTIME VERSIONS"
$tools = @{
    node    = "node --version"
    npm     = "npm --version"
    pnpm    = "pnpm --version"
    yarn    = "yarn --version"
    bun     = "bun --version"
    deno    = "deno --version"
    cargo   = "cargo --version"
    go      = "go version"
    python  = "python --version"
    git     = "git --version"
    volta   = "volta --version"
    fnm     = "fnm --version"
}
foreach ($name in ($tools.Keys | Sort-Object)) {
    try {
        $v = Invoke-Expression $tools[$name] 2>$null
        OK ("{0,-12} {1}" -f $name, ($v -join ' '))
    } catch {
        NA ("{0,-12} not found" -f $name)
    }
}

# ────────────────────────────────────────────────────────────────
H1 "2. PACKAGE MANAGER GLOBALS"

H2 "npm globals"
npm list -g --depth=0 2>$null | Select-String "├──|└──" | % { "  $_" }

H2 "pnpm globals"
if (Get-Command pnpm -EA 0) { pnpm list -g --depth=0 2>$null | Select-String "├──|└──" | % { "  $_" } }
else { NA "pnpm not found" }

H2 "corepack managed"
if (Get-Command corepack -EA 0) { corepack ls 2>$null | % { "  $_" } }
else { NA "corepack not found" }

# ────────────────────────────────────────────────────────────────
H1 "3. AUDIT ZONE INVENTORY"

$zones = [ordered]@{
    "npm globals"         = "$env:APPDATA\npm"
    "npm cache"           = "$env:LOCALAPPDATA\npm-cache"
    "pnpm home"           = "$env:LOCALAPPDATA\pnpm"
    "Volta"               = "$env:LOCALAPPDATA\Volta"
    "fnm"                 = "$env:LOCALAPPDATA\fnm"
    "bun"                 = "$env:USERPROFILE\.bun"
    "deno"                = "$env:USERPROFILE\.deno"
    "cargo"               = "$env:USERPROFILE\.cargo"
    "scoop"               = "$env:USERPROFILE\scoop"
    "chocolatey"          = "C:\ProgramData\chocolatey"
    "Node.js (system)"    = "C:\Program Files\nodejs"
}

foreach ($name in $zones.Keys) {
    $p = $zones[$name]
    if (Test-Path $p) {
        $mb = (gci $p -Recurse -EA 0 | measure Length -Sum).Sum / 1MB
        OK ("{0,-22} {1,8:N1} MB  {2}" -f $name, $mb, $p)
    } else {
        NA ("{0,-22}  not found" -f $name)
    }
}

# ────────────────────────────────────────────────────────────────
H1 "4. PATH ANALYSIS"

$pathEntries = $env:PATH -split ';' | ? { $_ -ne '' }
H2 "Total entries: $($pathEntries.Count)"

$dupes = $pathEntries | Group-Object | ? Count -gt 1
if ($dupes) { $dupes | % { WN "DUPLICATE: $($_.Name) (×$($_.Count))" } }

$missing = $pathEntries | ? { !(Test-Path $_) }
if ($missing) { $missing | % { WN "GHOST PATH: $_" } }

$pathEntries | ? { $_ -ne '' } | % {
    $icon = if (Test-Path $_) { "✅" } else { "⚠️ " }
    Write-Host "  $icon $_"
}

# ────────────────────────────────────────────────────────────────
H1 "5. SHADOW DETECTION"

$checkTools = @("node","npm","npx","pnpm","yarn","bun","deno","python","git","volta","cargo")
foreach ($t in $checkTools) {
    $all = Get-Command $t -All -EA 0
    if ($all.Count -gt 1) {
        WN "SHADOW '$t' → $($all.Count) copies:"
        $all | % { Write-Host ("      {0}" -f $_.Source) }
    } elseif ($all.Count -eq 1) {
        OK ("{0,-15} {1}" -f $t, $all[0].Source)
    } else {
        NA ("{0,-15} not in PATH" -f $t)
    }
}

# ────────────────────────────────────────────────────────────────
H1 "6. RECENT BINARIES (last 30 days)"

$recent = $env:PATH -split ';' |
    ? { $_ -ne '' -and (Test-Path $_) } |
    % { gci $_ -File -Include *.exe,*.cmd,*.bat,*.ps1 -EA 0 |
        ? LastWriteTime -gt (Get-Date).AddDays(-30) } |
    Select-Object Name,
                  @{N='Date'; E={$_.LastWriteTime.ToString("yyyy-MM-dd")}},
                  FullName |
    Sort-Object Date -Descending

if ($recent) {
    $recent | Format-Table -AutoSize
} else {
    NA "No binaries modified in last 30 days"
}

# ────────────────────────────────────────────────────────────────
H1 "7. NUCLEAR ZONE SCAN (keyword match)"

$keywords = "cli|tool|node|pnpm|npm|bun|deno|yarn|volta|fnm|nvm|cargo|rustup"
$scanRoots = @($env:APPDATA, $env:LOCALAPPDATA, "$env:USERPROFILE\.bun",
               "$env:USERPROFILE\.deno", "$env:USERPROFILE\.cargo")

$scanRoots | ? { Test-Path $_ } | % {
    gci $_ -Recurse -Depth 3 -File -EA 0 |
    ? { $_.Name -match $keywords -and $_.Extension -in '.exe','.cmd','.bat','.ps1' }
} | Select-Object @{N='Modified';E={$_.LastWriteTime.ToString("yyyy-MM-dd")}},
                  Name, FullName |
    Sort-Object Modified -Descending |
    Format-Table -AutoSize

# ────────────────────────────────────────────────────────────────
H1 "8. INSTALL HISTORY (PowerShell)"

$histFile = (Get-PSReadLineOption 2>$null).HistorySavePath
if ($histFile -and (Test-Path $histFile)) {
    $pattern = "install|iex|irm|iwr|curl|wget|choco|scoop|winget|npm.*-g|pnpm.*-g|global"
    $hits = Get-Content $histFile | ? { $_ -match $pattern } | Select-Object -Last 30
    $hits | % { Write-Host "  $_" -ForegroundColor DarkYellow }
} else {
    NA "No PowerShell history file found"
}

# ────────────────────────────────────────────────────────────────
H1 "9. ENVIRONMENT VARIABLES (dev-related)"

gci Env: | ? Name -Match "NODE|NPM|PNPM|YARN|BUN|DENO|NVM|VOLTA|CARGO|GO|RUST" |
    Sort-Object Name |
    % { "  {0,-30} = {1}" -f $_.Name, $_.Value }

# ────────────────────────────────────────────────────────────────
H1 "AUDIT COMPLETE"
Write-Host "  Timestamp: $(Get-Date)" -ForegroundColor Green
Write-Host "  Save with: .\power-audit.ps1 | Tee-Object audit-$(Get-Date -f yyyyMMdd).txt"
```

---

## 📌 Power User Quick Reference

```
FIND UNKNOWNS BY AGE
══════════════════════════════════════════════════════════════════
# Last 30 days
$env:PATH -split ';' | ? { $_ -and (Test-Path $_) } |
  % { gci $_ -File -Include *.exe,*.cmd -EA 0 |
      ? LastWriteTime -gt (Get-Date).AddDays(-30) } |
  sort LastWriteTime -Desc | ft Name, LastWriteTime, FullName

FIND ALL CLI TOOLS IN PATH
══════════════════════════════════════════════════════════════════
$env:PATH -split ';' | ? { $_ -and (Test-Path $_) } |
  % { gci $_ -File -Include *.exe,*.cmd -EA 0 } |
  select -Exp BaseName | sort -Unique

FIND SHADOW INSTALLS
══════════════════════════════════════════════════════════════════
Get-Command node,npm,pnpm,yarn,bun -All 2>$null |
  Group-Object Name | ? Count -gt 1 |
  % { "SHADOW: $($_.Name)"; $_.Group | % { "  $($_.Source)" } }

NUCLEAR DRY AUDIT (improved)
══════════════════════════════════════════════════════════════════
$env:APPDATA,$env:LOCALAPPDATA,$env:USERPROFILE |
  % { gci $_ -Recurse -Depth 4 -EA 0 } |
  ? { $_.Name -match "cli|tool|node|pnpm|npm|bun|deno" -and
      $_.Extension -in '.exe','.cmd','.bat' } |
  sort LastWriteTime -Desc | ft Name, LastWriteTime, FullName

iwr|iex DROP ZONES (where to look after a one-liner)
══════════════════════════════════════════════════════════════════
$env:USERPROFILE\.bun         bun
$env:USERPROFILE\.deno        deno
$env:USERPROFILE\.cargo       cargo / rustup
$env:LOCALAPPDATA\Volta       volta
$env:LOCALAPPDATA\fnm         fnm
$env:USERPROFILE\scoop        scoop (everything)
$env:APPDATA\npm              npm globals (NOT iwr|iex)
C:\ProgramData\chocolatey     choco (only if run as admin)

CORRELATE BINARY TO SOURCE
══════════════════════════════════════════════════════════════════
(Get-Command <name>).Source            find it
Get-Command <name> -All                find ALL copies
Get-Content (Get-Command <name>).Source  read the shim
```

---

*Companion to `WINDOWS_INSTALL_AUDIT_README.md` · PowerShell 5.1+ · Windows 10/11*
