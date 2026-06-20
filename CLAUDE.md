# JazzPhrase.stock — CLAUDE.md

## プロジェクト概要

ジャズフレーズをストックして楽譜を見ながら練習できる個人用Webアプリ。
単一HTMLファイル（index.html）で構成されたVanilla JSアプリ。

## リポジトリ・デプロイ

- GitHub: `https://github.com/byeolsatang/jazz-phrase-stock`
- 本番URL: `https://jazz-phrase-stock.vercel.app`
- デプロイ: git pushで自動デプロイ（Vercel連携済み）

## ファイル構成

```
jazz-phrase-stock/
├── index.html    # アプリ本体（すべてのコード）
├── README.md
└── CLAUDE.md     # このファイル
```

## 技術スタック

- フロントエンド: HTML / CSS / Vanilla JS（単一ファイル）
- 楽譜表示: Flat.io Embed SDK（iframe埋め込み）
- スコア取得: Flat.io REST API
- データ永続化: Supabase（PostgreSQL）+ localStorage（キャッシュ）
- ホスティング: Vercel

## デザインシステム

```css
--bg:      #0a1018;   /* ページ背景 */
--surface: #182030;   /* サイドバー・ヘッダー */
--sur2:    #1e2a3a;   /* カード展開パネル */
--border:  #2a3d52;   /* ボーダー */
--terra:   #e8563a;   /* テラコッタ：アクセント・ボタン・上級・お気に入り */
--steel:   #7a9ab5;   /* スチールブルー：サブ情報 */
--ice:     #d6edf4;   /* アイスブルー：テキスト・強調 */
--navy:    #2d5070;   /* ネイビー：キー選択 */
--muted:   #5a7a94;   /* ミュート：補助テキスト */

/* タグカテゴリカラー */
--tc-scale:    #7ab8d4;  /* スケール */
--tc-approach: #c47a3a;  /* アプローチ */
--tc-rhythm:   #7a9ab5;  /* リズム */
--tc-source:   #9a7ab5;  /* 出典 */

/* フォント */
font-family: 'Space Mono'（見出し・ラベル）, 'Noto Sans JP'（本文）
```

## 主要機能

- Flat.io「Jazz Phrase」コレクションからスコアを自動取得・インポート
- 楽譜をiframe（Flat.io Embed SDK）で埋め込み表示・再生
- フレーズ管理：コード進行 / キー（メジャー＋マイナー） / 難易度 / タグ
- タグシステム：スケール・アプローチ・リズム・出典の4カテゴリ＋自由入力
- サイドバーフィルター＋インクリメンタルサーチ
- お気に入り★ / 練習済み✓
- Supabase同期（PC↔スマホ）
- レスポンシブ対応（モバイルはボトムナビ）

## Flat.io設定

- App ID: `6497b624ac329cd68ba7e51d`
- Embed URL形式: `https://flat.io/embed/{scoreId}?layout=compact&controlsPosition=bottom&appId=6497b624ac329cd68ba7e51d`
- コレクション名: `Jazz Phrase`（ユーザーのFlat.ioアカウント内）

## Supabase設定

- テーブル: `phrases`
- スキーマ:
  ```sql
  id bigint generated always as identity primary key,
  name text, prog text, key text, level text,
  score_id text, fav boolean, done boolean,
  tags jsonb, notes text, created_at timestamptz
  ```
- 同期方式: 全件DELETE → INSERT（id競合回避）
- URL・KeyはlocalStorageに保存（アプリ内⚙設定から入力）

## タグプリセット

### スケール
Bebop scale / Bebop 下降形 / Bebop 上昇形 / Major scale / Lydian /
Lydian dominant / Mixolydian / Dorian / Harmonic minor /
Harmonic minor 下降形 / Harmonic minor 上昇形 / Melodic minor 上昇形 /
Melodic minor 下降形 / Phrygian dominant / Major pentatonic /
Minor pentatonic / Blues scale / Blue note追加 / Altered scale /
Whole tone / Diminished scale / Half-dim scale

### アプローチ
半音アプローチ / クロマチック / エンクロージャー / コードトーン /
ガイドトーン / ダイアトニック / トライトーン代理 / 代理コード /
アウトサイド / ブルース進行 / リズムチェンジ

### リズム
シンコペ / トリプレット / 8分音符主体 / 16分音符 / ターン / ロングトーン / ラン

### 出典
Charlie Parker / John Coltrane / Bill Evans / Herbie Hancock /
McCoy Tyner / Oscar Peterson / Wynton Kelly / Red Garland / Bud Powell /
自作 / セッション採譜

## コード進行セレクタ

ii-V-I / ii-V / ターンアラウンド / ブルース / その他

## よく使うコマンド

```bash
# デプロイ
git add .
git commit -m "メッセージ"
git push

# ローカル確認（ブラウザで直接開く）
open index.html
```

## 注意事項

- Supabase同期は「全件DELETE → INSERT」方式（idをSupabase自動採番にするため）
- localStorageはオフラインバックアップとして使用
- Flat.io楽譜は「リンクを知っている人が閲覧可能」に設定しないとembedが表示されない
- トークン・APIキーはlocalStorageに保存（コードには含めない）
