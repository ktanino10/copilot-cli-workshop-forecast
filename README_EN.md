# 📊 copilot-cli-workshop-forecast

> A workshop registration forecast dashboard — **Built entirely with GitHub Copilot CLI**

### 🌐 [▶ Open Dashboard (GitHub Pages)](https://ktanino10.github.io/copilot-cli-workshop-forecast/)

![Copilot CLI](https://img.shields.io/badge/Built_with-Copilot_CLI-8b5cf6?style=for-the-badge&labelColor=0a0a0c)
![Status](https://img.shields.io/badge/Status-ONLINE-00ffd5?style=for-the-badge&labelColor=0a0a0c)

---

## 🎯 Overview

An interactive HTML dashboard that visualizes and forecasts GitHub Workshop event registrations in real time.  
Just drag & drop a CSV file — **data stays in your browser only (never sent to any server)**.  
From data analysis logic to UI implementation, everything was built through conversations with **GitHub Copilot CLI**.

> 💡 **Future Extensibility**: The current CSV-based approach is designed so anyone on the team can use it immediately. By connecting directly to a registration platform API, the CSV export step becomes unnecessary — the dashboard would always show the latest data automatically.

### Deliverables

| File | Description |
|---|---|
| `index.html` | Main dashboard (runs as standalone HTML) |
| `mascot_duck.gif` | Rubber Ducky (language switch animation) |
| `mascot_mona.gif` | Mona (language switch animation) |
| `mascot_copilot.gif` | Copilot (language switch animation) |

---

## 🔧 How It Was Built

### 1. Dashboard Generation

```bash
# Launched Copilot CLI and gave the following instruction:
# "Use registrants_latest.csv to build a registration forecast dashboard.
#  Include cumulative, daily, company, and UTM source charts.
#  Use linear regression to forecast up to the event date."
```

What Copilot CLI did automatically:
1. Analyzed CSV structure and aggregated by date using `registered_at`
2. Built a linear regression model on recent trend data
3. Generated 3-scenario forecasts: optimistic (+30%) / baseline / pessimistic (-30%)
4. Created HTML with Chart.js

### 2. Dynamic CSV Loading

```
"Instead of embedding data in the HTML, make it so users can
 drag & drop a CSV. Save to localStorage for auto-restore."
```

→ Converted from hardcoded HTML to a **dynamic dashboard**. Just swap the CSV for the latest data.

### 3. UI Customization

→ Iteratively implemented through conversation:
- Dark theme HUD color palette
- Grid background + animations
- Chamfered KPI cards (clip-path) + monospace font
- Boot sequence → staggered section fade-in
- KPI count-up animation with easeOutQuart easing

### 4. Demo Mode & Security

```
"I want a Demo button for customer presentations that hides
 real company names. Show them as Company A, Company B instead."
```

→ Demo mode implemented + security review + XSS protection + SRI hash for CDN

### 5. Language Switch Mascots

```
"When switching languages, I want Rubber Ducky, Mona,
 and Copilot mascots to appear"
```

→ Typing `en` / `jp` triggers all 3 mascots with bounce animation

---

## 👀 How to Use

### Open the Dashboard

**Option 1: GitHub Pages (Recommended)**

[▶ https://ktanino10.github.io/copilot-cli-workshop-forecast/](https://ktanino10.github.io/copilot-cli-workshop-forecast/)

**Option 2: Local**
```bash
git clone https://github.com/ktanino10/copilot-cli-workshop-forecast.git
cd copilot-cli-workshop-forecast
open index.html    # macOS
# or: start index.html  (Windows)
```

> No server required. Fully self-contained HTML (Chart.js loaded from CDN)

### Controls

| Button / Action | Function |
|---|---|
| 📂 **Change CSV** | Switch to a different CSV file (clears localStorage) |
| 🎭 **DEMO** | Anonymize company names → "A社, B社..." and UTM → "Source A, B..." |
| 📅 **EVENT DATE** | Change the target event date (forecast auto-recalculates) |
| ⌨️ Type `en` | Switch to English (Rubber Ducky 🦆 Mona 🐙 Copilot ✨ appear) |
| ⌨️ Type `jp` | Switch to Japanese |

---

## 📋 Supported CSV Format

CSV files with the following header are supported:

```
last_name,company,title,domain,registered_at,utm_source,utm_medium,utm_campaign,...
```

| Column | Usage |
|---|---|
| `registered_at` | Daily aggregation & cumulative (ISO 8601, converted to JST) |
| `company` | Company ranking & unique company count |
| `utm_source` | Traffic source analysis (empty = "direct/none") |

---

## 🔒 Security

| Measure | Details |
|---|---|
| **XSS Prevention** | All CSV-derived data sanitized with `escapeHtml()` |
| **SRI** | Chart.js CDN includes `integrity` hash |
| **PII Protection** | CSV files excluded from repo (`.gitignore`) |
| **Local Only** | Data stored in browser `localStorage` only. No server transmission |
| **Demo Mode** | Auto-anonymizes company names & UTM sources for presentations |

---

## 🛠 Tech Stack

| Technology | Purpose |
|---|---|
| **GitHub Copilot CLI** | End-to-end build — analysis, coding, UI design |
| **Chart.js 4.4** | Interactive charts (CDN + SRI) |
| **Share Tech Mono** | Monospace font (Google Fonts) |
| **Vanilla HTML/CSS/JS** | Entire UI (no frameworks) |
| **GitHub Pages** | Hosting |

---

## 📝 What I Learned Using Copilot CLI

1. **Iterative Evolution**: A dashboard can be progressively evolved with text instructions alone (static HTML → dynamic CSV → custom UI)
2. **Security Review**: The code review agent caught vulnerabilities and fixed them (XSS, SRI, PII)
3. **UI Expressiveness**: Abstract UI instructions can be translated into full CSS animations
4. **Single-File HTML**: Only Chart.js CDN used. Serverless, shareable with anyone

---

## 🚀 GitHub Copilot CLI Beginner's Guide

A guide for anyone who wants to build projects like this dashboard.

### What Is GitHub Copilot CLI?

It's an **AI assistant that runs in your terminal (command line)**.  
You simply tell it what you want in plain language — "build me a dashboard" — and it writes code, creates files, and runs commands for you.  
The biggest advantage: **you don't need programming knowledge**. Just describe what you want, and it builds it.

### Installation

```bash
# macOS / Linux
brew install copilot-cli

# Windows
winget install GitHub.Copilot
```

> Prerequisite: An active [GitHub Copilot subscription](https://github.com/features/copilot/plans) is required.

### How to Launch

Open your terminal (macOS: "Terminal.app", Windows: "PowerShell") and type:

```bash
copilot
```

That's it. On first launch, you'll be asked to log in with your GitHub account.

### Prepare Your Workspace (Important!)

Copilot CLI **can read files in the folder where it's launched**.  
This means you need to launch it from the folder that contains your data and project files.

```bash
# 1. Create a working folder
mkdir ~/workspace/my-project

# 2. Navigate to that folder
cd ~/workspace/my-project

# 3. Launch Copilot here
copilot
```

> 💡 **Tired of navigating every time?** Set up shortcuts (aliases):
> ```bash
> # Add to ~/.zshrc or ~/.bashrc (one-time setup)
> alias ws='cd ~/workspace'
> alias co='copilot'
> ```
> After this, just type `ws` to jump to your workspace, and `co` to launch Copilot.

### Mode Switching — `Shift+Tab` (This changes everything)

Copilot CLI has **two modes**. Press `Shift+Tab` on your keyboard to switch:

| Mode | How It Works | When to Use |
|---|---|---|
| **Interactive** | Does one thing at a time, asks "is this OK?" after each step | Small fixes, or when you want to review each change carefully |
| **Plan** | First shows you "here's my plan". After you approve, it builds everything at once | Building dashboards, apps, or anything with multiple steps |

> 🔑 **Plan Mode Tip**: Say "I want to build X" and Copilot proposes a step-by-step plan → review it → approve → it auto-implements everything. This dashboard was built entirely in Plan mode.

### Model Selection — `/model` (Choose the AI's "brain")

Copilot CLI lets you **choose which AI model (brain) powers it**.  
Type `/model` during a session to see the selection screen:

| Model | In Plain Terms | When to Use |
|---|---|---|
| **Claude Sonnet** | The standard model (default) | Everyday tasks. Fast and well-balanced |
| **Claude Opus** | The most powerful, highest-quality model | Complex analysis, large code changes, hard problems |
| **GPT-5** | OpenAI's model | When you want a different approach from Claude |

> 💡 **How to choose**: Sonnet for daily work, Opus for critical tasks. GPT-5 as a second opinion when Claude's answer doesn't feel right.

### Useful Commands (`/` Slash Commands)

During a conversation, type commands starting with `/` to access special features:

| Command | What It Does |
|---|---|
| `/help` | Shows all available commands. Start here when lost |
| `/model` | Switch the AI model (brain). See the table above |
| `/diff` | Shows exactly which files Copilot changed and how. Great for reviewing before committing |
| `/review` | Launches a dedicated code review AI. Automatically finds bugs and security issues in your code |
| `/share` | Saves your conversation as a Markdown file or GitHub Gist. Useful for documentation or sharing with teammates |
| `/compact` | Summarizes a long conversation. Helps the AI remember earlier context without running out of memory |
| `/delegate` | Hands off your work to GitHub's cloud Copilot. It automatically creates a Pull Request for you |
| `/tasks` | Checks the status of background tasks (tests, builds running in the background) |
| `/context` | Shows how much of the AI's memory (tokens) you've used in the current session |

### Referencing Files — `@` Mentions

Add `@filename` in your prompt to **show the AI the contents of that file**:

```
@data.csv Analyze this CSV and build a dashboard
```

This lets the AI understand your file structure and data before working on it.  
You can even pass image files (`@photo.png`) and say "use this image to create X".

### Practical Workflow Example (How This Dashboard Was Built)

```bash
# 1. Navigate to your working folder
cd ~/workspace/my-dashboard

# 2. Put your data file in the folder
cp ~/Downloads/data.csv .

# 3. Launch Copilot
copilot

# 4. Press Shift+Tab to switch to Plan mode
#    (the display should change to "plan")

# 5. Tell it what you want, in plain language
# "@data.csv Analyze this and create an interactive HTML dashboard.
#  Use Chart.js, make it responsive."

# 6. Copilot shows its plan → review it → approve
#    → AI generates all the files automatically!

# 7. Open the result in your browser
open index.html

# 8. Want to improve it? Just give more instructions
# "Make the UI cooler"
# "Add a demo mode"
# "Support English too"
# → You can iterate and evolve it through conversation
```

### Additional Tips

| Action | What It Does |
|---|---|
| `copilot --experimental` | Launches with cutting-edge experimental features like Autopilot mode — a mode where Copilot keeps working until the task is fully complete, without stopping to ask |
| `!ls` (prefix with `!`) | Runs a shell command directly, bypassing Copilot. Useful for quick checks like `!ls` (list files) or `!git status` |
| `Ctrl+G` | Opens your preferred text editor for writing long prompts. Save and close to send the text to Copilot |
| `CLAUDE.md` file | Place this file in your project root. Copilot reads it automatically every time. Write project-wide rules like "use TypeScript" or "follow this coding style" |
