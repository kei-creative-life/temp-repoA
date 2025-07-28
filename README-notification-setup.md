# temp-repoA → temp-repoB 変更通知システム

このシステムは、temp-repoAの`docs`フォルダに変更があったときに、自動的にtemp-repoBに通知を送信します。

## 仕組み

1. **temp-repoA**: `docs/`フォルダの変更がmainブランチにマージされると通知を送信
2. **temp-repoB**: 通知を受信してIssueを作成し、必要に応じてSlack通知も送信

## セットアップ方法

### 方法1: Personal Access Token（推奨・簡単）

#### 1. Personal Access Tokenの作成
1. GitHubの設定 → Developer settings → Personal access tokens → Tokens (classic)
2. 新しいトークンを生成（スコープ: `repo`, `workflow`）
3. トークンをコピー

#### 2. Secretsの設定
**temp-repoA**のリポジトリ設定で以下のSecretを追加：
- `PAT_TOKEN`: 作成したPersonal Access Token

#### 3. ワークフローの有効化
- `notify-to-b-pat.yml`を使用（GitHub App版より簡単）

### 方法2: GitHub App（高度・セキュア）

#### 1. GitHub Appの作成
1. GitHub Organization/個人設定 → Developer settings → GitHub Apps
2. 新しいGitHub Appを作成
3. 必要な権限設定:
   - Repository permissions:
     - Contents: Read
     - Issues: Write
     - Pull requests: Read
     - Metadata: Read

#### 2. GitHub Appの設定
1. Private keyを生成・ダウンロード
2. App IDを確認
3. 両方のリポジトリにAppをインストール

#### 3. Secretsの設定
**temp-repoA**のリポジトリ設定で以下のSecretを追加：
- `APP_ID`: GitHub AppのID
- `APP_PRIVATE_KEY`: ダウンロードした秘密鍵の内容

#### 4. ワークフローの有効化
- `notify-to-b.yml`を使用

## オプション設定

### Slack通知の有効化（temp-repoB）
1. SlackでIncoming Webhookを設定
2. temp-repoBのリポジトリ変数に`SLACK_WEBHOOK_URL`を追加

## ファイル構成

```
temp-repoA/
└── .github/
    └── workflows/
        ├── notify-to-b.yml         # GitHub App版
        └── notify-to-b-pat.yml     # PAT版（推奨）

temp-repoB/
└── .github/
    └── workflows/
        └── handle-docs-update.yml  # 通知受信用
```

## 動作テスト

1. temp-repoAの`docs/`フォルダにファイルを追加/変更
2. プルリクエストを作成してmainブランチにマージ
3. temp-repoBでIssueが自動作成されることを確認

## トラブルシューティング

### ワークフローが実行されない場合
- リポジトリのActions設定が有効になっているか確認
- Secretsが正しく設定されているか確認
- ワークフローファイルの構文エラーがないか確認

### 通知が届かない場合
- temp-repoBのリポジトリでRepository dispatch eventsが有効か確認
- トークン/GitHub Appの権限が適切か確認
- ログでエラーメッセージを確認

## セキュリティ考慮事項

- **PAT版**: 個人アカウントのトークンを使用。簡単だが権限範囲が広い
- **GitHub App版**: 必要最小限の権限のみ。セットアップは複雑だがより安全

組織での使用にはGitHub App版を推奨します。 