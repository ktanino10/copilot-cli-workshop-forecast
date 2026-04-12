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

### Installation

```bash
# macOS / Linux
brew install copilot-cli

# Windows
winget install GitHub.Copilot
```

> Prerequisite: An active [GitHub Copilot subscription](https://github.com/features/copilot/plans) is required.

### Set Up Your Workspace

Copilot CLI **recognizes files in the directory where it's launched**.  
Start by creating a dedicated working folder:

```bash
mkdir ~/workspace/my-project
cd ~/workspace/my-project
copilot
```

> 💡 **Tip**: Set up aliases for quick access:
> ```bash
> # Add to ~/.zshrc or ~/.bashrc
> alias ws='cd ~/workspace'
> alias co='copilot'
> ```
> Then just type `ws` → `co` to jump to your workspace and launch Copilot.

### Mode Switching — `Shift+Tab`

Copilot CLI has two modes. Switch with `Shift+Tab`:

| Mode | Description | When to Use |
|---|---|---|
| **Interactive** | Step-by-step with confirmation | When you want careful control over changes |
| **Plan** | Creates a plan, then executes at once | For large implementation tasks |

> 🔑 **Plan Mode Tip**: Say "I want to build X" and Copilot will propose a plan → approve it → auto-implements everything. This dashboard was built using Plan mode.

### Model Selection — `/model`

```
/model
```

Choose your AI model. Characteristics:

| Model | Best For |
|---|---|
| **Claude Sonnet** | Default. Balanced. Great for everyday coding |
| **Claude Opus** | Highest quality. Complex analysis or large refactors |
| **GPT-5** | OpenAI family. When you want a different perspective |

> 💡 **Tip**: Use Opus for complex tasks, Sonnet for daily work.

### Essential Commands

| Command | Description |
|---|---|
| `/help` | Show all available commands |
| `/model` | Switch AI model |
| `/diff` | Review current changes |
| `/review` | Launch code review agent |
| `/share` | Save session as Markdown or Gist |
| `/compact` | Summarize conversation to save context |
| `/delegate` | Hand off work to GitHub Copilot (creates PR) |
| `/tasks` | Manage background tasks |
| `/context` | Check token usage |

### Referencing Files — `@` Mentions

Use `@filename` in your prompt to include file contents in context:

```
@data.csv Analyze this CSV and build a dashboard
```

### Practical Workflow Example

```bash
# 1. Navigate to your workspace
cd ~/workspace/my-dashboard

# 2. Place your data file
cp ~/Downloads/data.csv .

# 3. Launch Copilot
copilot

# 4. Press Shift+Tab to switch to Plan mode

# 5. Give your instruction
# "@data.csv Analyze this and create an interactive HTML dashboard
#  using Chart.js. Make it responsive."

# 6. Review the plan → approve → auto-implementation

# 7. Check the result
open index.html

# 8. Iterate with follow-up instructions
# "Make the UI cooler" "Add a demo mode" "Add dark mode"
```

### Additional Tips

- **Experimental mode**: Launch with `copilot --experimental` to access cutting-edge features like Autopilot
- **Shell commands**: Prefix with `!` (e.g., `!ls`) to run shell commands directly without going through Copilot
- **External editor**: Press `Ctrl+G` to edit long prompts in your preferred editor
- **Custom instructions**: Place `CLAUDE.md` or `.github/copilot-instructions.md` in your repo to give Copilot persistent context about your project
