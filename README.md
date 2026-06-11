# Modern WordPress Starter Kit

[![CI](https://github.com/i-Willink-Inc/wp-modern-starter-kit/actions/workflows/main.yml/badge.svg)](https://github.com/i-Willink-Inc/wp-modern-starter-kit/actions/workflows/main.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

PHPベースのWordPressテーマ開発を爆速化するスターターキットです。
Tailwind CSS によるモダンなスタイリング、事前構築済みの UI コンポーネント群、そして GitHub Actions による自動ビルド・デプロイフローを備え、受託開発における立ち上げから納品までのリードタイムを劇的に短縮します。

---

## 📋 目次

- [特徴](#-特徴)
- [プロジェクト構成](#-プロジェクト構成)
- [クイックスタート](#-クイックスタート)
- [開発ガイド](#-開発ガイド)
- [i-Willink DS primitives](#-i-willink-ds-primitives)
- [ビルドと納品](#-ビルドと納品)
- [利用可能なコマンド](#-利用可能なコマンド)
- [ドキュメント](#-ドキュメント)
- [コントリビューション](#-コントリビューション)
- [ライセンス](#-ライセンス)

---

## ✨ 特徴

| 技術 | 説明 |
|------|------|
| 🎨 **Tailwind CSS** | ユーティリティファーストなスタイリングと `template-parts` によるコンポーネント化 |
| 🧩 **Pre-built UI** | Hero, Cards, CTA など、即戦力の UI コンポーネントを多数同梱 |
| 🐘 **Pure PHP** | フロントエンドフレームワークに依存せず、PHP だけで完結する堅牢な構成 |
| 📦 **Auto Release** | GitHub Actions で納品用 ZIP ファイルを自動生成 |
| ⚡ **Modern DX** | `wp-env` によるゼロコンフィグなローカル開発環境とホットリロード |
| 🧹 **Code Quality** | PHPCS (WordPress Coding Standards) + Prettier による品質担保 |
| 🐳 **Devcontainer** | 統一された開発環境で、環境構築の手間を排除 |

---

## 📁 プロジェクト構成

```
wp-modern-starter-kit/
├── src/                     # 開発用ソースコード (ビルド対象)
│   ├── css/                 # Tailwind CSS / SCSS
│   └── js/                  # TypeScript / JavaScript
├── parts/                   # 再利用可能なテンプレートパーツ
│   ├── atoms/               # ★ Atomic Design: 最小単位コンポーネント
│   ├── molecules/           # ★ Atomic Design: 複合コンポーネント
│   ├── organisms/           # ★ Atomic Design: 大規模コンポーネント
│   ├── common/              # ヘッダー、フッター等
│   └── layouts/             # レイアウト定義
├── inc/                     # PHPロジック (functions.phpから分割)
├── templates/               # 個別ページテンプレート
├── docs/                    # ドキュメント
│   └── architecture.md      # アーキテクチャ設計書
├── functions.php            # テーマのエントリーポイント
├── style.css                # テーマ定義ファイル
└── .github/workflows/       # CI/CD (Lint, Release作成)
```

---

## 🚀 クイックスタート

### 前提条件

| ツール | 最小バージョン | 推奨 |
|--------|--------------|------|
| **Node.js** | 18.17.0 | 20.x LTS |
| **Docker** | - | 最新版（wp-env / Devcontainer 利用時に必須） |

### 1. リポジトリのクローン

```bash
git clone https://github.com/i-Willink-Inc/wp-modern-starter-kit.git my-theme
cd my-theme
```

### 2. 依存関係のインストール

```bash
npm install
```

### 3. ローカル環境の起動 (wp-env)

Docker を使用してローカルに WordPress 環境を立ち上げます。
**Docker Desktop** もしくは **OrbStack** がインストールされ、起動している必要があります。

```bash
npm run env:start
```

> [!NOTE]
> 初回起動時はWordPress本体やデータベースイメージのダウンロードが行われるため、数分かかる場合があります。

*   **サイト URL**: http://localhost:8888
*   **管理画面**: http://localhost:8888/wp-admin
    *   User: `admin`
    *   Password: `password`

### 4. 開発サーバーの起動

ファイルの変更を監視し、自動ビルドとホットリロードを行います。

```bash
npm run dev
```

### Devcontainer を使用する場合（推奨）

1. Docker Desktop または Rancher Desktop を起動
2. VS Code でプロジェクトを開く
3. コマンドパレット (Ctrl+Shift+P) → **「Dev Containers: Reopen in Container」**

> [!TIP]
> Devcontainer を使用すると、Node.js や必要なツールが全てコンテナ内にセットアップされるため、ローカル環境を汚さずに開発を始められます。

---

## 🛠 開発ガイド

### コンポーネントの利用

このキットには `parts/` 配下に Atomic Design に基づいた UI コンポーネントが含まれています。
PHP の `get_template_part` 関数を使用して、引数を渡しながら簡単に呼び出すことができます。

**使用例: ヒーローセクションの表示**

```php
<?php
get_template_part('parts/organisms/hero-simple', null, [
    'title'       => '株式会社サンプル',
    'description' => '最新の技術でビジネスを加速させます。',
    'button_text' => 'お問い合わせ',
    'button_url'  => '/contact',
    'image_url'   => 'https://example.com/hero.jpg'
]);
?>
```

**使用例: ボタン（Atom）の表示**

```php
<?php
get_template_part('parts/atoms/button', null, [
    'text'    => '詳細を見る',
    'variant' => 'primary',
    'size'    => 'lg'
]);
?>
```

**使用例: パンくずリスト（Molecule）の表示**

```php
<?php
get_template_part('parts/molecules/breadcrumbs', null, [
    'items' => [
        ['label' => 'ホーム', 'url' => '/'],
        ['label' => 'サービス', 'url' => '/services'],
        ['label' => '詳細']  // 最後の項目はリンクなし
    ]
]);
?>
```

### スタイリング

`src/css/style.css` を編集します。Tailwind CSS のユーティリティクラスを使用するか、必要に応じて `@apply` ディレクティブを使用してカスタムクラスを定義してください。

### コンポーネント一覧の確認

ローカル環境起動後、コンポーネントライブラリページで全コンポーネントのプレビューと使用方法を確認できます。

*   **コンポーネントライブラリ**: http://localhost:8888/?page_id=5

---

## 🎨 i-Willink DS primitives

このキットは [i-Willink Design System](https://github.com/willink-oss/willink-design-system) の **primitives（プリミティブトークン）のみ** を [`@willink-labs/css-tokens`](https://github.com/willink-oss/willink-design-system/tree/main/packages/css-tokens)（public npm / 認証不要）経由で取り込んでいます。

### 何を import しているか

`src/css/style.css` の先頭（brand / カスタムレイヤーより**前**）で primitives-only ファイルを import しています。

```css
@import "@willink-labs/css-tokens/src/tokens.scale.css";
```

`npm run build`（tailwindcss v3 CLI）が node_modules から解決し、`dist/style.css` に**インライン展開**されます。WordPress 側に追加の enqueue は不要です。

> [!NOTE]
> 正規の specifier は `@willink-labs/css-tokens/tokens.scale.css`（package.json `#exports` の alias）ですが、tailwindcss v3 CLI に同梱の postcss-import は `#exports` map を解決できないため、実体パス `src/tokens.scale.css` を import しています（同一ファイル）。

import されるのは `:root` 上の CSS 変数（プリミティブのみ）です。

| カテゴリ | 変数例 |
|---------|--------|
| Color scale | `--color-neutral-50..950`, `--color-blue-*` など数値スケール |
| Radius | `--radius-sm` `--radius-md` `--radius-lg` `--radius-xl` `--radius-full` |
| Duration | `--duration-fast` `--duration-base` `--duration-slow` |
| Easing | `--ease-standard` `--ease-emphasized` |
| Shadow | `--shadow-soft` `--shadow-md` `--shadow-glow` |

### Primitives-only 契約（brand は import しない）

DS の **semantic / brand ロール（`tokens.semantic.css` 等）は意図的に import していません**。ブランドカラーはテーマ（このキットおよび各クライアントテーマ）側が自前で定義します。DS から受け取るのは radius / motion / shadow / 数値カラースケールといった土台のみです。

### 上書き方法

CSS 変数のカスケードで上書きできます。import より**後**に `:root` で再宣言してください。

```css
@import "@willink-labs/css-tokens/src/tokens.scale.css";

:root {
  --radius-md: 0.375rem; /* テーマ都合で DS 値を上書き */
}
```

### ドキュメント

- リポジトリ: https://github.com/willink-oss/willink-design-system
- パッケージ docs: [`packages/css-tokens/README.md`](https://github.com/willink-oss/willink-design-system/tree/main/packages/css-tokens)
- ADR: [ADR-0009 css-tokens package](https://github.com/willink-oss/willink-design-system/blob/main/docs/adr/0009-css-tokens-package.md)

---

## 📦 ビルドと納品

### 本番用ビルド

CSS/JS の最適化（Minify）を行います。

```bash
npm run build
```

### テーマファイルのパッケージング (手動)

納品用に不要なファイル（node_modules, src 等）を除外し、テーマ一式を ZIP ファイルに圧縮します。

```bash
npm run zip
```

`dist/wp-modern-starter-kit.zip` が生成されます。

### 自動リリース (GitHub Actions)

Git タグをプッシュすると、自動的にビルドと ZIP 作成が行われ、GitHub Releases にアップロードされます。

```bash
git tag v1.0.0
git push origin v1.0.0
```

---

## 📋 利用可能なコマンド

| コマンド | 説明 |
|---------|------|
| `npm run dev` | 開発モード起動 (Watch + Hot Reload) |
| `npm run build` | プロダクションビルド |
| `npm run env:start` | ローカル WordPress 起動 |
| `npm run env:stop` | ローカル WordPress 停止 |
| `npm run lint` | コード品質チェック (PHP/JS/CSS) |
| `npm run format` | コード自動フォーマット |
| `npm run zip` | 納品用 ZIP ファイル生成 |

---

## 📚 ドキュメント

詳細なドキュメントは `docs/` ディレクトリを参照してください。

| ドキュメント | 説明 |
|-------------|------|
| [architecture.md](docs/architecture.md) | Atomic Design に基づくアーキテクチャ設計 |

---

## 🤝 コントリビューション

コントリビューションを歓迎します！詳細は `CONTRIBUTING.md` をご覧ください。

---

## 📄 ライセンス

このプロジェクトは MIT License の下で公開されています。
