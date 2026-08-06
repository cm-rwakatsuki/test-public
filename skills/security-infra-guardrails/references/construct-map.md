# コンストラクト対応表

security-infra リポジトリで管理しているセキュリティ検出サービスと、それを定義している AWS CDK コンストラクトの対応表。

## 検出・監査サービス

| サービス | コンストラクトのパス | `Security`<br>(ap-northeast-1) | `SecurityVirginia`<br>(us-east-1) |
|---|---|---|---|
| Amazon GuardDuty | `lib/constructs/guard-duty/index.ts` | あり | あり |
| AWS Security Hub | `lib/constructs/security-hub/index.ts` | あり（集約リージョン） | あり |
| Amazon Inspector | `lib/constructs/inspector/index.ts` | あり | あり |
| Amazon Detective | `lib/constructs/detective/index.ts` | あり | あり |
| AWS Config | `lib/constructs/aws-config/index.ts` | あり | あり |
| Amazon Macie | `lib/constructs/macie/index.ts` | あり | なし |
| VPC フローログ | `lib/constructs/vpc-flow-logs/index.ts` | あり | なし |

## ガードレール設定

| 設定 | コンストラクトのパス | `Security`<br>(ap-northeast-1) | `SecurityVirginia`<br>(us-east-1) |
|---|---|---|---|
| EBS 暗号化のデフォルト有効化 | `lib/constructs/ebs-encryption/index.ts` | あり | あり |
| VPC Block Public Access | `lib/constructs/vpc-public-block-access/index.ts` | あり | あり |
| S3 アカウントレベルのパブリックアクセスブロック | `lib/constructs/s3-account-public-access-block/index.ts` | あり | なし |
| IAM アカウントパスワードポリシー | `lib/constructs/account-password-policy/index.ts` | あり | なし |

## ログ保管先

| リソース | コンストラクトのパス | `Security`<br>(ap-northeast-1) | `SecurityVirginia`<br>(us-east-1) |
|---|---|---|---|
| ログ保管先バケット | `lib/constructs/log-destination-bucket/index.ts` | あり | あり |
| サーバーアクセスログ保管先バケット | `lib/constructs/server-access-logging-destination-bucket/index.ts` | あり | あり |
| CloudFormation テンプレート用バケット | `lib/constructs/cf-templates-bucket/index.ts` | あり | なし |

## 権限・連携

| リソース | コンストラクトのパス | `Security`<br>(ap-northeast-1) | `SecurityVirginia`<br>(us-east-1) |
|---|---|---|---|
| AWS DevOps Agent | `lib/constructs/devops-agent/index.ts` | あり | なし |
| CI/CD 用権限 | `lib/constructs/cicd-permission/index.ts` | あり | あり |
| GitHub Actions OIDC プロバイダー | `lib/constructs/github-actions-oidc-provider/index.ts` | あり | なし |
| IAM グループ | `lib/constructs/iam-group/index.ts` | あり | なし |
| API Gateway のログ出力権限 | `lib/constructs/api-gateway-logging-permission/index.ts` | あり | なし |

## 呼び出し元

| ファイル | 役割 |
|---|---|
| `bin/iac.ts` | スタックの定義とリージョンの割り当て |
| `bin/parameter.ts` | 環境ごとのパラメーター |
| `lib/security-stack.ts` | `ap-northeast-1` の `Security` スタック |
| `lib/security-sub-region-stack.ts` | `us-east-1` の `SecurityVirginia` スタック |
