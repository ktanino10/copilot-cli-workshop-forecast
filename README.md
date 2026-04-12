# 📊 copilot-cli-workshop-forecast

> **GitHub Copilot CLI** で作成したワークショップ集客予測ダッシュボード

### 🌐 [▶ ダッシュボードを開く（GitHub Pages）](https://ktanino10.github.io/copilot-cli-workshop-forecast/)

![Copilot CLI](https://img.shields.io/badge/Built_with-Copilot_CLI-8b5cf6?style=for-the-badge&labelColor=0a0a0c)
![Status](https://img.shields.io/badge/Status-ONLINE-00ffd5?style=for-the-badge&labelColor=0a0a0c)

---

## 🎯 概要 / Overview

GitHub Workshop のイベント登録者数をリアルタイムで可視化・予測するインタラクティブHTMLダッシュボード。  
CSVをドラッグ&ドロップするだけで動作し、**データはブラウザ内にのみ保存（サーバー送信なし）**。  
データ分析ロジックからUI実装まで、すべて **GitHub Copilot CLI** との対話で構築しました。

> 💡 **今後の拡張性**: 現在はCSVファイルを手動で読み込む方式ですが、これは社内の誰でもすぐに使えるようにするためです。登録プラットフォームのAPIに直接接続すれば、CSV出力も不要で、ダッシュボードを開くだけで常に最新データが自動反映されます。

### 成果物

| ファイル | 内容 |
|---|---|
| `index.html` | メインダッシュボード（HTML単体で動作） |
| `mascot_duck.gif` | ラバーダッキー（言語切替演出用） |
| `mascot_mona.gif` | モナ（言語切替演出用） |
| `mascot_copilot.gif` | コパイロット（言語切替演出用） |

---

## 🔧 作り方 / How It Was Built

### 1. ダッシュボード生成

```bash
# Copilot CLI を起動して以下を指示:
# 「registrants_latest.csvを利用して集客予測ダッシュボードを作って。
#   累積・日別・企業別・UTMソース別のチャートを入れて。
#   線形回帰でイベント日までの予測を出して。」
```

Copilot CLI が自動で実行したこと：
1. CSVの構造を解析し、`registered_at` カラムから日次集計
2. 直近のトレンドデータで線形回帰モデルを構築
3. 楽観(+30%) / 基本 / 悲観(-30%) の3シナリオ予測
4. Chart.js を使ったHTML生成

### 2. 動的CSV読み込み対応

```
「HTMLにデータを埋め込むのではなく、CSVをドラッグ&ドロップで
 読み込む方式に変えて。localStorageに保存して次回自動復元も。」
```

→ ハードコードHTML → **動的ダッシュボード** に変換。CSVを差し替えるだけで最新データに更新。

### 3. UI カスタマイズ

→ 対話の中で段階的に実装：
- ダークテーマのHUDカラーパレット
- グリッド背景 + アニメーション
- KPIカードの面取り角（clip-path）+ モノスペースフォント
- ブート演出 → セクション順次フェードイン
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
「言語切り替え時にラバーダッキー、モナ、コパイロットが出るようにしたい」
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

### 操作

| ボタン / 操作 | 機能 |
|---|---|
| 📂 **CSVを変更** | 別のCSVファイルに差替え（localStorageもクリア） |
| 🎭 **DEMO** | 企業名→「A社, B社...」、UTM→「Source A, B...」に匿名化 |
| 📅 **EVENT DATE** | イベント日を変更（予測が自動再計算） |
| ⌨️ `en` キー入力 | 英語モードに切替（ラバーダッキー🦆 モナ🐙 コパイロット✨ が登場） |
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
| **Share Tech Mono** | モノスペースフォント（Google Fonts） |
| **Vanilla HTML/CSS/JS** | UI全体（フレームワーク不使用） |
| **GitHub Pages** | ホスティング |

---

## 📝 Copilot CLI で学んだこと

1. **段階的な進化**: テキストの指示だけでダッシュボードを段階的に進化させられる（静的HTML → 動的CSV → カスタムUI）
2. **セキュリティレビュー**: コードレビューエージェントで脆弱性を発見・修正（XSS、SRI、PII）
3. **UI表現力**: 抽象的なUI指示からCSSアニメーションまで実装可能
4. **単体HTML**: Chart.js CDN のみ使用。サーバーレスで誰にでも共有可能

---

## 🚀 GitHub Copilot CLI 初心者ガイド

このダッシュボードのようなプロジェクトを自分でも作りたい方向けのガイドです。

### インストール

```bash
# macOS / Linux
brew install copilot-cli

# Windows
winget install GitHub.Copilot
```

> 前提条件: [GitHub Copilot サブスクリプション](https://github.com/features/copilot/plans)が必要です

### 作業ディレクトリの準備

Copilot CLI は**起動したディレクトリのファイルを認識**します。  
まず専用の作業フォルダを作りましょう：

```bash
mkdir ~/workspace/my-project
cd ~/workspace/my-project
copilot
```

> 💡 **Tip**: alias を設定すると便利です：
> ```bash
> # ~/.zshrc または ~/.bashrc に追加
> alias ws='cd ~/workspace'
> alias co='copilot'
> ```
> これで `ws` → `co` と打つだけでワークスペースに移動して Copilot を起動できます。

### モード切替 — `Shift+Tab`

Copilot CLI には2つのモードがあります。`Shift+Tab` で切替：

| モード | 説明 | 使いどき |
|---|---|---|
| **Interactive** | 1ステップずつ確認しながら進む | ファイル変更を慎重にしたい時 |
| **Plan** | 計画を立ててからまとめて実行 | 大きな実装タスク |

> 🔑 **Plan モードのコツ**: 「〇〇を作りたい」と伝えると、Copilot が計画を提案 → 承認するとまとめて実行してくれます。このダッシュボードも Plan モードで一気に作りました。

### モデル選択 — `/model`

```
/model
```

で使用するAIモデルを選択できます。特徴：

| モデル | 特徴 |
|---|---|
| **Claude Sonnet** | デフォルト。バランス型。日常のコーディングに最適 |
| **Claude Opus** | 最高精度。複雑な分析や大規模リファクタリング向け |
| **GPT-5** | OpenAI系。別の視点が欲しい時に |

> 💡 **Tip**: 複雑なタスクは Opus、日常作業は Sonnet、と使い分けると効率的です。

### 便利なコマンド一覧

| コマンド | 説明 |
|---|---|
| `/help` | 全コマンド一覧を表示 |
| `/model` | AIモデルを切替 |
| `/diff` | 現在の変更差分をレビュー |
| `/review` | コードレビューエージェントを起動 |
| `/share` | セッションをMarkdownやGistに保存 |
| `/compact` | 会話履歴を要約してコンテキスト節約 |
| `/delegate` | 作業をGitHub上のCopilotに引き継ぎ（PR作成） |
| `/tasks` | バックグラウンドタスクの管理 |
| `/context` | トークン使用量の確認 |

### ファイルの参照 — `@` メンション

プロンプトに `@ファイル名` と書くと、そのファイルの内容をCopilotに渡せます：

```
@data.csv このCSVを分析してダッシュボードを作って
```

### 実践的なワークフロー例

```bash
# 1. 作業フォルダに移動
cd ~/workspace/my-dashboard

# 2. データファイルを配置
cp ~/Downloads/data.csv .

# 3. Copilot 起動
copilot

# 4. Shift+Tab で Plan モードに切替

# 5. 指示を出す
# 「@data.csv を分析して、インタラクティブなHTMLダッシュボードを作って。
#   Chart.jsを使って、レスポンシブにして。」

# 6. 計画を確認して承認 → 自動実装

# 7. 結果を確認
open index.html

# 8. 追加の指示
# 「UIをもっとカッコよくして」「デモモードを追加して」
```

### その他のTips

- **Experimental モード**: `copilot --experimental` で起動すると、最新の実験的機能（Autopilot等）が使えます
- **Shell コマンド実行**: `!ls` のように `!` を先頭につけると、Copilotを通さず直接シェルコマンドを実行できます
- **外部エディタ**: `Ctrl+G` で長いプロンプトを外部エディタで編集できます
- **カスタム指示**: リポジトリに `CLAUDE.md` や `.github/copilot-instructions.md` を置くと、Copilotに毎回の前提条件を伝えられます
