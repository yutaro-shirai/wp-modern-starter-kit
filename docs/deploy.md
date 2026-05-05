# Deploy 手順

このスターターキットは GitHub Actions による **自動デプロイ** + **配布 zip** の 2 経路を提供します。

## 経路 A: 配布 ZIP (常時利用可能)

GitHub Release を発行すると `release.yml` が自動で zip を作成し Releases ページにアップロードします。

```bash
# ローカルで version bump → tag → push
npm version patch        # 1.0.0 → 1.0.1 (CHANGELOG 等に応じて minor / major も)
git push --follow-tags   # tag が GitHub に届くと release.yml が走る
```

→ GitHub Releases ページに `<theme-name>.zip` が公開されます (zip の名前は `package.json` の `scripts.zip` で決まる)。

WP 管理画面 → 外観 → テーマ → 新規追加 → アップロード で導入できます。

## 経路 B: 自動デプロイ (`deploy.yml`)

Release published or 手動実行 (`workflow_dispatch`) で SSH/SFTP 経由でサーバへ展開します。

> ⚠️ **デフォルトでは無効**。`DEPLOY_ENABLED=true` を Variables に設定するまで no-op で動作します。

### セットアップ手順 (初回のみ)

#### 1. Secrets を設定 (Settings > Secrets and variables > Actions > New repository secret)

| Name | Value 例 | 説明 |
|---|---|---|
| `DEPLOY_HOST` | `ssh.example.com` | デプロイ先ホスト名 |
| `DEPLOY_USER` | `wp-deploy` | SSH ユーザー名 |
| `DEPLOY_SSH_KEY` | `-----BEGIN OPENSSH PRIVATE KEY-----...` | SSH 秘密鍵全文 (改行含む) |
| `DEPLOY_KNOWN_HOSTS` | `ssh-keyscan -H ssh.example.com` の出力 | **必須**・MITM 対策のホスト鍵 |
| `DEPLOY_PATH` | `/home/wp-deploy/public_html/wp-content/themes` | テーマディレクトリの親パス |
| `DEPLOY_THEME_SLUG` | `wp-modern-starter-kit` | 解凍後のディレクトリ名 |

##### SSH 鍵生成例

```bash
# ローカルで専用デプロイ鍵を生成 (本番管理者鍵を流用しない)
ssh-keygen -t ed25519 -C "github-actions deploy" -f ~/.ssh/wp-deploy

# 公開鍵をサーバの ~/.ssh/authorized_keys に追加
cat ~/.ssh/wp-deploy.pub  # → サーバへ手動コピー

# 秘密鍵を GitHub Secrets DEPLOY_SSH_KEY に登録
cat ~/.ssh/wp-deploy

# known_hosts を取得
ssh-keyscan -H ssh.example.com  # → DEPLOY_KNOWN_HOSTS に登録
```

#### 2. Variables を設定 (Settings > Secrets and variables > Actions > Variables)

| Name | Value | 説明 |
|---|---|---|
| `DEPLOY_ENABLED` | `true` | このリポジトリで deploy.yml を有効化 |
| `DEPLOY_BACKUP` | `true` (推奨) | 旧テーマを `themes-backup/` に退避 |

#### 3. Environment を設定 (Settings > Environments)

`production` / `staging` の 2 つの environment を作成:
- **production**: Required reviewers に管理者を追加 (本番デプロイは事前承認必要)
- **staging**: Reviewer 不要 (検証用は自由に)

各 environment にも secrets を上書き可能 (env 別の host 等)。

### 利用方法

#### 自動デプロイ (Release 発行時)
```bash
git tag v1.1.0 && git push --follow-tags
# → release.yml で zip 公開 → deploy.yml が production 環境で起動
# → reviewer 承認後にサーバへ展開
```

#### 手動デプロイ (任意のタイミング)
1. GitHub Actions タブ → Deploy workflow → Run workflow
2. target: `staging` or `production` を選択 → Run
3. 進捗を確認

### デプロイの動作

1. `npm run zip` で `dist/<theme>.zip` 生成
2. SCP でサーバの `${DEPLOY_PATH}` に転送
3. `DEPLOY_BACKUP=true` の場合、既存テーマを `../themes-backup/${SLUG}-{timestamp}.tgz` にバックアップ
4. zip を `${SLUG}-incoming/` に展開 (atomic な切り替え準備)
5. 既存 `${SLUG}/` を `${SLUG}.old/` にリネーム → `${SLUG}-incoming/` を `${SLUG}/` にリネーム
6. `${SLUG}.old/` と zip を削除
7. `style.css` 先頭 12 行を表示して deploy 成功を verify

### ロールバック

```bash
ssh wp-deploy@ssh.example.com
cd /path/to/wp-content/themes-backup
ls -lt | head    # 最新バックアップ確認
tar xzf <theme>-20260512-143022.tgz -C ../themes/
```

## アクセシビリティ CI (`a11y.yml`)

PR ごとに pa11y + Lighthouse を実行します。WCAG 2.2 Level AA 準拠を担保する雛形です。

実際にチェックを走らせるには:

1. リポジトリルートに `.pa11yci.json` を作成 (チェック対象 URL リスト)
2. `lighthouserc.json` または `lighthouserc.js` を作成
3. wp-env / docker compose で WordPress を CI 環境に立ち上げる構成を追加

## トラブルシューティング

| 症状 | 対応 |
|---|---|
| `DEPLOY_ENABLED` warning | Variables で `DEPLOY_ENABLED=true` を設定 |
| `Host key verification failed` | `DEPLOY_KNOWN_HOSTS` を再取得して上書き |
| `Permission denied (publickey)` | 公開鍵がサーバの `~/.ssh/authorized_keys` に正しく追加されているか |
| 展開後にテーマが認識されない | `style.css` のヘッダ確認・パーミッション (644) 確認 |

## 派生プロジェクト用 (fork する場合)

このスターターキットを fork してテーマを作る場合、以下を書き換えてください:

- `package.json` の `name` / `description` / zip 名 (`scripts.zip` の出力名)
- `style.css` のテーマヘッダ (Theme Name / Author / License 等)
- 各 PHP ファイルの `text-domain` (例: `'wp-modern-starter-kit'` → `'your-theme'`)
- 各 PHP ファイルの `@package` コメント (例: `WP_Modern_Starter_Kit` → `Your_Theme`)
- `.github/workflows/release.yml` の `files: dist/<theme-name>.zip`
