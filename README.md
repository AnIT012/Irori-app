# 🏠 Irori（いろり）

> 家族間の情報共有を 1 つにまとめた、インストール可能な家庭用ダッシュボード Web アプリ

「予定・やること・買い物・食事・薬・帰宅状況……」家族で散らばりがちな情報を、ひとつの画面（＝囲炉裏端）に集めることをコンセプトにした PWA です。スマホのホーム画面に追加してネイティブアプリのように使えます。

- **形態**: 単一 HTML ファイルの SPA（ビルド工程なし）
- **バックエンド**: Firebase（Authentication + Cloud Firestore）
- **ホスティング**: Netlify（静的配信）
- **対応**: モバイル前提のレスポンシブ UI / ダークモード / PWA インストール対応

---

## 📦 このリポジトリについて

`Irori-app` は、これまで 2 つに分かれていたリポジトリを **1 本に統合した正典（canonical）リポジトリ** です。

| 旧リポジトリ | 位置づけ | 本リポジトリでの扱い |
|---|---|---|
| [`annonymousIT/Irori`](https://github.com/annonymousIT/Irori) | 原型（プロトタイプ・約165関数） | アーカイブ（参考用に保存） |
| [`annonymousIT/Irori-Ver2`](https://github.com/annonymousIT/Irori-Ver2) | 第2世代（最新・約233関数 / v2.0.0） | アーカイブ（本リポジトリに集約） |

統合の中身：

- アプリ本体（`index.html`）は **Ver2 の最新版（v2.0.0）** を正典として採用。原型は上位互換のため機能のマージは不要でした。
- Ver2 に欠落していた **`netlify.toml`（デプロイ設定）を原型から移植**。
- `manifest.json` / `sw.js` / アイコン類は最新の Ver2 を採用。

> 旧 2 リポジトリの中間スナップショット（`Ver2/irori.html` など）は履歴として旧リポジトリ側に残しています。

---

## ✨ 主な機能

4 つのタブで家庭の情報を切り替えます。

| タブ | 内容 |
|---|---|
| 👨‍👩‍👧 **家族（ホーム）** | 家族メンバーの帰宅・在宅ステータス、今日の予定、ホームダッシュボード、ウィジェット |
| ✅ **やること** | 家族共有の TODO / タスク、買い物リスト |
| 🗓 **予定** | カレンダー、スケジュール、明日の予定 |
| 👤 **マイページ** | 個人設定、テーマ（ダーク/フォントサイズ）、緊急連絡先・暗証番号などの管理 |

データ領域（Firestore コレクション）として、以下を扱います：

`members`（家族メンバー）/ `schedules`（予定）/ `tasks`（やること）/ `shopping`（買い物）/
`meals`（食事）/ `medicine`（薬）/ `bath`（お風呂）/ `locations`（位置・帰宅）/
`status`・`status_events`（在宅ステータス）/ `tomorrow`（明日の予定）/ `emergency`（緊急連絡先）/
`codes`（暗証番号）/ `widgets`（ウィジェット）/ `settings`（設定）/ `feedbacks`（フィードバック）

※ すべて `iedash/{ROOM}` 配下のサブコレクションとして、家族（ルーム）単位で分離して保存されます。

---

## 🚀 ローカルでの動かし方

ビルド不要・静的ファイルのみなので、任意の静的サーバで配信するだけです。

```bash
# 例: Python の簡易サーバ
python -m http.server 8000
# → http://localhost:8000 を開く

# 例: Node の serve
npx serve .
```

> `file://` で直接開くと Firebase / Service Worker 周りが正しく動かないため、必ず HTTP(S) 経由で開いてください。

---

## ☁️ デプロイ（Netlify）

このリポジトリをそのまま Netlify に接続すれば配信できます。設定は [`netlify.toml`](netlify.toml) に同梱済みです。

```toml
[build]
  publish = "."

[build.processing]
  skip_processing = true
```

ビルドコマンドは不要（publish ディレクトリはリポジトリ直下）。

---

## 🔧 Firebase 設定

クライアント用 Firebase の設定（`apiKey` 等）は `index.html` 内にインラインで埋め込まれています。
Firebase の Web `apiKey` は仕様上クライアントに露出する前提の値ですが、**データ保護は Firestore セキュリティルールに依存** します。フォークして自分の Firebase プロジェクトで運用する場合は、

1. Firebase コンソールで新規プロジェクトを作成
2. `index.html` 内の `firebaseConfig` を自分の値に差し替え
3. Authentication と Firestore を有効化し、適切なセキュリティルールを設定

を行ってください。詳細は Issues を参照。

---

## 🗂 ファイル構成

```
Irori-app/
├── index.html      … アプリ本体（HTML+CSS+JS の単一ファイル）
├── manifest.json   … PWA マニフェスト
├── sw.js           … Service Worker（※現状アプリ側で無効化中。Issue 参照）
├── netlify.toml    … Netlify デプロイ設定
├── icon-192.png    … PWA アイコン
├── icon-512.png    … PWA アイコン（maskable）
└── icon.png        … アイコン素材
```

---

## 📝 ライセンス

未設定。利用条件は今後 Issue で検討予定です。
