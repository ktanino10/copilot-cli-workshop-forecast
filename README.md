# 📊 copilot-cli-workshop-forecast

> **GitHub Copilot CLI** で作成したワークショップ集客予測ダッシュボード  
> A workshop registration forecast dashboard — **Built entirely with GitHub Copilot CLI**

### 🌐 [▶ ダッシュボードを開く / Open Dashboard（GitHub Pages）](https://ktanino10.github.io/copilot-cli-workshop-forecast/)

📖 **[English README →](README_EN.md)**

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
| 🌐 **EN / JP** ボタン | ヘッダーのボタンで言語切替（マスコット演出付き） |

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

### そもそも GitHub Copilot CLI とは？

**ターミナル（黒い画面）で動くAIアシスタント**です。  
自然な日本語で「〇〇を作って」と話しかけるだけで、AIがコードを書き、ファイルを作り、コマンドを実行してくれます。  
プログラミングの知識がなくても、やりたいことを言葉で伝えれば形にしてくれるのが最大の特徴です。

### インストール

```bash
# macOS / Linux
brew install copilot-cli

# Windows
winget install GitHub.Copilot
```

> 前提条件: [GitHub Copilot サブスクリプション](https://github.com/features/copilot/plans)が必要です

### 起動方法

ターミナル（macOSなら「ターミナル.app」、Windowsなら「PowerShell」）を開いて：

```bash
copilot
```

と入力するだけ。初回はGitHubアカウントでのログインが求められます。

### 作業ディレクトリの準備（重要！）

Copilot CLI は**起動したフォルダの中にあるファイルを読み取れます**。  
つまり、データファイルや作業ファイルを置いてあるフォルダで起動することが大事です。

```bash
# 1. 作業用フォルダを作る
mkdir ~/workspace/my-project

# 2. そのフォルダに移動する
cd ~/workspace/my-project

# 3. ここで Copilot を起動する
copilot
```

> 💡 **毎回の移動が面倒な場合**: ショートカット（alias）を設定できます：
> ```bash
> # ~/.zshrc または ~/.bashrc に以下を追加（1回だけ）
> alias ws='cd ~/workspace'
> alias co='copilot'
> alias wsco='cd ~/workspace && copilot'  # 移動+起動を一発で
> ```
> `ws` で移動、`co` で起動、`wsco` なら一発で移動＆起動。

### モード切替 — `Shift+Tab`（これを知っているかで効率が激変）

Copilot CLI には**2つのモード**があります。キーボードの `Shift+Tab` で切り替えます：

| モード | どう動くか | いつ使う？ |
|---|---|---|
| **Interactive** | 1つ作業するたびに「これでいい？」と確認してくれる | 小さな修正や、何が起きるか確認しながら進めたい時 |
| **Plan** | まず「こうやって作るよ」と計画書を見せてくれる。OKしたらまとめて一気に実装 | ダッシュボードやアプリなど、大きなものを作りたい時 |

> 🔑 **Plan モードのコツ**: 「〇〇を作りたい」と伝えると、Copilot が作業計画を提案 → 中身を確認 → 承認するとまとめて全部作ってくれます。このダッシュボードも Plan モードで一気に作りました。

### モデル選択 — `/model`（AIの「頭脳」を選ぶ）

Copilot CLI では、裏で動いている**AIモデル（頭脳）を自分で選べます**。  
起動中に `/model` と入力すると選択画面が出ます：

| モデル | 分かりやすく言うと | おすすめの使い方 |
|---|---|---|
| **Claude Sonnet** | 標準モデル（デフォルト） | 普段の作業はこれでOK。速くてバランスが良い |
| **Claude Opus** | 最上位の高精度モデル | 複雑な分析、大量のコード変更、難しい問題を解かせたい時 |
| **GPT-5** | OpenAI社のモデル | Claudeとは違うアプローチの回答が欲しい時に |

> 💡 **使い分けのコツ**: 日常はSonnet、ここぞという時にOpus。GPT-5は「Claudeの回答がしっくりこない」時のセカンドオピニオンに。

### 便利なコマンド一覧（`/` で始まるスラッシュコマンド）

Copilot CLI の会話中に `/` で始まるコマンドを入力すると、特別な機能が使えます：

| コマンド | 何ができるか |
|---|---|
| `/help` | 全コマンドの一覧を表示。困ったらまずこれ |
| `/model` | AIモデル（頭脳）を切替。上の表を参照 |
| `/diff` | Copilot がどのファイルをどう変更したか、差分を一覧表示。レビューに便利 |
| `/review` | コードレビュー専用AIが起動。バグやセキュリティ問題を自動で見つけてくれる |
| `/share` | 今の会話内容をMarkdownファイルやGitHub Gistに保存。記録や共有に |
| `/compact` | 会話が長くなった時に要約してくれる。AIが過去の話を忘れにくくなる |
| `/delegate` | 今の作業をGitHub上のCopilotに引き継ぐ。自動でPull Requestを作ってくれる |
| `/tasks` | バックグラウンドで走っているタスク（テスト実行など）の状態を確認 |
| `/context` | 「今の会話でどれくらいAIの記憶（トークン）を使っているか」を表示 |

### ファイルの参照 — `@` メンション

プロンプト（AIへの指示文）に `@ファイル名` と書くと、**そのファイルの中身をAIに見せられます**：

```
@data.csv このCSVを分析してダッシュボードを作って
```

これにより、AIがファイルの構造やデータを理解した上で作業してくれます。  
画像ファイル（`@photo.png`）を渡して「この画像を元に〇〇して」も可能です。

### 実践的なワークフロー例（このダッシュボードの作り方）

```bash
# 1. 作業フォルダに移動
cd ~/workspace/my-dashboard

# 2. 分析したいデータファイルをフォルダに入れる
cp ~/Downloads/data.csv .

# 3. Copilot を起動
copilot

# 4. Shift+Tab を押して Plan モードに切替
#    （画面の表示が「plan」に変わればOK）

# 5. やりたいことを日本語で伝える
# 「@data.csv を分析して、インタラクティブなHTMLダッシュボードを作って。
#   Chart.jsを使って、レスポンシブにして。」

# 6. Copilot が計画を提示 → 内容を確認して承認
#    → AIが自動でファイルを生成！

# 7. 結果をブラウザで確認
open index.html

# 8. さらに改善したければ、追加の指示を出すだけ
# 「UIをもっとカッコよくして」
# 「デモモードを追加して」
# 「英語にも対応させて」
# → 何度でも対話しながら進化させられます
```

### その他のTips

| 操作 | 説明 |
|---|---|
| `copilot --experimental` | 最新の実験的機能（Autopilotモード等）を有効にして起動。Autopilotは、完了まで自動で進めてくれるモード |
| `!ls` （`!` + コマンド） | Copilotを通さず、直接シェルコマンドを実行。ファイル確認(`!ls`)やGit操作(`!git status`)に便利 |
| `Ctrl+G` | 長い指示文を書きたい時、普段使っているテキストエディタが開く。書き終えて保存するとCopilotに送信される |
| `CLAUDE.md` ファイル | プロジェクトのルートにこのファイルを置いておくと、Copilotが毎回自動で読み込む。「このプロジェクトはTypeScriptで書いて」等の前提条件を書いておける |
