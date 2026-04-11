# 📊 COCKPIT HUD — Workshop Forecast Dashboard

> **GitHub Copilot CLI** で作成した閃光のハサウェイ風コックピットHUDの集客予測ダッシュボード

### 🌐 [▶ ダッシュボードを開く（GitHub Pages）](https://ktanino10.github.io/copilot-cli-workshop-forecast/)

![HUD System](https://img.shields.io/badge/HUD-SYSTEM_ONLINE-00ffd5?style=for-the-badge&labelColor=050a0e)
![Copilot CLI](https://img.shields.io/badge/Built_with-Copilot_CLI-8b5cf6?style=for-the-badge&labelColor=050a0e)
![Status](https://img.shields.io/badge/SCAN-ACTIVE-ffa500?style=for-the-badge&labelColor=050a0e)

---

## 🎯 概要 / Overview

GitHub Workshop のイベント登録者数をリアルタイムで可視化・予測するインタラクティブHTMLダッシュボード。  
CSVをドラッグ&ドロップするだけで動作し、**データはブラウザ内にのみ保存（サーバー送信なし）**。  
データ分析ロジックからUI実装まで、すべて **GitHub Copilot CLI** との対話で構築しました。

### 成果物

| ファイル | 内容 |
|---|---|
| `index.html` | メインダッシュボード（HTML単体で動作） |
| `mascot_*.gif` | GitHub公式スティッカー（言語切替演出用） |

---

## 🔧 作り方 / How It Was Built

### 1. 元のダッシュボード生成

```bash
# Copilot CLI を起動して以下を指示:
# 「registrants_latest.csvを利用して集客予測ダッシュボードを作って。
#   累積・日別・企業別・UTMソース別のチャートを入れて。
#   線形回帰で4/17までの予測を出して。」
```

Copilot CLI が自動で実行したこと：
1. CSVの構造を解析し、`registered_at` カラムから日次集計
2. 直近21日間のデータで線形回帰モデルを構築
3. 楽観(+30%) / 基本 / 悲観(-30%) の3シナリオ予測
4. Chart.js を使ったHTML生成（GitHub dark テーマ）

### 2. 動的CSV読み込み対応

```
「HTMLにデータを埋め込むのではなく、CSVをドラッグ&ドロップで
 読み込む方式に変えて。localStorageに保存して次回自動復元も。」
```

→ 15MB のハードコードHTML → **30KB の動的ダッシュボード** に変換

### 3. UI を閃光のハサウェイ風に

```
「閃光のハサウェイなどのガンダムのコックピットのような感じにしたい」
```

→ 以下を対話の中で段階的に実装：
- シアン(#00ffd5) + アンバー(#ffa500) のHUDカラーパレット
- グリッド背景 + スキャンラインアニメーション
- KPIカードの面取り角（clip-path）+ モノスペースフォント
- HUDブート演出（「SYSTEM INITIALIZING...」→ セクション順次フェードイン）
- KPI値のカウントアップアニメーション

### 4. デモモード＆セキュリティ

```
「顧客にデモ用に見せたいので、Demo用ボタンを作り、
 それを押すとHTMLには生データの顧客情報名は表示させないようにして。
 例えば、A社、B社みたいな見せ方に変えて。」
```

→ デモモード実装 + セキュリティレビュー + XSS対策 + SRIハッシュ追加

### 5. 言語切替マスコット

```
「言語切り替え時にラバーダッキー、もな、copilotが出るようにしたい」
```

→ `en` / `jp` キー入力で3体のマスコットがバウンドアニメーションで登場

---

## 👀 使い方 / How to Use

### ダッシュボードを開く

**方法1: GitHub Pages（推奨）**

[▶ https://ktanino10.github.io/copilot-cli-workshop-forecast/](https://ktanino10.github.io/copilot-cli-workshop-forecast/)

**方法2: ローカル**
```bash
git clone https://github.com/ktanino10/copilot-cli-workshop-forecast.git
cd copilot-cli-workshop-forecast
open index.html    # macOS
# or: start index.html  (Windows)
```

> ※ サーバー不要。HTML単体で完結（Chart.js は CDN から読み込み）

### 起動シーケンス

CSV を読み込むと HUD ブートシーケンスが実行されます：

```
> SYSTEM INITIALIZING...
> ████████████████████ 100%
> [KPI] 314 → 予測 350 → +5.8/日 → 残り6日 → 企業142社
> ALL SYSTEMS OPERATIONAL ■
```

### 操作

| ボタン / 操作 | 機能 |
|---|---|
| 📂 **CSVを変更** | 別のCSVファイルに差替え（localStorageもクリア） |
| 🎭 **DEMO** | 企業名→「A社, B社...」、UTM→「Source A, B...」に匿名化 |
| 📅 **EVENT DATE** | イベント日を変更（予測が自動再計算） |
| ⌨️ `en` キー入力 | 英語モードに切替（マスコットが登場🦆🐙✨） |
| ⌨️ `jp` キー入力 | 日本語モードに切替 |

---

## 📋 対応CSVフォーマット

以下のヘッダーを含むCSVファイルに対応：

```
last_name,company,title,domain,registered_at,utm_source,utm_medium,utm_campaign,...
```

| カラム | 用途 |
|---|---|
| `registered_at` | 日次集計・累積計算（ISO 8601、JST変換） |
| `company` | 企業別ランキング・ユニーク企業数 |
| `utm_source` | 流入元分析（空欄 = "direct/none"） |

---

## 🔒 セキュリティ

| 対策 | 内容 |
|---|---|
| **XSS防止** | 全CSV由来データを `escapeHtml()` でサニタイズ |
| **SRI** | Chart.js CDNに `integrity` ハッシュ付き |
| **PII保護** | CSVはリポジトリに含まれない（`.gitignore`で除外） |
| **ローカル完結** | データはブラウザ `localStorage` のみ。サーバー送信なし |
| **デモモード** | 顧客プレゼン時に企業名・UTMを自動匿名化 |

---

## 🛠 技術スタック

| 技術 | 用途 |
|---|---|
| **GitHub Copilot CLI** | 全体の構築・分析・コーディング |
| **Chart.js 4.4** | インタラクティブチャート（CDN + SRI） |
| **Share Tech Mono** | HUD風モノスペースフォント（Google Fonts） |
| **Vanilla HTML/CSS/JS** | UI全体（フレームワーク不使用） |
| **GitHub Pages** | ホスティング |

---

## 📝 Copilot CLI で学んだこと

1. **段階的な進化**: テキストの指示だけでダッシュボードを段階的に進化させられる（静的HTML → 動的CSV → コックピットUI）
2. **セキュリティレビュー**: コードレビューエージェントで脆弱性を発見・修正（XSS、SRI、PII）
3. **UI表現力**: 「閃光のハサウェイ風」という抽象的な指示からCSSアニメーションまで実装可能
4. **単体HTML**: Chart.js CDN のみ使用。サーバーレスで誰にでも共有可能

---

<div align="center">

**/// Ξ GUNDAM — COCKPIT HUD FORECAST SYSTEM ///**

*Not even Mafty can stop this registration trend.*

</div>
