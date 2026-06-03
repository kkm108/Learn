# Ubuntu Across Every Device — Complete Beginner's Guide

A practical, step-by-step guide for installing, configuring, and using Ubuntu
across desktop PCs, laptops, Macs, Raspberry Pi, and using mobile devices
(iPad, iPhone, Android) as remote terminals.

---

## Table of Contents

1. [Understanding Your Fleet — What Goes Where](#1-understanding-your-fleet--what-goes-where)
2. [Before You Begin — Things Every Install Needs](#2-before-you-begin--things-every-install-needs)
3. [Desktop PCs — Full Ubuntu Desktop LTS Install](#3-desktop-pcs--full-ubuntu-desktop-lts-install)
4. [Laptops — Native Install and Dual Boot](#4-laptops--native-install-and-dual-boot)
5. [Mac — Intel and Apple Silicon](#5-mac--intel-and-apple-silicon)
6. [Raspberry Pi 4 and 5](#6-raspberry-pi-4-and-5)
7. [iPad and iPhone — Remote Terminal](#7-ipad-and-iphone--remote-terminal)
8. [Android Tablets and Phones — Remote Terminal](#8-android-tablets-and-phones--remote-terminal)
9. [Connecting Everything on Your Network](#9-connecting-everything-on-your-network)
10. [First Things to Do After Any Install](#10-first-things-to-do-after-any-install)
11. [Buying New Hardware — What to Get](#11-buying-new-hardware--what-to-get)
12. [Troubleshooting Common Problems](#12-troubleshooting-common-problems)

---

## 1. Understanding Your Fleet — What Goes Where

Before installing anything, it helps to understand what role each device type
plays best. Not every device needs to run Ubuntu — and trying to force Ubuntu
onto a device that isn't suited for it creates more problems than it solves.

### Role Assignment Table

| Device | Best Role | Ubuntu? | Notes |
|---|---|---|---|
| Desktop PC | **Primary recording/automation server** | ✅ Full install | Most control, best for running the recorder |
| Laptop | **Developer workstation or secondary server** | ✅ Full install | Dual boot keeps Windows as backup |
| Mac (Intel) | **Developer workstation** | ✅ Works well | Native install or dual boot |
| Mac (Apple Silicon M1/M2/M3) | **Developer workstation** | ⚠️ Limited | Virtual machine only, not native |
| Raspberry Pi 4/5 | **Lightweight server, remote agent** | ✅ Full install | Excellent for always-on tasks |
| iPad / iPhone | **Remote terminal only** | ❌ Not installable | Control other machines via SSH |
| Android tablet / phone | **Remote terminal only** | ❌ Not recommended natively | Control other machines via SSH |

### The Simplest Possible Setup

If you are just getting started and have one desktop PC and want everything
working quickly, do this:

1. Install Ubuntu Desktop LTS 24.04 on the desktop PC
2. Use your phone or tablet to connect via SSH when you are away from it
3. Expand to more machines once you are comfortable

---

## 2. Before You Begin — Things Every Install Needs

### What You Need for Any Ubuntu Installation

**A USB drive (8 GB or larger)**
You will flash the Ubuntu installer onto this. All data on the USB drive will
be erased, so use one you do not care about.

**The Ubuntu ISO file**
Download Ubuntu Desktop LTS 24.04 from the official site:
```
https://ubuntu.com/download/desktop
```
The file is about 5.7 GB and ends in `.iso`. This is called a disk image.

> **Why 24.04 LTS?** LTS means Long-Term Support — Canonical (the company
> behind Ubuntu) guarantees security updates and bug fixes until April 2029.
> Non-LTS versions get only 9 months of support. Always use LTS for servers
> and automation machines.

**A tool to write the ISO to the USB drive**
- On Windows: [Rufus](https://rufus.ie) (free, no install required)
- On Mac: [balenaEtcher](https://etcher.balena.io) (free)
- On Linux: balenaEtcher or the built-in `dd` command

### How to Create the Ubuntu USB Drive

**Using Rufus (Windows):**

1. Download and open Rufus (it's a single `.exe` file, no installation needed)
2. Plug in your USB drive
3. Under "Device", select your USB drive
4. Under "Boot selection", click "SELECT" and choose the Ubuntu `.iso` file
5. Leave all other settings at their defaults
6. Click "START"
7. If asked about ISO mode vs DD mode, choose **DD mode**
8. Wait 5–10 minutes. When it says "READY", your USB is prepared.

**Using balenaEtcher (Mac or Linux):**

1. Download and open balenaEtcher
2. Click "Flash from file" and select the Ubuntu `.iso`
3. Click "Select target" and choose your USB drive
4. Click "Flash!" and wait for it to finish

### Understanding Partitions (Plain English)

A **partition** is a section of a hard drive. Your hard drive is like a
warehouse; partitions are like rooms inside it. Ubuntu needs its own rooms.
You can:

- Give Ubuntu the **entire drive** (easiest, erases Windows completely)
- Give Ubuntu **part of the drive** (dual boot — keeps Windows too)

For automation machines where you want Ubuntu to be the only thing running,
giving it the full drive is simpler and more reliable.

---

## 3. Desktop PCs — Full Ubuntu Desktop LTS Install

Desktop PCs are the best candidates for a full Ubuntu install. They are
powerful, usually have no battery concerns, and are already stationary.

### What "Full Install" Means

Ubuntu takes over the entire hard drive. Windows is erased. The PC boots
directly into Ubuntu every time.

> **Before you erase Windows:** Back up anything important to an external
> drive or cloud storage. Once Ubuntu is installed, your Windows files are
> gone unless you backed them up.

### Step-by-Step Installation

**Step 1 — Enter the BIOS/UEFI**

This is the screen that appears before Windows loads. You access it by
pressing a specific key immediately when the PC turns on — before any logo
appears. The key varies by manufacturer:

| Manufacturer | Key to press |
|---|---|
| Dell | F2 or F12 |
| HP | F10 or Esc |
| Lenovo | F1 or F2 |
| ASUS | F2 or Delete |
| MSI | Delete |
| Acer | F2 or Delete |
| Gigabyte | Delete |

Turn on the PC and tap the key repeatedly until the BIOS screen appears.
If Windows starts loading, restart and try again — the window is very short
(1–2 seconds).

**Step 2 — Disable Secure Boot**

In the BIOS, find the "Secure Boot" option. It is usually under a tab called
"Security", "Boot", or "Authentication". Set it to **Disabled**.

> **What is Secure Boot?** It is a Windows security feature that prevents
> unauthorized bootloaders. Ubuntu can work with Secure Boot on modern
> hardware, but disabling it prevents 90% of beginners' installation problems.
> You can re-enable it after Ubuntu is installed if needed.

**Step 3 — Change Boot Order**

Still in the BIOS, find "Boot Order" or "Boot Priority". Move "USB Drive" or
"Removable Devices" to the top of the list. This tells the PC to start from
your USB drive before trying the hard drive.

Save and exit the BIOS. The standard key is F10, but look for a "Save &
Exit" option on screen.

**Step 4 — Boot from USB**

The PC will restart and boot from your Ubuntu USB drive. You will see a
purple Ubuntu screen. Wait 30–60 seconds.

**Step 5 — Try or Install**

You will see two options:
- "Try Ubuntu" — runs Ubuntu from the USB without changing your PC
- "Install Ubuntu" — begins the installation

Click **Install Ubuntu**.

**Step 6 — Installation Wizard**

Follow the prompts:

1. **Language:** English (or your preferred language)
2. **Keyboard layout:** Detect automatically or choose English (India)
3. **Network:** Connect to Wi-Fi now if you want — it downloads updates during install
4. **Updates and software:** Choose "Normal installation" and check
   "Install third-party software for graphics and Wi-Fi hardware"
   — this installs drivers that Ubuntu otherwise leaves out
5. **Installation type:** Choose "Erase disk and install Ubuntu"
   This erases Windows and gives Ubuntu the full disk.
6. **Time zone:** Kolkata (India Standard Time, UTC+5:30)
7. **Your name and password:** Create an account. Use a strong password.
   Check "Require my password to log in."

Click "Install Now" and confirm when asked. Installation takes 10–20 minutes.

**Step 7 — First Boot**

When installation finishes, you will be asked to remove the USB drive and
press Enter. The PC restarts into Ubuntu.

Log in with the password you created. You are done.

### Configuring Auto-Login (Required for Headless Operation)

For a machine that will run the recorder without anyone sitting at it,
configure it to log in automatically so the graphical session starts on boot:

```bash
sudo nano /etc/gdm3/custom.conf
```

Find this section and edit it:

```ini
[daemon]
AutomaticLoginEnable = true
AutomaticLogin = yourusername
```

Replace `yourusername` with the account name you created during install.
Press Ctrl+X, then Y, then Enter to save.

Reboot to confirm it works:

```bash
sudo reboot
```

The machine should boot to the desktop without asking for a password.

### Enabling SSH (Control the Machine Remotely)

```bash
sudo apt install openssh-server
sudo systemctl enable --now ssh
```

Find the machine's IP address:

```bash
ip addr show | grep "inet " | grep -v 127
```

The output looks like `inet 192.168.1.100/24`. The IP address is
`192.168.1.100` — write this down.

From any other device on your network, you can now connect:

```bash
ssh yourusername@192.168.1.100
```

---

## 4. Laptops — Native Install and Dual Boot

Laptops present one extra consideration: you may want to keep Windows
alongside Ubuntu (dual boot), especially if the laptop needs to be used for
everyday Windows tasks too.

### Option A — Full Ubuntu Install (Recommended for Dedicated Automation)

Same steps as Section 3. The laptop becomes an Ubuntu machine permanently.

**Laptop-specific BIOS consideration:** Many laptops ship with the boot order
showing internal drive first and have no easy way to change it via a key. If
your USB drive is not booting, try pressing **F12** (or your manufacturer's
key) right at startup to get a one-time "Boot Menu" — a list of devices to
boot from right now, without permanently changing the BIOS settings.

### Option B — Dual Boot (Keep Windows and Ubuntu)

Dual boot lets you choose Windows or Ubuntu when the laptop starts.

**Before doing this, shrink the Windows partition:**

1. In Windows, right-click the Start button → Disk Management
2. Right-click the C: drive → "Shrink Volume"
3. Enter the amount to shrink in MB. For Ubuntu, shrink at least **50,000 MB**
   (50 GB). If you plan to do heavy recording, shrink 100,000 MB (100 GB).
4. Click Shrink. An "Unallocated" block will appear — this is where Ubuntu goes.

**Then follow the installation steps in Section 3, but at Step 6:**

Choose "Install Ubuntu alongside Windows" instead of "Erase disk".
Ubuntu will automatically use the unallocated space you created.

After installation, every time the laptop starts you will see the **GRUB
menu** — a simple text list that asks "Ubuntu" or "Windows Boot Manager".
Use arrow keys to choose and press Enter. If you do nothing, Ubuntu loads
after 10 seconds.

### Laptop Battery and Lid Behavior

For a laptop running as an unattended server, prevent it from sleeping when
the lid closes:

```bash
sudo nano /etc/systemd/logind.conf
```

Find and change these lines (uncomment them by removing the `#`):

```ini
HandleLidSwitch=ignore
HandleLidSwitchExternalPower=ignore
HandleLidSwitchDocked=ignore
```

Save, then apply:

```bash
sudo systemctl restart systemd-logind
```

The laptop can now stay on with the lid closed, plugged into power.

---

## 5. Mac — Intel and Apple Silicon

### Apple Silicon (M1, M2, M3, M4) — Virtual Machine Only

Ubuntu cannot be installed natively on Apple Silicon Macs. The ARM
architecture that Apple uses is incompatible with the standard AMD64 Ubuntu
installer.

**The solution: UTM (free virtual machine app)**

UTM runs Ubuntu inside a window on your Mac. It is not as fast as native, but
for development, SSH, and remote control of other machines it is completely
adequate.

1. Download UTM from [https://mac.getutm.app](https://mac.getutm.app) or the
   Mac App Store (the App Store version costs money; the website version is free)
2. Download the Ubuntu Server 24.04 ARM image from:
   `https://ubuntu.com/download/server/arm`
3. In UTM, click the `+` button → "Virtualize" → "Linux"
4. Click "Browse" and select the Ubuntu ARM `.iso`
5. Set RAM to at least **4 GB** (8 GB recommended)
6. Set disk size to at least **40 GB**
7. Click "Save" then the play button to start the VM

Ubuntu Server (without a graphical desktop) installs in the VM. This is
appropriate for an Apple Silicon Mac — use it as a terminal to SSH into your
main recording machine, or run lightweight automation tasks.

> **Why not Ubuntu Desktop on Apple Silicon?** Ubuntu Desktop 24.04 has
> experimental Apple Silicon support but Wi-Fi, display, and sleep/wake are
> unreliable. For serious work, use Server in a VM.

### Intel Mac — Native Dual Boot

Intel Macs can run Ubuntu natively alongside macOS.

**Step 1 — Make space on the Mac**

Open Disk Utility (Applications → Utilities → Disk Utility).
Select your main drive → Partition → click `+` to add a partition.
Name it "Ubuntu", set format to "MS-DOS (FAT)", set size to at least 50 GB.
Click Apply.

**Step 2 — Disable System Integrity Protection (SIP)**

Restart the Mac, hold **Command+R** until the Apple logo appears to enter
Recovery Mode. Open Terminal from the Utilities menu and type:

```bash
csrutil disable
```

Restart the Mac normally.

**Step 3 — Create the USB installer**

Use balenaEtcher to write the standard Ubuntu Desktop AMD64 `.iso` to a USB drive.

**Step 4 — Boot from USB**

Restart the Mac, hold the **Option (⌥) key**. A boot menu appears showing all
bootable drives. Select the Ubuntu USB drive (it may appear as "EFI Boot").

**Step 5 — Install**

Follow the installation wizard. At the "Installation type" screen, choose
"Something else" (the manual partitioning option):

- Find the FAT partition you created — it will show as `free space` or a FAT
  volume of the size you specified
- Click the `+` button, set it to "Ext4", mount point `/`
- Proceed with installation

**After installation:** Restart while holding the Option key — you will see
both "Macintosh HD" and "ubuntu" as boot options each time you start the Mac.

> **Wi-Fi on Intel Macs:** Broadcom Wi-Fi (used in most Intel Macs) requires
> a driver. After install, connect via ethernet and run:
> `sudo apt install bcmwl-kernel-source`
> Then disconnect ethernet — Wi-Fi will work.

---

## 6. Raspberry Pi 4 and 5

The Raspberry Pi is an excellent always-on server for lightweight tasks:
acting as a network hub, running scripts, hosting files, or proxying requests
to more powerful machines.

> **What the Pi cannot do:** The Raspberry Pi 4 and 5 use ARM processors
> and have limited RAM (up to 8 GB on Pi 5). Running a full Chrome browser
> with screen capture and FFmpeg encoding is too heavy for them. Use the Pi
> as a coordinator/scheduler that sends work to more powerful Ubuntu machines,
> not as the recorder itself.

### What You Need

- Raspberry Pi 4 (4 GB or 8 GB RAM recommended) or Raspberry Pi 5
- A microSD card (32 GB or larger, Class 10 or faster)
- USB-C power supply (official Pi power supply recommended)
- A computer to flash the SD card

### Flashing Ubuntu onto the SD Card

**The easiest method: Raspberry Pi Imager**

1. Download Raspberry Pi Imager from [https://rpi.imager.raspberrypi.com](https://rpi.imager.raspberrypi.com)
2. Insert your microSD card
3. Open Raspberry Pi Imager
4. Click "Choose OS" → "Other general-purpose OS" → "Ubuntu" →
   "Ubuntu Server 24.04 LTS (64-bit)"
   > **Why Server, not Desktop?** The Pi does not need a graphical desktop for
   > the automation role. Server uses far less RAM (200 MB vs 1.5 GB), leaving
   > more headroom for actual work.
5. Click "Choose Storage" → select your SD card
6. Click the gear icon (⚙️) to configure:
   - Enable SSH
   - Set a hostname (e.g., `pi-server`)
   - Set username and password
   - Configure Wi-Fi (network name and password)
7. Click "Write"

### First Boot and Finding the Pi on Your Network

Insert the SD card into the Pi, connect power. Wait 2–3 minutes for first-time
setup (it expands the filesystem automatically).

Find the Pi's IP address — there are two ways:

**From your router:** Log into your router's admin page (usually
`192.168.1.1` or `192.168.0.1` in a browser). Look for "Connected Devices"
or "DHCP Clients". Find the device named `pi-server` (or whatever hostname
you set).

**Using nmap from another Linux machine:**
```bash
sudo apt install nmap
nmap -sn 192.168.1.0/24   # adjust to your network range
```

Once you have the IP, SSH in:
```bash
ssh ubuntu@192.168.1.X
```
(Replace `ubuntu` with your configured username.)

### Giving the Pi a Fixed IP Address

By default, the Pi gets a different IP address each time it restarts. Fix
this by assigning a static IP in your router's DHCP settings, using the Pi's
MAC address. Look for "DHCP Reservation" or "Static DHCP" in your router's
admin panel.

Alternatively, set it in Ubuntu:

```bash
sudo nano /etc/netplan/50-cloud-init.yaml
```

Replace the content with:

```yaml
network:
  version: 2
  ethernets:
    eth0:
      addresses: [192.168.1.200/24]
      routes:
        - to: default
          via: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
  wifis:
    wlan0:
      dhcp4: true
      access-points:
        "YourWiFiName":
          password: "YourWiFiPassword"
```

Apply:
```bash
sudo netplan apply
```

---

## 7. iPad and iPhone — Remote Terminal

You cannot install Ubuntu on an iPad or iPhone. Their operating systems are
locked and do not permit alternative OS installs. But you can use them as
**remote terminals** — essentially, a screen and keyboard that connects to your
Ubuntu machine over the network via SSH.

This is actually very useful: you can check on your recording jobs, start or
stop recordings, and troubleshoot from anywhere using a device you already own.

### Best SSH App for iPad and iPhone: Blink Shell

**Blink Shell** is the highest-quality SSH client for iOS/iPadOS.
It costs a one-time fee ($19.99) but is worth it for serious use.

Free alternative: **SSH Files** (by Secure ShellFish) — excellent, with file
browsing. **a-Shell** is good for those who want a more terminal-like experience.

**Setting up Blink Shell:**

1. Open Blink Shell → tap the terminal area → type `config`
2. Under "Hosts", tap `+`:
   - Alias: `ubuntu-desktop` (any name you like)
   - Host: `192.168.1.100` (your Ubuntu machine's IP)
   - Port: `22`
   - User: `yourusername`
3. Save. Now type `ssh ubuntu-desktop` to connect.

**Keyboard shortcuts in Blink Shell on iPad:**
- Swipe left/right with two fingers to switch between sessions
- Use a Bluetooth keyboard for a full typing experience
- `Ctrl+D` to disconnect

### Mosh — Better Than SSH on Mobile

Mosh is an alternative to SSH that handles network interruptions gracefully.
If you lose your connection (Wi-Fi drops, phone goes to sleep), Mosh
automatically reconnects where you left off. SSH drops the session.

Install Mosh on your Ubuntu machine:
```bash
sudo apt install mosh
sudo ufw allow 60000:61000/udp   # open the ports Mosh uses
```

In Blink Shell, type `mosh ubuntu-desktop` instead of `ssh ubuntu-desktop`.

### Using the iPad as a Visual Dashboard

Beyond the terminal, you can use the iPad browser to view web dashboards
running on your Ubuntu machine. For example, if your recording system has a
web status page running on port 8080:

```
http://192.168.1.100:8080
```

Type this directly into Safari on the iPad. No setup required beyond
having both devices on the same network.

---

## 8. Android Tablets and Phones — Remote Terminal

Android gives you more flexibility than iOS — you can run a Linux environment
directly on the device using **Termux**, or use it as a remote terminal with
an SSH app, just like the iPad.

### Option A — SSH Client (Simplest, Recommended)

**Best app: JuiceSSH** (free with optional paid upgrade) or **Termius** (free
tier is sufficient for SSH).

**Setting up JuiceSSH:**

1. Install JuiceSSH from the Play Store
2. Open it → Connections → `+` to add a new connection
3. Enter your Ubuntu machine's IP address and username
4. Tap the connection to connect

That is it. You now have a terminal on your phone that controls your Ubuntu machine.

### Option B — Termux (Linux Environment on Android)

Termux runs a real Linux userspace directly on Android without root. You can
install `ssh`, `python`, `git`, `vim`, and hundreds of other tools. The
Android device becomes a lightweight Linux machine in itself.

**Installing Termux:**

> **Important:** Do not install Termux from the Play Store. The Play Store
> version is outdated and unmaintained. Use F-Droid instead.

1. Enable "Install from unknown sources" in Android Settings →
   Security → Unknown Sources (or per-app in newer Android)
2. Download and install F-Droid from [https://f-droid.org](https://f-droid.org)
3. In F-Droid, search for "Termux" and install it

**After installing Termux, update it and install SSH:**

```bash
pkg update && pkg upgrade
pkg install openssh
```

**Connect to your Ubuntu machine:**
```bash
ssh yourusername@192.168.1.100
```

**Useful Termux packages for automation tasks:**

```bash
pkg install python git curl wget vim nano
```

**Termux with a keyboard:** A Bluetooth keyboard transforms a large Android
tablet into a usable Linux workstation for lightweight tasks.

### Giving Android a Static IP

For an Android device that stays on your network (a tablet on a stand, for
example), go to: Settings → Wi-Fi → tap your network → Advanced → IP Settings
→ change from DHCP to Static. Set an IP address outside your router's DHCP
range (e.g., `192.168.1.150`).

---

## 9. Connecting Everything on Your Network

With multiple machines running Ubuntu, you need them to talk to each other
reliably. The key is giving every machine a stable IP address and knowing how
to reach each one.

### Recommended Network Layout

```
Your Router (192.168.1.1)
│
├── Desktop PC 1 (Ubuntu)    192.168.1.10  ← Main recording server
├── Desktop PC 2 (Ubuntu)    192.168.1.11
├── Desktop PC 3 (Ubuntu)    192.168.1.12
├── Laptop (Ubuntu)          192.168.1.20
├── Mac (UTM/Ubuntu)         192.168.1.30
├── Raspberry Pi             192.168.1.50
├── iPad                     192.168.1.100  ← SSH client
└── Android Tablet           192.168.1.101  ← SSH client
```

### Setting Up SSH Keys (Log In Without Passwords)

Typing a password every time you SSH into a machine becomes tedious with
multiple machines. SSH keys solve this: you generate a pair of keys (public
and private), put the public key on every Ubuntu machine, and from then on
you log in automatically.

**Generate a key pair on your main laptop/desktop (do this once):**

```bash
ssh-keygen -t ed25519 -C "your-email@example.com"
```

Press Enter three times to accept all defaults with no passphrase.

This creates two files:
- `~/.ssh/id_ed25519` — your **private key** (never share this)
- `~/.ssh/id_ed25519.pub` — your **public key** (safe to share)

**Copy the public key to each Ubuntu machine:**

```bash
ssh-copy-id yourusername@192.168.1.10
ssh-copy-id yourusername@192.168.1.11
ssh-copy-id yourusername@192.168.1.12
# ... repeat for each machine
```

You will be asked for the password once per machine. After that, `ssh` to any
of these machines requires no password.

### SSH Config File — Memorable Shortcuts

Instead of typing `ssh yourusername@192.168.1.10` every time, create an SSH
config file with short names:

```bash
nano ~/.ssh/config
```

Add an entry for each machine:

```
Host desktop1
    HostName 192.168.1.10
    User yourusername

Host desktop2
    HostName 192.168.1.11
    User yourusername

Host pi
    HostName 192.168.1.50
    User ubuntu

Host laptop
    HostName 192.168.1.20
    User yourusername
```

Now you connect with just:
```bash
ssh desktop1
ssh pi
ssh laptop
```

### Shared Network Storage (Access Files from Any Machine)

If one machine is recording video files and you want to access them from
another machine or a Windows PC, set up a Samba share:

**On the Ubuntu machine that stores recordings:**

```bash
sudo apt install samba
sudo nano /etc/samba/smb.conf
```

Add at the bottom:

```ini
[recordings]
   path = /home/yourusername/recordings
   read only = no
   browsable = yes
   valid users = yourusername
```

Set a Samba password (separate from your Ubuntu password):

```bash
sudo smbpasswd -a yourusername
sudo systemctl restart smbd
```

**Access from Windows:** Open File Explorer, type in the address bar:
```
\\192.168.1.10\recordings
```

**Access from another Ubuntu machine:**
```bash
sudo apt install smbclient cifs-utils
sudo mount -t cifs //192.168.1.10/recordings /mnt/recordings \
  -o username=yourusername
```

**Access from Mac:** In Finder, press Command+K and enter:
```
smb://192.168.1.10/recordings
```

---

## 10. First Things to Do After Any Install

Run these steps on every Ubuntu machine right after installation.

### Update Everything

```bash
sudo apt update && sudo apt upgrade -y
```

This downloads and installs all security patches. Takes 5–10 minutes on a
fresh install. Do this before anything else.

### Install Commonly Needed Tools

```bash
sudo apt install -y \
  curl \
  wget \
  git \
  vim \
  nano \
  htop \
  net-tools \
  openssh-server \
  build-essential \
  software-properties-common \
  unzip \
  tree \
  tmux
```

**What these are:**
- `curl` / `wget` — download files from the internet
- `git` — version control, needed for most projects
- `vim` / `nano` — text editors (`nano` is easier for beginners)
- `htop` — a visual process monitor (run it with `htop`, press Q to quit)
- `net-tools` — network utilities including `ifconfig`
- `build-essential` — compilers needed to build software from source
- `tmux` — keeps terminal sessions alive after you disconnect

### Set Up the Firewall

Ubuntu includes a firewall called `ufw` (Uncomplicated Firewall).
Enable it with SSH access allowed:

```bash
sudo ufw allow ssh
sudo ufw enable
```

Verify it is running:
```bash
sudo ufw status
```

### Set the Correct Time Zone

```bash
sudo timedatectl set-timezone Asia/Kolkata
timedatectl   # verify it shows IST
```

### Install Node.js (for the HTML recorder)

The Ubuntu package manager's version of Node.js is often outdated.
Install the current LTS version directly from NodeSource:

```bash
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt install -y nodejs
node --version   # should show v20.x or higher
npm --version
```

### Install FFmpeg (for recording)

```bash
sudo apt install -y ffmpeg
ffmpeg -version | head -1
```

### Set Up Automatic Security Updates

This keeps the machine patched without manual intervention:

```bash
sudo apt install -y unattended-upgrades
sudo dpkg-reconfigure --priority=low unattended-upgrades
```

Choose "Yes" when asked. Security patches now install automatically overnight.

### Create a systemd Service for the Recorder

This makes the recorder start automatically when the machine boots and
restart automatically if it crashes.

Create the service file:

```bash
sudo nano /etc/systemd/system/html-recorder.service
```

Paste this content (adjust paths to match your setup):

```ini
[Unit]
Description=HTML Video Recorder
After=graphical-session.target
Wants=graphical-session.target

[Service]
Type=simple
User=yourusername
WorkingDirectory=/home/yourusername/html-recorder
ExecStart=/home/yourusername/html-recorder/entrypoint.sh
Restart=on-failure
RestartSec=10
StandardOutput=journal
StandardError=journal
Environment=DISPLAY=:0
Environment=DBUS_SESSION_BUS_ADDRESS=unix:path=/run/user/1000/bus

[Install]
WantedBy=graphical-session.target
```

Enable and start it:

```bash
sudo systemctl daemon-reload
sudo systemctl enable html-recorder
sudo systemctl start html-recorder
```

Check it is running:
```bash
sudo systemctl status html-recorder
```

View logs:
```bash
journalctl -u html-recorder -f
```
(The `-f` flag means "follow" — it shows new log lines as they appear. Press Ctrl+C to stop.)

---

## 11. Buying New Hardware — What to Get

If you are purchasing new hardware specifically for Ubuntu automation and
recording, here is exactly what to look for.

### Primary Recording Machine (Desktop PC)

This is the machine that runs Ubuntu Desktop LTS and does the actual
recording. Everything else in your setup can be cheap — this one machine
determines your recording quality and reliability.

**Minimum configuration (will work, not ideal):**

| Component | Minimum | Why |
|---|---|---|
| CPU | Intel Core i5 (12th gen+) or AMD Ryzen 5 5000 series | FFmpeg software encoding is CPU-heavy |
| RAM | 16 GB | Chrome + FFmpeg + OS overhead |
| Storage | 512 GB SSD (NVMe preferred) | MP4 files fill up fast; NVMe prevents FFmpeg from waiting on disk |
| GPU | Integrated (Intel UHD or AMD Radeon) | Needed for Xvfb rendering and optional hardware encoding |
| Network | Gigabit ethernet port | Faster than Wi-Fi for reliability |

**Recommended configuration (comfortable for multiple simultaneous recordings):**

| Component | Recommended | Why |
|---|---|---|
| CPU | Intel Core i7/i9 (13th/14th gen) or AMD Ryzen 7/9 7000 series | Multiple recording instances in parallel |
| RAM | 32 GB | Multiple Chrome instances use RAM fast |
| Storage | 1 TB NVMe SSD + 4 TB HDD | NVMe for active work, HDD for archive |
| GPU | NVIDIA RTX 3060 or AMD RX 6600 | Hardware encoding (`h264_nvenc`/`h264_amf`) offloads CPU dramatically |
| Network | 2.5 Gigabit ethernet | If you are streaming recordings over the network |

**Specific recommendations available in India (as of 2024):**

- **Budget build:** AMD Ryzen 5 5600 + B550 motherboard + 16 GB DDR4 +
  512 GB NVMe — approximately ₹35,000–40,000 for the core components
- **Mid-range build:** AMD Ryzen 7 7700 + B650 motherboard + 32 GB DDR5 +
  1 TB NVMe — approximately ₹65,000–80,000
- **GPU addition:** NVIDIA RTX 3060 adds approximately ₹25,000–30,000 and
  dramatically speeds up encoding — worth it if you record at 60 fps often

> **Intel vs AMD for Ubuntu:** Both work well. AMD's integrated graphics
> (on APU models like Ryzen 5 5600G) work excellently with Ubuntu and
> can run Xvfb without a discrete GPU at all, making them good budget options.

### Raspberry Pi (Always-On Coordinator)

For the Pi role (scheduler, network agent, lightweight tasks):

- **Raspberry Pi 5 with 4 GB RAM** — ₹7,000–8,000 approximately
- **Official power supply** — essential, third-party supplies cause instability
- **SanDisk Extreme or Samsung Pro Endurance microSD** (64 GB) — ₹800–1,200
  Do not use cheap SD cards; they fail within months under constant read/write.
- **Raspberry Pi Active Cooler** (for Pi 5) — ₹600–800, prevents throttling

The Pi 4 with 4 GB RAM is also fine and cheaper if Pi 5 availability is low.

### Laptop (Developer Workstation)

If you need a portable Ubuntu machine:

- **ThinkPad L or E series (Lenovo):** Available in India, excellent Linux
  support, every component has upstream drivers, easy to repair.
  Specifically: ThinkPad E14 or L14 with AMD Ryzen — ₹50,000–70,000.
- **HP ProBook 440 G10 or G11:** Widely available in India, good Linux
  compatibility — ₹55,000–75,000.
- **Dell Latitude 5000 series:** Enterprise-grade, excellent driver support —
  ₹65,000–90,000.

**Avoid:** Gaming laptops with NVIDIA Optimus graphics switching unless you
know how to configure `prime-select`. The dual-GPU setup causes display and
sleep issues on Ubuntu that take significant troubleshooting.

### Networking Equipment

If you are setting up a dedicated network for multiple recording machines:

- **Router:** TP-Link Archer AX55 or AX73 (Wi-Fi 6, gigabit ethernet ports)
  — ₹5,000–8,000. Provides reliable DHCP and DHCP reservation for static IPs.
- **Network switch (if running ethernet to multiple machines):**
  TP-Link TL-SG108 (8-port gigabit unmanaged switch) — ₹1,200.
  Run ethernet cables to each recording machine; far more reliable than Wi-Fi
  for machines that are always on.

### What to Skip

**Do not buy:** Chromebooks (cannot run full Ubuntu without modifications and
they lose Google support). Ultrabooks with ARM processors (software compatibility
issues). Netbooks or Atom-based hardware (too slow for FFmpeg).

---

## 12. Troubleshooting Common Problems

### "Permission denied" when running a command with sudo

Your user account may not be in the `sudo` group. To check:
```bash
groups
```
You should see `sudo` in the output. If not:
```bash
su -
usermod -aG sudo yourusername
exit
```
Log out and back in for it to take effect.

### Wi-Fi not working after install

This is usually a missing driver. Connect via ethernet cable temporarily, then:

**For Broadcom Wi-Fi (common on older machines and Intel Macs):**
```bash
sudo apt install bcmwl-kernel-source
sudo modprobe wl
```

**For Realtek Wi-Fi:**
```bash
sudo apt install rtl8821ce-dkms   # or rtl8188eu-dkms depending on your chip
```

To identify your Wi-Fi chip:
```bash
lspci | grep -i wireless
# or for USB Wi-Fi:
lsusb
```

### Screen stays black after enabling auto-login

GDM (the display manager) may need a specific configuration for your GPU.
Try:
```bash
sudo nano /etc/gdm3/custom.conf
```
Add this line in the `[daemon]` section:
```ini
WaylandEnable=false
```
This forces Ubuntu to use X11 instead of Wayland. Then reboot.

### SSH connection refused

The SSH server may not be running:
```bash
sudo systemctl status ssh
sudo systemctl start ssh
sudo systemctl enable ssh
```

If the firewall is blocking it:
```bash
sudo ufw allow ssh
sudo ufw reload
```

### Machine runs slow after running FFmpeg

FFmpeg uses as much CPU as it can get. To limit it:
```bash
# Run FFmpeg with lower CPU priority
nice -n 10 ffmpeg ...

# Or limit to specific CPU cores:
taskset -c 0-3 ffmpeg ...   # uses only cores 0, 1, 2, 3
```

### Cannot connect from another machine (connection timed out)

Check the IP address is correct:
```bash
ip addr show
```

Check the firewall:
```bash
sudo ufw status verbose
```

Check the SSH service is listening:
```bash
ss -tlnp | grep 22
```

Ping the machine from another device:
```bash
ping 192.168.1.10
```
If ping fails but the machine is on, the firewall is blocking ICMP.
Allow it:
```bash
sudo ufw allow proto icmp
```

### Raspberry Pi will not boot

The most common cause is a bad SD card or a corrupt image.
Re-flash the SD card with Raspberry Pi Imager. Use a different SD card if
the problem persists — cheap SD cards fail silently.

For Pi 5 specifically: use only the official USB-C power supply (5V, 5A).
Underpowered supplies cause random freezes and corruption.

### Ubuntu Desktop freezes under heavy FFmpeg load

Increase swap space (virtual RAM using disk):
```bash
sudo fallocate -l 8G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

Also check if you are running out of RAM:
```bash
free -h
```

If `available` is under 500 MB while recording, add more RAM to the machine
or reduce the number of simultaneous recordings.

### How to Know if Everything Is Working

Run this check on any Ubuntu recording machine:

```bash
# Display server
echo $DISPLAY                          # should print :0

# Audio
pactl info | grep "Server Name"        # should print PulseAudio or PipeWire info

# FFmpeg can see the display
ffmpeg -f x11grab -r 1 -s 100x100 \
  -i :0 -t 1 -vframes 1 /tmp/test.jpg 2>/dev/null \
  && echo "DISPLAY CAPTURE: OK" \
  || echo "DISPLAY CAPTURE: FAILED"

# Node.js
node --version                         # should print v20.x or similar

# SSH
sudo systemctl is-active ssh           # should print "active"
```

All lines should show success. If anything fails, the relevant section above
covers the fix.
