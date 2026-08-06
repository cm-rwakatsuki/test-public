---
name: security-infra-guardrails
description: security-infra リポジトリで管理している AWS セキュリティ検出基盤（GuardDuty / Security Hub / Inspector / Macie / Detective / AWS Config / VPC フローログ）の構成と調査手順。セキュリティ検出サービスの有効状態や設定を確認したいとき、検出結果の調査でどのサービスを参照すべきか判断したいとき、該当設定を定義している AWS CDK コードの場所を特定したいときに使用する。
---

# security-infra ガードレール調査

security-infra リポジトリは、AWS アカウントのセキュリティ検出基盤を AWS CDK で管理している。セキュリティ検出サービスの設定に関する調査では、このスキルの手順に従うこと。

## 前提となる構成

- 検出サービスは 2 リージョンに展開している
  - `ap-northeast-1`: CloudFormation スタック `Security`。Security Hub の集約リージョン
  - `us-east-1`: CloudFormation スタック `SecurityVirginia`。集約先ではない
- 各サービスは `lib/constructs/` 配下のカスタムコンストラクトで有効化している
- Macie・VPC フローログ・IAM グループなど一部のリソースは `ap-northeast-1` にのみ存在する

サービスとコードの対応は `references/construct-map.md` を参照すること。

## ステップ 1: 対象リージョンとスタックを特定する

調査対象のリソースが属するリージョンを確認し、対応するスタックを決める。

- `ap-northeast-1` → `Security` スタック
- `us-east-1` → `SecurityVirginia` スタック

リージョンが特定できない場合は、Security Hub の集約リージョンである `ap-northeast-1` から調べる。

## ステップ 2: 検出サービスの有効状態を確認する

対象リージョンで、調査に関係する検出サービスが有効になっているかを確認する。両リージョンに存在するサービスは、片方だけ設定がずれていないかも確認する。

## ステップ 3: 設定を定義しているコードを特定する

`references/construct-map.md` の対応表から、該当サービスのコンストラクトのファイルパスを特定する。コンストラクトの実装と、それを呼び出しているスタック（`lib/security-stack.ts` または `lib/security-sub-region-stack.ts`）の両方を読むこと。

環境ごとに変わる値は `bin/parameter.ts` に定義されているため、設定値が想定と異なる場合はこのファイルも確認する。

## ステップ 4: 報告する

以下を含めて報告する。

1. 対象リージョンとスタック名
2. 検出サービスの有効状態と、確認した設定値
3. 設定を定義しているコードのファイルパス
4. 設定に問題がある場合は、修正すべきコードの箇所と修正方針

設定変更を提案する場合は、`ap-northeast-1` と `us-east-1` のどちらに影響するかを必ず明記すること。
