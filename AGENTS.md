# CLI 認証と AWS インフラ

## ユーザーによる認証・認可の実行

- CLI 操作で認証または認可が必要な場合は、必要な具体的コマンドをユーザーに提示し、ユーザー自身に実行してもらう。ログイン、サインイン、認可付与のコマンドをユーザーの代わりに実行しない。
- ユーザーから結果の報告を受けた後、その結果を使ってタスクを続行する。CLI に適した ID 確認コマンドがある場合は、有効な ID を確認する。

## ローカル CLI の認証

- AWS CLI: 設定済みの IAM Identity Center プロファイル `sso-admin-profile` を使う。`aws sso login --profile sso-admin-profile` でサインインし、`aws sts get-caller-identity --profile sso-admin-profile` で確認する。`--profile` または `AWS_PROFILE` でプロファイルを明示的に指定する。
- Wrangler: `wrangler login --use-keyring` による OAuth を使う。有効な ID は `wrangler whoami` で確認する。
- Cloudflare `cf` CLI: `cf auth login` による OAuth を使う。有効な ID は `cf auth whoami` で確認する。名前付きプロファイルは `cf auth create <profile>` で作成し、`--profile <profile>` で指定する。
- GitHub CLI: `gh auth login --hostname github.com` によるブラウザ OAuth を使う。認証状態は `gh auth status --active --hostname github.com` で確認する。
- ローカル CLI の認証に API キーや Personal Access Token を使わない。

## CI/CD の認証

- CI/CD から AWS やその他の対応クラウドプロバイダーへアクセスする場合は、OIDC フェデレーションを優先する。長期有効な AWS アクセスキーを使わない。
- API トークンは、サービスまたは CLI が要求する場合に限り CI/CD でのみ使う。各トークンには必要最小限の権限を付与し、CI/CD のシークレットストアに保管する。
- GitHub Actions の `gh` では、`GH_TOKEN` 経由で組み込みの `${{ github.token }}` を使う。組み込みトークンで必要なアクセスを得られない場合に限り、Personal Access Token を使う。
- トークンやキーの値を、このファイル、ソースコード、コマンド出力、ログに記載しない。

## AWS アプリケーションインフラ

- AWS 上のアプリケーションインフラの構築・変更には AWS CDK を使う。`cdk deploy` の前に `cdk synth` と `cdk diff` を確認する。

## 組み込みシステム向け PlatformIO

- 組み込みシステムの PlatformIO 作業では、ビルド、テスト、デバイスへの書き込み、シリアルモニタリングに PlatformIO Core の `pio` CLI を使う。
