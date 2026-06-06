# 🔍 Windows Installation Audit Guide
### Discover Everything Installed via npm, pnpm, npx, corepack, curl | iex, and One-Liners

> **Who is this for?** Windows beginners and intermediate users who want a clear, complete picture of what's installed on their machine — and how to clean it up or audit it confidently.

---

## 📋 Table of Contents

1. [Quick-Start Cheat Sheet](#1-quick-start-cheat-sheet)
2. [Understanding How Tools Get Installed on Windows](#2-understanding-how-tools-get-installed-on-windows)
3. [Auditing npm Global Installs](#3-auditing-npm-global-installs)
4. [Auditing pnpm Global Installs](#4-auditing-pnpm-global-installs)
5. [Auditing npx (Ephemeral & Cached Packages)](#5-auditing-npx-ephemeral--cached-packages)
6. [Auditing corepack-Managed Package Managers](#6-auditing-corepack-managed-package-managers)
7. [Auditing curl | iex and PowerShell One-Liner Installs](#7-auditing-curl--iex-and-powershell-one-liner-installs)
8. [Full PATH Audit](#8-full-path-audit)
9. [Filesystem Deep-Dive Audit](#9-filesystem-deep-dive-audit)
10. [Environment Variables Audit](#10-environment-variables-audit)
11. [Startup Programs & Services Audit](#11-startup-programs--services-audit)
12. [All-in-One Audit Script](#12-all-in-one-audit-script)
13. [User-Specific Tweaks & Customizations](#13-user-specific-tweaks--customizations)
14. [Uninstalling / Cleaning Up](#14-uninstalling--cleaning-up)
15. [FAQ](#15-faq)
16. [Troubleshooting](#16-troubleshooting)
17. [Glossary](#17-glossary)

---

## 1. Quick-Start Cheat Sheet

> **Open PowerShell** by pressing `Win + X` → select **Windows PowerShell** or **Terminal**.  
> All commands below are PowerShell unless stated otherwise.

```powershell
# ── Node / npm ──────────────────────────────────────────────────────────────
node -v                          # Node.js version
npm -v                           # npm version
npm list -g --depth=0            # All globally installed npm packages

# ── pnpm ────────────────────────────────────────────────────────────────────
pnpm -v                          # pnpm version (if installed)
pnpm list -g --depth=0          # All globally installed pnpm packages

# ── corepack ────────────────────────────────────────────────────────────────
corepack -v                      # corepack version
corepack ls                      # Managed package managers

# ── npx cache ───────────────────────────────────────────────────────────────
# npx doesn't keep a persistent install list — see Section 5

# ── PATH ────────────────────────────────────────────────────────────────────
$env:PATH -split ';'            # All directories in your PATH

# ── What's actually executable on your system? ──────────────────────────────
Get-Command npm, node, pnpm, yarn, bun, deno 2>$null | Select-Object Name, Source
```

---

## 2. Understanding How Tools Get Installed on Windows

Before auditing, it helps to know *where* things land:

| Installer Method | What It Does | Where It Usually Lands |
|---|---|---|
| `npm install -g <pkg>` | Installs globally via npm | `%APPDATA%\npm\` |
| `pnpm add -g <pkg>` | Installs globally via pnpm | `%LOCALAPPDATA%\pnpm\` |
| `npx <pkg>` | Downloads & runs (may cache) | `%LOCALAPPDATA%\npm-cache\_npx\` |
| `corepack enable` | Shims yarn/pnpm into PATH | Inside Node.js install dir |
| `curl … | iex` or `irm … | iex` | Runs a remote PowerShell script | Varies — usually `%LOCALAPPDATA%\Programs\`, `%APPDATA%\`, or custom |
| Winget / Chocolatey / Scoop | Windows package managers | `C:\ProgramData\`, `%USERPROFILE%\scoop\`, etc. |
| Direct `.exe` / `.msi` installer | Classic Windows installer | `C:\Program Files\` or `C:\Program Files (x86)\` |

> 💡 **Key insight:** Many CLI tools skip "Add/Remove Programs" entirely. That's why PATH + filesystem auditing is essential.

---

## 3. Auditing npm Global Installs

### 3.1 — List every globally installed npm package

```powershell
npm list -g --depth=0
```

**Sample output:**
```
C:\Users\YourName\AppData\Roaming\npm
├── @angular/cli@17.3.0
├── create-react-app@5.0.1
├── typescript@5.4.2
└── vercel@33.5.0
```

### 3.2 — Find where npm puts global packages

```powershell
npm root -g          # Physical folder of installed packages
npm bin -g           # Folder of executable binaries (should be in PATH)
npm config get prefix  # The prefix directory (parent of both above)
```

### 3.3 — Inspect a specific package

```powershell
npm list -g <package-name>          # Check if installed & version
npm info <package-name> version     # Latest version on registry
```

### 3.4 — Show all npm config values

```powershell
npm config list        # User + project config
npm config list -l     # Everything including defaults
```

### 3.5 — Find your .npmrc files

```powershell
# User-level config
Get-Content "$env:USERPROFILE\.npmrc" -ErrorAction SilentlyContinue

# Global config
npm config get globalconfig
Get-Content (npm config get globalconfig) -ErrorAction SilentlyContinue
```

### 3.6 — Physically browse global npm packages

```powershell
# Open in File Explorer
explorer (npm root -g)

# Or list in PowerShell
Get-ChildItem (npm root -g) | Select-Object Name, LastWriteTime
```

---

## 4. Auditing pnpm Global Installs

> **Note:** pnpm stores globals differently from npm. It uses a separate virtual store.

### 4.1 — Check if pnpm is installed

```powershell
Get-Command pnpm -ErrorAction SilentlyContinue
pnpm -v
```

### 4.2 — List all global pnpm packages

```powershell
pnpm list -g --depth=0
```

### 4.3 — Find pnpm's global directories

```powershell
pnpm root -g           # Global node_modules
pnpm bin -g            # Global bin directory
pnpm store path        # Content-addressable store location
```

### 4.4 — pnpm config and settings

```powershell
pnpm config list                   # All pnpm config
pnpm config get global-dir         # Where globals live
pnpm config get global-bin-dir     # Where global binaries are placed

# Find .npmrc used by pnpm (pnpm reads the same .npmrc)
Get-Content "$env:USERPROFILE\.npmrc" -ErrorAction SilentlyContinue
```

### 4.5 — Browse pnpm's store (deduplicated storage)

```powershell
# pnpm uses a content-addressable store — packages aren't duplicated
explorer (pnpm store path)
```

### 4.6 — pnpm vs npm globals side by side

```powershell
Write-Host "── npm globals ──"
npm list -g --depth=0 2>$null

Write-Host "`n── pnpm globals ──"
pnpm list -g --depth=0 2>$null
```

---

## 5. Auditing npx (Ephemeral & Cached Packages)

> **Important:** `npx` is designed for *run-once* use and doesn't maintain a package list. But it *does* leave a cache.

### 5.1 — How npx works (beginner explanation)

When you run `npx create-react-app my-app`:
1. npx checks if `create-react-app` is already installed globally → uses it if found
2. If not, it downloads it into a **temporary cache folder**
3. Runs it, and **may or may not delete** the temporary files

### 5.2 — Find the npx cache

```powershell
# npm's cache directory (npx stores its downloads here)
npm config get cache

# The _npx subfolder specifically
$npxCache = Join-Path (npm config get cache) "_npx"
if (Test-Path $npxCache) {
    Get-ChildItem $npxCache -Directory | Select-Object Name, LastWriteTime
} else {
    Write-Host "No npx cache found."
}
```

### 5.3 — Browse the npx cache visually

```powershell
explorer (Join-Path (npm config get cache) "_npx")
```

### 5.4 — Check if a package was run via npx recently

```powershell
# npx cache entries are hashed folders — look at modification dates
$npxCache = Join-Path (npm config get cache) "_npx"
Get-ChildItem $npxCache -Recurse -Filter "package.json" |
    Select-Object FullName, LastWriteTime |
    Sort-Object LastWriteTime -Descending |
    Select-Object -First 20
```

### 5.5 — Clear the npx cache

```powershell
npm cache clean --force    # Clears ALL npm/npx cache
```

> ⚠️ This also clears npm's regular download cache, which may slow down your next `npm install`.

---

## 6. Auditing corepack-Managed Package Managers

> **What is corepack?** It's a Node.js built-in tool that manages *which version* of `yarn`, `pnpm`, and `npm` a project uses. It ships with Node.js 16.9+.

### 6.1 — Check if corepack is enabled

```powershell
corepack -v             # Corepack version
Get-Command yarn -ErrorAction SilentlyContinue     # Is yarn available?
Get-Command pnpm -ErrorAction SilentlyContinue     # Is pnpm via corepack?
```

### 6.2 — List corepack-managed package managers

```powershell
corepack ls
```

### 6.3 — Find where corepack stores its downloads

```powershell
# Corepack cache is usually here:
$corepkgCache = "$env:LOCALAPPDATA\node\corepack"
if (Test-Path $corepkgCache) {
    Get-ChildItem $corepkgCache -Recurse -Depth 2
} else {
    # Alternate location (older Node setups)
    "$env:USERPROFILE\.node\corepack"
}
```

### 6.4 — Check if yarn is via corepack or standalone

```powershell
# Get the actual executable path
(Get-Command yarn -ErrorAction SilentlyContinue).Source

# If it's inside the Node.js directory, it's corepack-managed
# If it's in a separate folder, it was installed independently
```

### 6.5 — Disable corepack (revert to direct installs)

```powershell
corepack disable        # Remove corepack shims from PATH
corepack disable yarn   # Only remove yarn shim
corepack disable pnpm   # Only remove pnpm shim
```

### 6.6 — Re-enable corepack

```powershell
corepack enable
```

---

## 7. Auditing curl | iex and PowerShell One-Liner Installs

> These are the **trickiest** to audit because they can install anything, anywhere, with no central registry.

### 7.1 — Common tools installed this way on Windows

| Tool | Install Command Pattern | Default Location |
|---|---|---|
| Volta (Node version manager) | `winget install Volta.Volta` or `iwr … \| iex` | `%LOCALAPPDATA%\Volta\` |
| NVM for Windows | `.exe` installer | `%APPDATA%\nvm\` |
| fnm | `winget install Schniz.fnm` or `choco` | `%LOCALAPPDATA%\fnm\` |
| Bun | `powershell -c "irm bun.sh/install.ps1 \| iex"` | `%USERPROFILE%\.bun\bin\` |
| Deno | `irm https://deno.land/install.ps1 \| iex` | `%USERPROFILE%\.deno\bin\` |
| Scoop | `irm get.scoop.sh \| iex` | `%USERPROFILE%\scoop\` |
| Chocolatey | `Set-ExecutionPolicy … ; iex (…)` | `C:\ProgramData\chocolatey\` |
| Rust / Cargo | `winget install Rustlang.Rustup` | `%USERPROFILE%\.cargo\bin\` |
| Go | `.msi` installer | `C:\Program Files\Go\bin\` |

### 7.2 — Check for common one-liner-installed tools

```powershell
$tools = @("bun", "deno", "volta", "fnm", "nvm", "scoop", "choco", "cargo", "rustup", "go", "zig")
foreach ($tool in $tools) {
    $cmd = Get-Command $tool -ErrorAction SilentlyContinue
    if ($cmd) {
        Write-Host "✅ $tool → $($cmd.Source)"
    } else {
        Write-Host "❌ $tool not found in PATH"
    }
}
```

### 7.3 — Check common install directories

```powershell
$suspectDirs = @(
    "$env:USERPROFILE\.bun",
    "$env:USERPROFILE\.deno",
    "$env:USERPROFILE\.cargo",
    "$env:LOCALAPPDATA\Volta",
    "$env:LOCALAPPDATA\fnm",
    "$env:APPDATA\nvm",
    "$env:USERPROFILE\scoop",
    "C:\ProgramData\chocolatey",
    "$env:USERPROFILE\.volta"
)

foreach ($dir in $suspectDirs) {
    if (Test-Path $dir) {
        $size = (Get-ChildItem $dir -Recurse -ErrorAction SilentlyContinue |
            Measure-Object -Property Length -Sum).Sum / 1MB
        Write-Host ("✅ {0,-45} ({1:N1} MB)" -f $dir, $size)
    } else {
        Write-Host "❌ $dir"
    }
}
```

### 7.4 — Check Windows Add/Remove Programs (covers .msi/.exe installs)

```powershell
# 64-bit programs
Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\* |
    Select-Object DisplayName, DisplayVersion, Publisher, InstallDate |
    Where-Object DisplayName -ne $null |
    Sort-Object DisplayName |
    Format-Table -AutoSize

# 32-bit programs (on 64-bit Windows)
Get-ItemProperty HKLM:\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\* |
    Select-Object DisplayName, DisplayVersion, Publisher |
    Where-Object DisplayName -ne $null |
    Sort-Object DisplayName |
    Format-Table -AutoSize

# Per-user installs
Get-ItemProperty HKCU:\Software\Microsoft\Windows\CurrentVersion\Uninstall\* |
    Select-Object DisplayName, DisplayVersion, Publisher |
    Where-Object DisplayName -ne $null |
    Sort-Object DisplayName |
    Format-Table -AutoSize
```

### 7.5 — Check PowerShell execution history for past installs

```powershell
# PSReadLine history (recent PowerShell commands)
$histFile = (Get-PSReadLineOption).HistorySavePath
if (Test-Path $histFile) {
    Get-Content $histFile |
        Where-Object { $_ -match "iex|irm|curl|install|choco|scoop|winget" } |
        Select-Object -Last 50
}
```

### 7.6 — Check winget (Windows Package Manager)

```powershell
winget list              # Everything installed via winget
winget list --source winget  # Filter to winget source only
```

### 7.7 — Check Scoop installs

```powershell
scoop list               # All scoop-installed packages (if scoop is installed)
```

### 7.8 — Check Chocolatey installs

```powershell
choco list --local-only  # All locally installed choco packages
```

---

## 8. Full PATH Audit

> Your `PATH` variable is the master list of directories Windows searches for executables. Every installer that works from the command line *must* be in PATH.

### 8.1 — Display your full PATH (one entry per line)

```powershell
$env:PATH -split ';' | Where-Object { $_ -ne '' } | Sort-Object
```

### 8.2 — Find duplicate PATH entries

```powershell
$env:PATH -split ';' |
    Where-Object { $_ -ne '' } |
    Group-Object |
    Where-Object Count -gt 1 |
    Select-Object Name, Count
```

### 8.3 — Find PATH entries that don't exist on disk

```powershell
$env:PATH -split ';' | Where-Object { $_ -ne '' } | ForEach-Object {
    if (-not (Test-Path $_)) {
        Write-Host "⚠️  Missing: $_"
    }
}
```

### 8.4 — Find all executables in PATH (the master tool list)

```powershell
# List every .exe, .cmd, .bat, .ps1 in your PATH
$env:PATH -split ';' | Where-Object { $_ -ne '' } | ForEach-Object {
    if (Test-Path $_) {
        Get-ChildItem $_ -File -Include *.exe, *.cmd, *.bat, *.ps1 -ErrorAction SilentlyContinue
    }
} | Select-Object Name, Directory | Sort-Object Name | Format-Table -AutoSize
```

### 8.5 — Find where a specific command lives

```powershell
Get-Command node    # Where does 'node' resolve to?
Get-Command npm
Get-Command pnpm
(Get-Command node).Source   # Just the path
```

### 8.6 — Separate User PATH vs System PATH

```powershell
# User PATH (your account only)
[Environment]::GetEnvironmentVariable("PATH", "User") -split ';' |
    Where-Object { $_ -ne '' }

# System PATH (all users)
[Environment]::GetEnvironmentVariable("PATH", "Machine") -split ';' |
    Where-Object { $_ -ne '' }
```

### 8.7 — Visualize PATH (pretty table with existence check)

```powershell
$env:PATH -split ';' | Where-Object { $_ -ne '' } | ForEach-Object {
    [PSCustomObject]@{
        Path   = $_
        Exists = (Test-Path $_)
        Files  = if (Test-Path $_) { (Get-ChildItem $_ -File -ErrorAction SilentlyContinue).Count } else { 0 }
    }
} | Format-Table -AutoSize
```

---

## 9. Filesystem Deep-Dive Audit

### 9.1 — Key locations to check

```powershell
# All the common spots where dev tools live:
$locations = @(
    # npm / node
    "$env:APPDATA\npm",
    "$env:APPDATA\npm-cache",
    "$env:LOCALAPPDATA\npm-cache",

    # pnpm
    "$env:LOCALAPPDATA\pnpm",
    "$env:APPDATA\pnpm",

    # Node version managers
    "$env:LOCALAPPDATA\Volta",
    "$env:LOCALAPPDATA\fnm",
    "$env:APPDATA\nvm",
    "$env:USERPROFILE\.nvmrc",

    # Runtime tools
    "$env:USERPROFILE\.bun",
    "$env:USERPROFILE\.deno",
    "$env:USERPROFILE\.cargo",

    # Windows package managers
    "$env:USERPROFILE\scoop",
    "C:\ProgramData\chocolatey",

    # Standard programs
    "C:\Program Files\nodejs",
    "C:\Program Files (x86)\nodejs"
)

foreach ($loc in $locations) {
    $exists = Test-Path $loc
    $icon = if ($exists) { "✅" } else { "❌" }
    Write-Host "$icon $loc"
}
```

### 9.2 — Find all node_modules on your whole drive (careful — slow!)

```powershell
# This can take a while on a full drive. Limit the search scope:
Get-ChildItem C:\Users\$env:USERNAME -Filter "node_modules" -Directory -Recurse -ErrorAction SilentlyContinue |
    Select-Object FullName, LastWriteTime |
    Sort-Object LastWriteTime -Descending
```

### 9.3 — Find all package.json files (your projects)

```powershell
Get-ChildItem C:\Users\$env:USERNAME -Filter "package.json" -Recurse -ErrorAction SilentlyContinue |
    Where-Object { $_.FullName -notmatch "node_modules" } |
    Select-Object FullName, LastWriteTime
```

### 9.4 — Check disk usage of key tool directories

```powershell
function Get-FolderSize($path) {
    if (-not (Test-Path $path)) { return "Not found" }
    $bytes = (Get-ChildItem $path -Recurse -ErrorAction SilentlyContinue |
        Measure-Object -Property Length -Sum).Sum
    "{0:N1} MB" -f ($bytes / 1MB)
}

$dirs = @(
    "$env:APPDATA\npm",
    "$env:APPDATA\npm-cache",
    "$env:LOCALAPPDATA\npm-cache",
    "$env:LOCALAPPDATA\pnpm",
    "$env:USERPROFILE\.bun",
    "$env:USERPROFILE\.deno",
    "$env:USERPROFILE\.cargo",
    "$env:USERPROFILE\scoop"
)

foreach ($d in $dirs) {
    Write-Host ("{0,-45} {1}" -f $d, (Get-FolderSize $d))
}
```

---

## 10. Environment Variables Audit

> Environment variables control how tools behave. Many installers set them silently.

### 10.1 — View all environment variables

```powershell
Get-ChildItem Env: | Sort-Object Name | Format-Table -AutoSize
```

### 10.2 — Find dev-tool-related environment variables

```powershell
Get-ChildItem Env: | Where-Object {
    $_.Name -match "NODE|NPM|PNPM|YARN|BUN|DENO|NVM|VOLTA|CARGO|RUST|GO|JAVA|PYTHON|PATH|HOME"
} | Format-Table Name, Value -AutoSize
```

### 10.3 — Check specific variables

```powershell
$env:NODE_ENV          # "development", "production", etc.
$env:NODE_PATH         # Extra module search paths
$env:NPM_CONFIG_PREFIX # npm global prefix override
$env:PNPM_HOME         # pnpm home directory
$env:BUN_INSTALL       # Bun install directory
$env:DENO_INSTALL      # Deno install directory
$env:CARGO_HOME        # Cargo (Rust) home
$env:VOLTA_HOME        # Volta home
```

### 10.4 — View User vs System environment variables separately

```powershell
# User-level variables
[Environment]::GetEnvironmentVariables("User").GetEnumerator() |
    Sort-Object Name | Format-Table Name, Value -AutoSize

# System-level variables
[Environment]::GetEnvironmentVariables("Machine").GetEnumerator() |
    Sort-Object Name | Format-Table Name, Value -AutoSize
```

### 10.5 — Open the GUI environment variable editor

```powershell
# Opens the "Edit System Environment Variables" dialog
rundll32 sysdm.cpl,EditEnvironmentVariables
```

---

## 11. Startup Programs & Services Audit

> Some tools (like Volta, certain version managers) modify startup behavior.

### 11.1 — Check startup programs (current user)

```powershell
Get-ItemProperty HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\Run |
    Select-Object * -ExcludeProperty PS*
```

### 11.2 — Check startup programs (all users)

```powershell
Get-ItemProperty HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Run |
    Select-Object * -ExcludeProperty PS*
```

### 11.3 — Check scheduled tasks related to dev tools

```powershell
Get-ScheduledTask |
    Where-Object { $_.TaskName -match "node|npm|yarn|pnpm|volta|nvm|bun|deno|chocolatey|scoop" } |
    Select-Object TaskName, State, TaskPath
```

### 11.4 — Check Windows services

```powershell
Get-Service | Where-Object {
    $_.DisplayName -match "node|npm|yarn|volta|chocolatey"
} | Select-Object DisplayName, Status, StartType
```

---

## 12. All-in-One Audit Script

> Save this as `audit-tools.ps1` and run it anytime to get a full picture.

```powershell
# ============================================================
# audit-tools.ps1 — Windows Dev Tools Full Audit
# Run: .\audit-tools.ps1
# ============================================================

function Section($title) {
    Write-Host "`n" -NoNewline
    Write-Host ("=" * 60) -ForegroundColor Cyan
    Write-Host "  $title" -ForegroundColor Yellow
    Write-Host ("=" * 60) -ForegroundColor Cyan
}

function TryCmd($cmd, $args) {
    try {
        & $cmd @args 2>$null
    } catch {
        Write-Host "  (not available)" -ForegroundColor DarkGray
    }
}

Section "SYSTEM INFO"
Write-Host "  Date      : $(Get-Date)"
Write-Host "  User      : $env:USERNAME"
Write-Host "  Computer  : $env:COMPUTERNAME"
Write-Host "  OS        : $((Get-WmiObject Win32_OperatingSystem).Caption)"

Section "NODE.JS & npm"
TryCmd node @("-v")
TryCmd npm @("-v")
Write-Host "  npm globals:"
TryCmd npm @("list", "-g", "--depth=0")
Write-Host "  npm prefix : $(npm config get prefix 2>$null)"
Write-Host "  npm cache  : $(npm config get cache 2>$null)"

Section "pnpm"
if (Get-Command pnpm -ErrorAction SilentlyContinue) {
    TryCmd pnpm @("-v")
    Write-Host "  pnpm globals:"
    TryCmd pnpm @("list", "-g", "--depth=0")
    Write-Host "  pnpm store : $(pnpm store path 2>$null)"
} else {
    Write-Host "  pnpm not found" -ForegroundColor DarkGray
}

Section "COREPACK"
if (Get-Command corepack -ErrorAction SilentlyContinue) {
    TryCmd corepack @("-v")
    TryCmd corepack @("ls")
} else {
    Write-Host "  corepack not found" -ForegroundColor DarkGray
}

Section "npx CACHE"
$npxCache = Join-Path (npm config get cache 2>$null) "_npx"
if (Test-Path $npxCache) {
    $entries = Get-ChildItem $npxCache -Directory -ErrorAction SilentlyContinue
    Write-Host "  npx cache entries: $($entries.Count)"
    $entries | Select-Object -First 10 | ForEach-Object {
        Write-Host "    - $($_.Name)  [$($_.LastWriteTime)]"
    }
} else {
    Write-Host "  No npx cache found"
}

Section "OTHER RUNTIMES"
$runtimes = @("bun", "deno", "yarn", "volta", "fnm", "nvm", "cargo", "go", "zig", "rustup")
foreach ($r in $runtimes) {
    $c = Get-Command $r -ErrorAction SilentlyContinue
    if ($c) {
        Write-Host "  ✅ $r → $($c.Source)"
    } else {
        Write-Host "  ❌ $r not in PATH" -ForegroundColor DarkGray
    }
}

Section "WINDOWS PACKAGE MANAGERS"
foreach ($pm in @("scoop", "choco", "winget")) {
    $c = Get-Command $pm -ErrorAction SilentlyContinue
    if ($c) { Write-Host "  ✅ $pm found: $($c.Source)" }
    else     { Write-Host "  ❌ $pm not found" -ForegroundColor DarkGray }
}

Section "PATH ENTRIES"
$pathEntries = $env:PATH -split ';' | Where-Object { $_ -ne '' }
Write-Host "  Total entries: $($pathEntries.Count)"
$pathEntries | ForEach-Object {
    $exists = Test-Path $_
    $icon   = if ($exists) { "✅" } else { "⚠️ " }
    Write-Host "  $icon $_"
}

Section "KEY DIRECTORIES"
$dirs = @(
    "$env:APPDATA\npm",
    "$env:LOCALAPPDATA\pnpm",
    "$env:LOCALAPPDATA\Volta",
    "$env:LOCALAPPDATA\fnm",
    "$env:APPDATA\nvm",
    "$env:USERPROFILE\.bun",
    "$env:USERPROFILE\.deno",
    "$env:USERPROFILE\.cargo",
    "$env:USERPROFILE\scoop",
    "C:\ProgramData\chocolatey"
)
foreach ($d in $dirs) {
    $exists = Test-Path $d
    $icon   = if ($exists) { "✅" } else { "  " }
    Write-Host "  $icon $d"
}

Section "DONE"
Write-Host "  Audit complete. Review output above." -ForegroundColor Green
```

**How to run it:**

```powershell
# Allow script execution (one-time setup)
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser

# Run the audit
.\audit-tools.ps1

# Save output to a file
.\audit-tools.ps1 | Tee-Object -FilePath audit-report.txt
```

---

## 13. User-Specific Tweaks & Customizations

### 13.1 — Set a consistent npm global prefix (prevents permission issues)

```powershell
# Create a dedicated folder for npm globals
$npmGlobalDir = "$env:USERPROFILE\npm-global"
New-Item -ItemType Directory -Force -Path $npmGlobalDir

# Tell npm to use it
npm config set prefix $npmGlobalDir

# Add it to your PATH permanently
$userPath = [Environment]::GetEnvironmentVariable("PATH", "User")
if ($userPath -notmatch [regex]::Escape($npmGlobalDir)) {
    [Environment]::SetEnvironmentVariable(
        "PATH",
        "$userPath;$npmGlobalDir\bin",
        "User"
    )
    Write-Host "✅ Added $npmGlobalDir\bin to your user PATH"
} else {
    Write-Host "✅ Already in PATH"
}
```

### 13.2 — Switch between Node.js versions

> If you work on multiple projects needing different Node versions, use a version manager.

```powershell
# Option A: fnm (fast, Rust-based)
winget install Schniz.fnm
fnm install 18       # Install Node 18
fnm install 20       # Install Node 20
fnm use 18           # Switch to Node 18
fnm list             # List installed versions

# Option B: Volta (project-pinning)
winget install Volta.Volta
volta install node@18
volta pin node@20    # Pin version for current project

# Check active Node version
node -v
```

### 13.3 — Profile-based auto-setup (PowerShell profile)

Add tool initializations to your PowerShell profile so they load automatically:

```powershell
# Open your PowerShell profile for editing
notepad $PROFILE

# Add these lines as needed:
# ── fnm ──────────────────────────────────────────────────
fnm env --use-on-cd | Out-String | Invoke-Expression

# ── pnpm ─────────────────────────────────────────────────
$env:PNPM_HOME = "$env:LOCALAPPDATA\pnpm"
$env:PATH = "$env:PNPM_HOME;$env:PATH"

# ── bun ──────────────────────────────────────────────────
$env:BUN_INSTALL = "$env:USERPROFILE\.bun"
$env:PATH = "$env:BUN_INSTALL\bin;$env:PATH"

# ── cargo (Rust) ─────────────────────────────────────────
$env:PATH = "$env:USERPROFILE\.cargo\bin;$env:PATH"

# ── deno ─────────────────────────────────────────────────
$env:DENO_INSTALL = "$env:USERPROFILE\.deno"
$env:PATH = "$env:DENO_INSTALL\bin;$env:PATH"
```

### 13.4 — Check and reload your PowerShell profile

```powershell
# Where your profile lives
$PROFILE

# Reload without restarting PowerShell
. $PROFILE
```

### 13.5 — Per-project .npmrc (isolate settings per project)

Create a `.npmrc` file in your project folder:

```ini
# .npmrc — project-level settings
engine-strict=true
save-exact=true
audit-level=high
```

---

## 14. Uninstalling / Cleaning Up

### 14.1 — Remove a global npm package

```powershell
npm uninstall -g <package-name>
```

### 14.2 — Remove a global pnpm package

```powershell
pnpm remove -g <package-name>
```

### 14.3 — Clean npm cache

```powershell
npm cache clean --force
npm cache verify     # Verify cache integrity without deleting
```

### 14.4 — Clean pnpm store (remove unused packages)

```powershell
pnpm store prune
```

### 14.5 — Remove Node.js version manager installs

```powershell
# fnm: remove a version
fnm uninstall 18

# Volta: remove a tool
volta uninstall node
```

### 14.6 — Remove PATH entries permanently

```powershell
# Read current user PATH
$userPath = [Environment]::GetEnvironmentVariable("PATH", "User")

# Remove a specific entry (replace with the path you want to remove)
$pathToRemove = "C:\path\to\remove"
$newPath = ($userPath -split ';' | Where-Object { $_ -ne $pathToRemove }) -join ';'

# Write it back
[Environment]::SetEnvironmentVariable("PATH", $newPath, "User")
Write-Host "✅ Removed $pathToRemove from PATH"
```

### 14.7 — Uninstall Node.js completely

```powershell
# Via winget
winget uninstall OpenJS.NodeJS

# Or via Settings → Apps → search "Node.js"

# Then manually clean leftover folders:
Remove-Item "$env:APPDATA\npm" -Recurse -Force -ErrorAction SilentlyContinue
Remove-Item "$env:APPDATA\npm-cache" -Recurse -Force -ErrorAction SilentlyContinue
Remove-Item "$env:LOCALAPPDATA\npm-cache" -Recurse -Force -ErrorAction SilentlyContinue
Remove-Item "C:\Program Files\nodejs" -Recurse -Force -ErrorAction SilentlyContinue
```

---

## 15. FAQ

### ❓ Why does `npm list -g` show different packages than I expected?

**A:** You may have multiple Node.js installations (e.g., one from the official installer, one from nvm, one from fnm). Each has its own separate global package store. Run `Get-Command node` to see which Node.js is currently active, and check `npm root -g` to see where *that* npm stores globals.

---

### ❓ I ran `npx some-tool` months ago. Is it still on my system?

**A:** Maybe. npx may have cached it in `%LOCALAPPDATA%\npm-cache\_npx\`. Check with:
```powershell
Get-ChildItem (Join-Path (npm config get cache) "_npx") -ErrorAction SilentlyContinue
```
If it's there, run `npm cache clean --force` to remove it.

---

### ❓ What's the difference between npm, pnpm, yarn, and bun?

**A:** They all install JavaScript packages, but differ in speed, disk usage, and features:

| | npm | pnpm | yarn | bun |
|---|---|---|---|---|
| Speed | Moderate | Fast | Fast | Very Fast |
| Disk Usage | High (copies) | Low (links) | Medium | Low |
| Comes With Node? | Yes | No | No | No |
| Lockfile | package-lock.json | pnpm-lock.yaml | yarn.lock | bun.lockb |

---

### ❓ Is `irm … | iex` safe?

**A:** It depends entirely on the URL. This pattern downloads and executes a script directly — if the source is trustworthy (e.g., official Deno, Bun, or Scoop installers from their official domains), it's generally safe. If you're unsure, download the script first and inspect it before running:
```powershell
# Download without running
Invoke-WebRequest -Uri "https://example.com/install.ps1" -OutFile install.ps1
notepad install.ps1   # Inspect it
# Then if satisfied:
.\install.ps1
```

---

### ❓ Why are there two PATH variables (User and System)?

**A:** Windows maintains separate PATH lists:
- **System PATH** — applies to all users, requires admin rights to modify
- **User PATH** — applies only to your account, no admin needed

Your effective PATH is both combined. Dev tools installed without admin (npm, pnpm, bun, deno) usually go to User PATH.

---

### ❓ I see a tool in PATH but get "command not found" — why?

**A:** Common causes:
1. The PATH change was made *after* your current terminal opened → **restart the terminal**
2. The binary folder itself is in PATH, but the binary name is wrong → check the folder contents
3. There's a conflict — another tool with the same name comes earlier in PATH

```powershell
# Check which one wins
Get-Command <toolname> | Select-Object Name, Source

# Check if there are multiple versions
Get-Command <toolname> -All | Select-Object Name, Source
```

---

### ❓ How do I know which version manager is controlling my Node.js?

```powershell
# Where does node resolve to?
(Get-Command node).Source

# Typical paths:
# C:\Program Files\nodejs\node.exe        → Official installer
# C:\Users\...\AppData\Local\fnm\...      → fnm
# C:\Users\...\AppData\Local\Volta\...    → Volta
# C:\Users\...\AppData\Roaming\nvm\...    → nvm-windows
```

---

### ❓ Can I use npm and pnpm on the same machine?

**A:** Yes! They're independent. Just be aware that each has its own global store, and if you mix them in the same project you may get conflicting lockfiles. Stick to one per project.

---

## 16. Troubleshooting

### 🔧 "npm: command not found" after install

```powershell
# 1. Check if Node was installed correctly
Test-Path "C:\Program Files\nodejs\npm.cmd"

# 2. Check PATH
$env:PATH -split ';' | Where-Object { $_ -match "node" }

# 3. Add Node to PATH manually if missing
$nodePath = "C:\Program Files\nodejs"
$userPath = [Environment]::GetEnvironmentVariable("PATH", "User")
[Environment]::SetEnvironmentVariable("PATH", "$userPath;$nodePath", "User")

# 4. Restart terminal
```

---

### 🔧 npm install -g keeps asking for admin rights

```powershell
# Fix: point npm globals to a user-owned directory
npm config set prefix "$env:USERPROFILE\npm-global"
# Then add %USERPROFILE%\npm-global\bin to your PATH (see Section 13.1)
```

---

### 🔧 "EACCES" or "EPERM" permission errors

```powershell
# Check who owns the npm cache/global dirs
Get-Acl (npm config get cache) | Format-List

# Quick fix: take ownership
takeown /f (npm root -g) /r /d y
icacls (npm root -g) /grant "$env:USERNAME:(OI)(CI)F" /t
```

---

### 🔧 pnpm not found even though I installed it

```powershell
# pnpm installs itself to LOCALAPPDATA\pnpm
# Check if that path is in PATH:
$env:PATH -split ';' | Where-Object { $_ -match "pnpm" }

# If missing, add it:
$pnpmHome = "$env:LOCALAPPDATA\pnpm"
$userPath = [Environment]::GetEnvironmentVariable("PATH", "User")
[Environment]::SetEnvironmentVariable("PATH", "$userPath;$pnpmHome", "User")
# Then restart your terminal
```

---

### 🔧 Two conflicting versions of the same tool

```powershell
# See ALL locations where a command exists
Get-Command node -All | Select-Object Name, Source

# See which one runs first (the winner)
(Get-Command node).Source

# Reorder PATH to put your preferred one first
# Edit via GUI:
rundll32 sysdm.cpl,EditEnvironmentVariables
```

---

### 🔧 `corepack` shims override my installed yarn/pnpm

```powershell
# If corepack's yarn conflicts with your standalone install:
corepack disable yarn    # Remove corepack yarn shim

# Or disable corepack entirely
corepack disable
```

---

### 🔧 PowerShell says "execution of scripts is disabled"

```powershell
# Check current policy
Get-ExecutionPolicy -List

# Allow user scripts (safe default)
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

---

### 🔧 `irm | iex` install script failed silently

```powershell
# Check your execution policy
Get-ExecutionPolicy

# Check if the URL is reachable
Invoke-WebRequest -Uri "https://example.com/install.ps1" -UseBasicParsing

# Run with verbose output
$VerbosePreference = "Continue"
# Then re-run the install command
```

---

## 17. Glossary

| Term | Meaning |
|---|---|
| **npm** | Node Package Manager — the default package manager bundled with Node.js |
| **pnpm** | Performant npm — faster, disk-efficient alternative to npm |
| **npx** | Node Package Execute — runs a package without permanently installing it |
| **corepack** | Built-in Node.js tool that manages which yarn/pnpm version a project uses |
| **yarn** | Another npm alternative developed by Facebook |
| **bun** | A fast all-in-one JavaScript runtime + package manager |
| **deno** | A secure JavaScript/TypeScript runtime (alternative to Node.js) |
| **PATH** | An environment variable listing directories Windows searches for executables |
| **global install** | A package installed system-wide, accessible from any directory |
| **local install** | A package installed inside a specific project's `node_modules/` |
| **iex / Invoke-Expression** | PowerShell command that executes a string as code |
| **irm / Invoke-RestMethod** | PowerShell command that downloads content from a URL |
| **prefix** | The root directory npm uses for global installs |
| **shim** | A small wrapper script that forwards commands to the real tool |
| **store** | pnpm's central package storage — saves disk space by deduplicating packages |
| **version manager** | A tool (fnm, nvm, volta) that lets you switch between Node.js versions |
| **winget** | Microsoft's official Windows Package Manager |
| **scoop** | A user-space Windows package manager (no admin needed) |
| **chocolatey** | A popular Windows package manager for developer tools |
| **ExecutionPolicy** | Windows setting controlling which PowerShell scripts are allowed to run |

---

## 📌 Quick Reference Card

```
DISCOVER WHAT'S INSTALLED
═══════════════════════════════════════════════════════
npm list -g --depth=0          All npm globals
pnpm list -g --depth=0         All pnpm globals
corepack ls                    corepack-managed tools
winget list                    winget installs
scoop list                     scoop installs
choco list --local-only        chocolatey installs

WHERE DO THINGS LIVE?
═══════════════════════════════════════════════════════
npm root -g                    npm global packages
npm bin -g                     npm global binaries
pnpm root -g                   pnpm global packages
pnpm store path                pnpm content store
npm config get cache           npm/npx cache
$env:APPDATA\npm               default npm location
$env:LOCALAPPDATA\pnpm         default pnpm location
$env:USERPROFILE\.bun          bun location
$env:USERPROFILE\.deno         deno location

PATH TOOLS
═══════════════════════════════════════════════════════
$env:PATH -split ';'           Full PATH list
Get-Command <tool>             Where a command lives
Get-Command <tool> -All        All copies in PATH

CLEAN UP
═══════════════════════════════════════════════════════
npm uninstall -g <pkg>         Remove npm global
pnpm remove -g <pkg>           Remove pnpm global
npm cache clean --force        Clear npm/npx cache
pnpm store prune               Clean pnpm store
```

---

*Last updated: 2025 — Works with Node.js 16+, npm 8+, pnpm 8+, Windows 10/11*
