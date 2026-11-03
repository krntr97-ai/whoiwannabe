# なりたい自分メーター（公開用）

ログイン（Supabase Auth）とクラウド保存（Supabase）つきの、1ファイル構成のWebアプリです。

- `index.html` … アプリ本体（これだけ置けば動く）
- 上のフォルダの `supabase-schema.sql` … Supabase側の設定（実行済みならOK）

## 1. Vercelに置く

1. GitHubに新しいリポジトリを作り、`index.html` を入れる（ルート直下）
2. vercel.com → Add New → Project → そのリポジトリを選ぶ
3. Framework Preset は「Other」、Build Command と Output Directory は空のまま → Deploy
4. 公開URL（例：https://xxxx.vercel.app）が決まる

## 2. SupabaseにURLを教える（これを忘れると、メールのリンクが動かない）

Supabase → Authentication → URL Configuration

- Site URL：公開URL（例：https://xxxx.vercel.app）
- Redirect URLs：同じ公開URLを追加する

## 3. 動作テスト

1. 公開URLを開く → 新規登録 → 確認メールのリンクを開く → ログインできる
2. 目標を入力 → 「保存しました」と出る
3. 別のブラウザ（またはスマホ）で同じアカウントにログイン → 同じデータが見える
4. 別のアカウントを作り、最初のアカウントのデータが見えないことを確認

## メモ

- キーについて：`index.html` に入っている `sb_publishable_...` は、公開されても安全な鍵です。
  守りは、SQLで設定した「自分の行だけ見える」ルール（RLS）が担当します。
- `sb_secret_...` と `service_role` は、絶対に書き込まないでください。
- 無料プランでは、しばらく使われないとプロジェクトが一時停止することがあります。
- 同じアカウントを2つの端末で同時に編集すると、あとから保存した内容が勝ちます。
