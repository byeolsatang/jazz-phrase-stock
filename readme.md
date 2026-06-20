# 🎹 JazzPhrase.stock

ジャズフレーズをストックして、楽譜を見ながら練習できる個人用Webアプリ。

Flat.ioに保存した楽譜をAPIで取得し、スケール・奏法タグやコード進行で管理・検索できます。

---

## 🌐 デモ

https://jazz-phrase-stock.vercel.app

---

## 機能

- **Flat.io連携** — Jazz Phraseコレクションのスコアを自動取得・埋め込み表示・再生
- **フレーズ管理** — コード進行 / キー / 難易度 / タグで分類
- **タグシステム** — スケール・アプローチ・リズム・出典の4カテゴリ＋自由入力
- **絞り込み** — サイドバーでコード進行・キー・難易度・タグをフィルタリング
- **検索** — フレーズ名・メモ・タグをインクリメンタルサーチ
- **お気に入り★ / 練習済み✓** — 学習進捗を管理
- **Supabase同期** — PC・スマホどちらからでも同じデータを参照
- **レスポンシブ対応** — スマホはボトムナビ＋ボトムシートフィルター

---

## 技術構成

| 項目 | 内容 |
|------|------|
| フロントエンド | HTML / CSS / Vanilla JS（単一ファイル） |
| 楽譜表示 | Flat.io Embed SDK |
| スコア取得 | Flat.io REST API |
| データ永続化 | Supabase（PostgreSQL） |
| ホスティング | Vercel |

---

## セットアップ

### 1. Flat.io

1. [Flat Developer](https://flat.io/developers) でアプリを作成
2. REST API → Personal tokens でトークンを発行
   - スコープ: `scores.readonly` + `collections.readonly`
3. Flat.io上に `Jazz Phrase` コレクションを作成し、楽譜を追加（公開設定にする）

### 2. Supabase

1. [Supabase](https://supabase.com) でプロジェクトを作成
2. SQL Editorで以下を実行：

```sql
create table phrases (
  id bigint generated always as identity primary key,
  name text not null,
  prog text default 'other',
  key text default 'C',
  level text default 'intermediate',
  score_id text default '',
  fav boolean default false,
  done boolean default false,
  tags jsonb default '[]',
  notes text default '',
  created_at timestamptz default now()
);

alter table phrases enable row level security;

create policy "全員読み書き可" on phrases
  for all using (true) with check (true);
```

3. Project URL と anon キーをメモ

### 3. アプリ設定

1. `jazz-phrase-stock.vercel.app` を開く
2. ⚙ ボタン → 設定モーダル
3. Flat.io Personal Access Token を入力
4. Supabase Project URL と Anon Key を入力
5. 保存 → 「↻ Jazz Phraseを同期」でスコアを取得

---

## タグプリセット

### スケール
`Bebop scale` `Bebop 下降形` `Bebop 上昇形` `Harmonic minor` `Harmonic minor 下降形` `Melodic minor 上昇形` `Melodic minor 下降形` `Dorian` `Phrygian dominant` `Altered scale` `Lydian dominant` `Whole tone` `Diminished scale` `Major pentatonic` `Minor pentatonic` `Blues scale` など

### アプローチ
`半音アプローチ` `クロマチック` `エンクロージャー` `コードトーン` `ガイドトーン` `トライトーン代理` `アウトサイド` `ブルース進行` `リズムチェンジ` など

### リズム
`シンコペ` `トリプレット` `8分音符主体` `16分音符` `ターン` `ロングトーン` `ラン`

### 出典
`Charlie Parker` `John Coltrane` `Bill Evans` `Herbie Hancock` `McCoy Tyner` `Oscar Peterson` `Wynton Kelly` `Red Garland` `Bud Powell` `自作` `セッション採譜`

---

## デプロイ

```bash
git add .
git commit -m "update"
git push
```

GitHubへのpushでVercelが自動デプロイします。
