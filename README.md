# 📊 Workshop Forecast Dashboard

GitHub Workshop の登録者数をリアルタイムで可視化・予測するダッシュボード。  
閃光のハサウェイ風コックピットHUD UIを採用。

## 🚀 使い方

1. **[Dashboard を開く](https://ktanino10.github.io/workshop-forecast-dashboard/)**
2. CSVファイルをドラッグ&ドロップ（または「ファイルを選択」）
3. ダッシュボードが自動生成されます！

> 🔒 **データはブラウザ内のみに保存され、サーバーには送信されません。**

## ✨ 機能

| 機能 | 説明 |
|---|---|
| 📈 **累積予測チャート** | 線形回帰モデルによる3シナリオ予測（楽観/基本/悲観） |
| 📊 **日別登録者数** | バーチャートで日次トレンドを可視化 |
| 🏢 **企業別TOP15** | 登録者数の多い企業をランキング表示 |
| 📡 **UTMソース分析** | 流入元の内訳をドーナツチャートで表示 |
| 🎭 **デモモード** | 企業名・UTMを匿名化して顧客向けプレゼンに対応 |
| 📅 **イベント日変更** | 異なるイベントにも再利用可能 |
| 🌐 **日英切替** | キーボードで `en` / `jp` と入力（イースターエッグ） |
| 💾 **自動保存** | 次回アクセス時にデータを自動復元 |

## 📋 対応CSVフォーマット

以下のヘッダーを含むCSVファイルに対応:

```
last_name,company,title,domain,registered_at,utm_source,utm_medium,utm_campaign,...
```

- `registered_at`: ISO 8601 タイムスタンプ（例: `2026-03-10T00:20:42.704029Z`）
- `company`: 企業名
- `utm_source`: 流入元（空の場合は "direct/none" として集計）

## 🔒 セキュリティ

- CSVデータはリポジトリに**含まれません**（`.gitignore` で除外）
- HTMLエスケープによるXSS対策済み
- Chart.js CDNにSRI（Subresource Integrity）ハッシュ付き
- データはブラウザの localStorage のみに保存（サーバー送信なし）

## 🛠️ 技術スタック

- **HTML/CSS/JavaScript**（単一ファイル、サーバー不要）
- **Chart.js 4.4.4**（CDN）
- **Share Tech Mono**（Google Fonts）
- **GitHub Pages** でホスティング
