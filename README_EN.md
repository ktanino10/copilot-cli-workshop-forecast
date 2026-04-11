# 📊 COCKPIT HUD — Workshop Forecast Dashboard

> A Gundam Hathaway's Flash-inspired cockpit HUD dashboard for workshop registration forecasting — **Built entirely with GitHub Copilot CLI**

### 🌐 [▶ Open Dashboard (GitHub Pages)](https://ktanino10.github.io/copilot-cli-workshop-forecast/)

![HUD System](https://img.shields.io/badge/HUD-SYSTEM_ONLINE-00ffd5?style=for-the-badge&labelColor=050a0e)
![Copilot CLI](https://img.shields.io/badge/Built_with-Copilot_CLI-8b5cf6?style=for-the-badge&labelColor=050a0e)
![Status](https://img.shields.io/badge/SCAN-ACTIVE-ffa500?style=for-the-badge&labelColor=050a0e)

---

## 🎯 Overview

An interactive HTML dashboard that visualizes and forecasts GitHub Workshop event registrations in real time.  
Just drag & drop a CSV file — **data stays in your browser only (never sent to any server)**.  
From data analysis logic to UI implementation, everything was built through conversations with **GitHub Copilot CLI**.

### Deliverables

| File | Description |
|---|---|
| `index.html` | Main dashboard (runs as standalone HTML) |
| `mascot_*.gif` | Official GitHub stickers (language switch animation) |

---

## 🔧 How It Was Built

### 1. Initial Dashboard Generation

```bash
# Launched Copilot CLI and gave the following instruction:
# "Use registrants_latest.csv to build a registration forecast dashboard.
#  Include cumulative, daily, company, and UTM source charts.
#  Use linear regression to forecast up to 4/17."
```

What Copilot CLI did automatically:
1. Analyzed CSV structure and aggregated by date using `registered_at`
2. Built a linear regression model on the last 21 days of data
3. Generated 3-scenario forecasts: optimistic (+30%) / baseline / pessimistic (-30%)
4. Created HTML with Chart.js (GitHub dark theme)

### 2. Dynamic CSV Loading

```
"Instead of embedding data in the HTML, make it so users can
 drag & drop a CSV. Save to localStorage for auto-restore."
```

→ Converted from a 15MB hardcoded HTML to a **30KB dynamic dashboard**

### 3. Gundam Cockpit UI (Hathaway's Flash)

```
"I want it to look like a cockpit from Hathaway's Flash / Gundam"
```

→ Iteratively implemented through conversation:
- Cyan (#00ffd5) + amber (#ffa500) HUD color palette
- Grid background + scan line animation
- Chamfered KPI cards (clip-path) + monospace font
- HUD boot sequence ("SYSTEM INITIALIZING..." → staggered section fade-in)
- KPI count-up animation with easeOutQuart easing

### 4. Demo Mode & Security

```
"I want a Demo button for customer presentations that hides
 real company names. Show them as Company A, Company B instead."
```

→ Demo mode implemented + security review + XSS protection + SRI hash for CDN

### 5. Language Switch Mascots

```
"When switching languages, I want the rubber ducky, Mona,
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

### Boot Sequence

After loading a CSV, the HUD boot sequence plays:

```
> SYSTEM INITIALIZING...
> ████████████████████ 100%
> [KPI] 314 → Forecast 350 → +5.8/day → 6 days left → 142 companies
> ALL SYSTEMS OPERATIONAL ■
```

### Controls

| Button / Action | Function |
|---|---|
| 📂 **Change CSV** | Switch to a different CSV file (clears localStorage) |
| 🎭 **DEMO** | Anonymize company names → "A社, B社..." and UTM → "Source A, B..." |
| 📅 **EVENT DATE** | Change the target event date (forecast auto-recalculates) |
| ⌨️ Type `en` | Switch to English mode (mascots appear 🦆🐙✨) |
| ⌨️ Type `jp` | Switch to Japanese mode |

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
| **Share Tech Mono** | HUD-style monospace font (Google Fonts) |
| **Vanilla HTML/CSS/JS** | Entire UI (no frameworks) |
| **GitHub Pages** | Hosting |

---

## 📝 What I Learned Using Copilot CLI

1. **Iterative Evolution**: A dashboard can be progressively evolved with text instructions alone (static HTML → dynamic CSV → cockpit UI)
2. **Security Review**: The code review agent caught vulnerabilities and fixed them (XSS, SRI, PII)
3. **UI Expressiveness**: Abstract instructions like "make it look like Hathaway's Flash" can be translated into full CSS animations
4. **Single-File HTML**: Only Chart.js CDN used. Serverless, shareable with anyone

---

<div align="center">

**/// Ξ GUNDAM — COCKPIT HUD FORECAST SYSTEM ///**

*Not even Mafty can stop this registration trend.*

</div>
