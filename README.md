# 出品AIアシスタント（Android PWA）

## スマホだけで公開する
1. GitHubで新しい公開リポジトリ（例: `listing-ai`）を作る。
2. このZIPをスマホで展開し、`index.html`、`app.js`、`sw.js`、`manifest.webmanifest`、`icon.svg` をリポジトリの一番上にアップロードする（`worker.js` は公開してもAPIキーは含まれないが、GitHub Pagesで動かす必要はない）。
3. GitHub → リポジトリ → Settings → Pages → Deploy from a branch → `main` / `(root)` → Save。
4. 数分後に `https://ユーザー名.github.io/listing-ai/` にアクセス。Android Chromeのメニューから「ホーム画面に追加」または「アプリをインストール」。

## AIを使うための追加設定（任意）
**OpenAI APIはChatGPT Plusとは別料金です。**
1. OpenAI PlatformでAPIキーを発行。利用料金・予算上限を確認。
2. Cloudflareに登録し、Workers & Pages → Create → Worker を作成。
3. Workerのコードを `worker.js` の内容に置き換えてデプロイ。
4. WorkerのSettings → Variables and Secretsで `OPENAI_API_KEY` を **Secret** として登録。`ALLOWED_ORIGIN` に `https://ユーザー名.github.io` を設定（末尾の `/` は付けない）。
5. PWAの「設定」にWorker URL（例 `https://listing-ai.アカウント.workers.dev`）を貼って保存。
6. 写真を選んで「AIで出品文を作成」。

## 注意点
- メルカリ相場の**自動取得機能はありません**。検索リンクからメルカリで売却済み商品の価格を調べ、手入力した実例の中央値から出品価格を提案します。
- 画像のAI解析は、Worker経由でOpenAIへ送信されます。個人情報が写り込まないようにしてください。
- AIの推定価格はリアルタイム相場ではありません。ブランドや真贋をAIが確定できるわけではありません。
- 保存履歴はスマホのブラウザ内（localStorage）に保存。端末変更・ブラウザデータ削除で消えます。
- Worker URLは公開可能なURLです。CORSはブラウザからの利用制限であり、**第三者による直接API呼び出しや料金の不正利用を防ぐ認証ではありません**。公開運用前にCloudflare Access、Turnstile、認証・レート制限などの保護を追加してください。
