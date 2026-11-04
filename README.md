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

## 4. 招待制にする（クライアント限定で公開する場合）

アプリ側は、すでに招待制になっています（登録ボタンは出ません）。Supabase側で、次の2つをやります。

1. 新規登録を止める
   - Supabase → Authentication → Sign In / Providers（表記は変わることがあります）
   - 「Allow new users to sign up」をオフにして保存
2. 招待を送る
   - Supabase → Authentication → Users → 「Invite user」
   - クライアントのメールアドレスを入れて、送信

### 招待された人の流れ
1. 招待メールのリンクを開く（アプリが開く）
2. 「はじめに、パスワードを決めてください」の画面で、パスワードを決め、利用規約への同意にチェックして始める
3. 次回からは、メールとパスワードでログイン（忘れたら「パスワードを忘れた」から再設定）

### 注意
- 招待リンクには有効期限があります。切れたら、同じ画面から、もう一度招待を送ってください
- Site URL と Redirect URLs に、公開URLが入っていないと、招待のリンクが正しく開きません（手順2）
- 一般公開に切り替えるときは、index.html の `INVITE_ONLY` を `false` にし、Supabaseの「Allow new users to sign up」をオンにします
