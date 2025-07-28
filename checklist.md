✅ GitHub Apps通知システム構築チェックリスト
🛠️ Phase 1: GitHub App作成・設定
1.1 GitHub App作成
[ ] GitHub設定 → Developer settings → GitHub Apps → New GitHub App
[ ] App名: temp-repo-sync (任意の名前)
[ ] Homepage URL: https://github.com/kei-creative-life
[ ] Webhook URL: https://example.com/webhook (ダミーでOK)
[ ] Webhook secret: 空欄でOK
1.2 権限設定 (Repository permissions)
[ ] Contents: Write
[ ] Pull requests: Write
[ ] Actions: Write
[ ] Metadata: Read
1.3 イベント設定
[ ] Subscribe to events: 特に設定不要（workflow内で指定）
1.4 インストール設定
[ ] Where can this GitHub App be installed?: Only on this account
[ ] GitHub App作成完了
1.5 認証情報取得
[ ] App IDをメモ
[ ] Generate a private keyで秘密鍵をダウンロード
[ ] 秘密鍵の内容をコピー（-----BEGIN RSA PRIVATE KEY-----から-----END RSA PRIVATE KEY-----まで全て）

🔧 Phase 2: リポジトリ設定
2.1 GitHub Appインストール
[ ] GitHub App設定ページ → Install App
[ ] temp-repoAにインストール
[ ] temp-repoBにインストール
2.2 temp-repoA Secrets設定
[ ] temp-repoA → Settings → Secrets and variables → Actions
[ ] Repository secretsに以下を追加：
[ ] APP_ID: GitHub AppのID
[ ] APP_PRIVATE_KEY: 秘密鍵の内容（改行含む全文）
2.3 temp-repoB Secrets設定
[ ] temp-repoB → Settings → Secrets and variables → Actions
[ ] Repository secretsに以下を追加：
[ ] APP_ID: GitHub AppのID
[ ] APP_PRIVATE_KEY: 秘密鍵の内容（改行含む全文）
2.4 temp-repoB GitHub Actions設定
[ ] temp-repoB → Settings → Actions → General
[ ] Workflow permissions:
[ ] ☑️ Read and write permissions
[ ] ☑️ Allow GitHub Actions to create and approve pull requests