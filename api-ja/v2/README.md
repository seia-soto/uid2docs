# UID2 API v2 Documentation

UID2 の定義、形式、指針、構成要素、その他の概念的な詳細については、 [UID2 概要](../../README-ja.md) を参照してください。連絡先やライセンス情報、正規化およびハッシュエンコーディングの規則については、 [Unified ID 2.0 API Documentation](../README.md) を参照してください。

このページでは、UID2 API v2 を使い始めるために必要な以下の情報を提供します:

- [UID2 API v1 Compatibility and Upgrade Requirements（UID2 API v1 の互換性とアップグレードの要件）](#uid2-api-v1-compatibility-and-upgrade-requirements)
- [Environments（環境）](#environments)
- [Authentication and Authorization（認証と承認）](#authentication-and-authorization)

API の使用方法については、以下のページを参照してください。

| Documentation                                                              | Content Description                                                                                                                                                                             |
| :------------------------------------------------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Encrypting Requests and Decrypting Responses](./encryption-decryption.md) | UID2 API のリクエスト/レスポンスワークフロー、リクエストの暗号化と応答の復号化の要件、および Python でのスクリプトの例                                                                          |
| [Endpoints](./endpoints/README.md)                                         | ID トークンを管理し、メールアドレス、電話番号、ハッシュを UID2 と UID2 を生成するために使用したソルトバケット ID に対応付けるための API リファレンスです。                                      |
| [Integration Guides](./guides/README.md)                                   | パブリッシャー、DSP、広告主、データプロバイダーなどの UID2 参加者、および Microsoft Azure、AWS、Snowflake などの Operator Enterprise Partner 向けの UID2 インテグレーション・ワークフローです。 |
| [SDKs](./sdks/README.md)                                                   | Web サイト用 Client-side JavaScript SDK と RTB SDK です。                                                                                                                                       |

API バージョン 1 からの改善点・変更点の一覧は、[UID2 API v1→v2 アップグレードガイド](./upgrade-guide.md) を参照してください。

## UID2 API v1 Compatibility and Upgrade Requirements

ここでは、UID2 API v2 と v1 の互換性について説明します:

- UID2 API v2 は UID2 API v1 と互換性がないため、[アップグレード](./upgrade-guide.md) が必要です。
- v1 エンドポイントは、**2023 年 3 月 31 日**までサポートされます。その後、すべての v1 SDK ファイルとエンドポイント、v0 SDK ファイル、およびすべての未バージョンエンドポイントは、非推奨となり削除されます。
- 以前に発行されたクライアント API キーは、v1 エンドポイントで引き続き機能し、v2 エンドポイントで必要になります。
- v2 エンドポイントを使用するには、[API リクエストの暗号化と API レスポンスの復号化](./encryption-decryption.md) にクライアントシークレットが必要です。

## Environments

すべての UID2 エンドポイントは、同じベース URL を使用します。

| Environment | Cloud Region                 | Code             | Base URL                            |
| :---------- | :--------------------------- | :--------------- | :---------------------------------- |
| テスト環境  | AWS US East (Ohio)           | `us-east-2`      | `https://operator-integ.uidapi.com` |
| 本番環境    | AWS US East (Ohio)           | `us-east-2`      | `https://prod.uidapi.com`           |
| 本番環境    | AWS Asia Pacific (Sydney)    | `ap-southeast-2` | `https://au.prod.uidapi.com`        |
| 本番環境    | AWS Asia Pacific (Tokyo)     | `ap-northeast-1` | `https://jp.prod.uidapi.com`        |
| 本番環境    | AWS Asia Pacific (Singapore) | `ap-southeast-1` | `https://sg.prod.uidapi.com`        |

例えば、https://operator-integ.uidapi.com/v2/token/generate

## Authentication and Authorization

UID2 エンドポイントに対して認証するには、以下が必要です:

- リクエストの認証ヘッダーにベアラートークンとして含まれるクライアント API キーです。
  <br/>`Authorization: Bearer YourTokenBV3tua4BXNw+HVUFpxLlGy8nWN6mtgMlIk=`
- [POST /token/refresh](./endpoints/post-token-refresh.md) を除くすべてのエンドポイントで、API リクエストを暗号化し、API レスポンスを復号化するためのクライアントシークレットです。<br/>詳細と Python の例については、[リクエストの暗号化とレスポンスの復号化](./encryption-decryption.md) を参照してください。
