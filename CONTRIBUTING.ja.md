# Contributing Guide

## 開発フロー
1. リポジトリをフォーク
2. フィーチャーブランチを作成 (`git checkout -b feature/amazing-feature`)
3. 変更をコミット (`git commit -m 'feat: Add amazing feature'`)
4. ブランチにプッシュ (`git push origin feature/amazing-feature`)
5. Pull Request を作成

## コミットメッセージ規約
[Conventional Commits](https://www.conventionalcommits.org/) に従います。

| Prefix | 用途 |
|--------|------|
| `feat:` | 新機能 |
| `fix:` | バグ修正 |
| `docs:` | ドキュメント変更 |
| `style:` | コードスタイル変更 |
| `refactor:` | リファクタリング |
| `test:` | テスト追加・修正 |
| `chore:` | その他の変更 |

## コードスタイル
- PHPCS (WordPress Coding Standards) に従ってください。
- PR作成前に `npm run lint` と `npm run format` を実行してください。

## 質問・サポート
Issue を作成するか、Discussions でお問い合わせください。
