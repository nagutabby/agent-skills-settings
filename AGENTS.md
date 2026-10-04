# CLI authentication and AWS infrastructure

## User-run authentication and authorization

- When a CLI operation requires authentication or authorization, show the user the specific command(s) needed and ask them to run those commands themselves. Do not run login, sign-in, or authorization-grant commands on the user's behalf.
- After the user reports the result, continue the task using that result and verify the active identity when the CLI provides a suitable identity-check command.

## Local CLI authentication

- AWS CLI: Use the configured IAM Identity Center profile `sso-admin-profile`. Sign in with `aws sso login --profile sso-admin-profile` and verify with `aws sts get-caller-identity --profile sso-admin-profile`. Pass the profile explicitly with `--profile` or `AWS_PROFILE`.
- Wrangler: Use OAuth with `wrangler login --use-keyring`. Check the active identity with `wrangler whoami`.
- Cloudflare `cf` CLI: Use OAuth with `cf auth login`. Check the active identity with `cf auth whoami`. Use a named profile with `cf auth create <profile>` and pass it with `--profile <profile>`.
- GitHub CLI: Use browser OAuth with `gh auth login --hostname github.com`. Check authentication with `gh auth status --active --hostname github.com`.
- Do not use API keys or personal access tokens for local CLI authentication.

## CI/CD authentication

- Prefer OIDC federation for CI/CD access to AWS and other supported cloud providers. Do not use long-lived AWS access keys.
- Use API tokens only in CI/CD when the service or CLI requires them. Give each token the minimum required permissions and store it in the CI/CD secret store.
- For `gh` in GitHub Actions, use the built-in `${{ github.token }}` through `GH_TOKEN`; use a personal access token only when the built-in token cannot provide the required access.
- Never put token or key values in this file, source code, command output, or logs.

## AWS application infrastructure

- Use AWS CDK to build or change application infrastructure on AWS. Review `cdk synth` and `cdk diff` before `cdk deploy`.

## PlatformIO for embedded systems

- For PlatformIO work on embedded systems, use PlatformIO Core's `pio` CLI for builds, tests, device uploads, and serial monitoring.
