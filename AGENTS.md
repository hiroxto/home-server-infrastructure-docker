# Repository Guidelines

## プロジェクト構成

このリポジトリは自宅サーバーで使用する Docker イメージを管理します。
`src/` 配下にイメージ定義があります。
アプリケーションコード、アセット、独立した単体テストは含まれていない。

## 関連リポジトリ

- [hiroxto/infrastructure](https://github.com/hiroxto/infrastructure): 自宅サーバー、ドメイン、Cloudflare などを Terraform で管理する。
- [hiroxto/home-server-infrastructure](https://github.com/hiroxto/home-server-infrastructure): 自宅サーバーの構成やサービスを管理する。当プロジェクトでビルドした Docker イメージはこのリポジトリで使用される。

変更の責務が Docker イメージの作成以外にある場合は、これらのリポジトリも確認する。

## ビルド・検証コマンド

ローカル作業には Docker と [Task](https://taskfile.dev/) が必要となる。

- `task` または `task help`: 利用可能なタスクを一覧表示する。
- `task build`: すべての Docker イメージをビルドする。
- `task build:epgstation`: EPGStation イメージのみビルドする。
- `task build:mirakurun`: Mirakurun イメージのみビルドする。

プルリクエストでは `.github/workflows/test.yml` が変更されたイメージのみビルドする。`main` への push では対象イメージを GHCR に公開する。

## テスト方針

GitHub Actions の CI で変更したイメージが正常にビルドできることを必須の検証とする。
プルリクエスト作成前に、対応する `task build:<image>` を実行する。
共通ワークフローを変更した場合は、パスフィルターにより CI が実行されない可能性があるため、両イメージへの影響を確認する。実行したコマンドと確認結果は PR の「動作確認」に記載する。

## コミットとプルリクエスト

- 1 つのコミットでは 1 つの意図に絞る
- コミットメッセージのプレフィックスには `feat:`，`fix:`，`delete:`，`docs:`，`ci:` のような短い Conventional Commits 形式を使用する。
- コミットメッセージは何を変更するかを日本語で簡潔に書く
- コミット前に差分を確認し，実際の変更内容に沿ったメッセージにする。
- PR 作成時は `.github/pull_request_template.md` の構成に従い，テンプレートの見出しを維持して本文を埋める。

## セキュリティと設定

GitHub からの公開には、必要最小限の `packages: write` 権限を持つ `GITHUB_TOKEN` を使用する。
ベースイメージや取得元のバージョン変更を確認し、Renovate で管理可能な依存関係はバージョンを固定する。
