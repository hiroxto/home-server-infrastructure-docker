# Repository Guidelines

## プロジェクト構成

このリポジトリは自宅サーバーで使用する Docker イメージを管理する。
`src/` 配下にイメージ定義がある。
アプリケーションコード、アセット、独立した単体テストは含まれていない。

## 関連リポジトリ

- [hiroxto/infrastructure](https://github.com/hiroxto/infrastructure): 自宅サーバー、ドメイン、Cloudflare などを Terraform で管理する。
- [hiroxto/home-server-infrastructure](https://github.com/hiroxto/home-server-infrastructure): 自宅サーバーの構成やサービスを管理する。当プロジェクトでビルドした Docker イメージはこのリポジトリで使用される。

変更対象が Docker イメージの作成以外にある場合は、これらのリポジトリも確認する。

## ビルド・検証コマンド

ローカル作業には Docker と [mise](https://mise.jdx.dev/) が必要となる。

- `mise tasks`: 利用可能なタスクを一覧表示する。
- `mise run build`: すべての Docker イメージをビルドする。
- `mise run build:epgstation`: EPGStation イメージのみビルドする。
- `mise run build:mirakurun`: Mirakurun イメージのみビルドする。

プルリクエストでは、`.github/workflows/test.yml` によって変更されたイメージのみがビルドされる。`main` への push では対象イメージを GHCR に公開する。

## テスト方針

変更したイメージが GitHub Actions の CI で正常にビルドできることを必須の確認項目とする。
プルリクエスト作成前に、対応する `mise run build:<image>` を実行する。
共通ワークフローを変更した場合は、パスフィルターにより CI が実行されない可能性があるため、両イメージへの影響を確認する。実行したコマンドと確認結果は PR の「動作確認」に記載する。

## セキュリティと設定

イメージの公開には、必要最小限の `packages: write` 権限を持つ `GITHUB_TOKEN` を使用する。
ベースイメージや取得元のバージョン変更を確認し、Renovate で管理可能な依存関係はバージョンを固定する。
