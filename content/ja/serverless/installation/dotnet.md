---
title: .NET アプリケーションのインスツルメンテーション
kind: ドキュメント
further_reading:
  - link: serverless/serverless_tagging/
    tag: サーバーレス
    text: サーバーレスアプリケーションのタグ付け
  - link: serverless/distributed_tracing/
    tag: サーバーレス
    text: サーバーレスアプリケーションのトレース
  - link: serverless/custom_metrics/
    tag: サーバーレス
    text: サーバーレスアプリケーションからのカスタムメトリクスの送信
---
## 必須セットアップ

未構成の場合:

- [AWS インテグレーション][1]をインストールします。これにより、Datadog は AWS から Lambda メトリクスを取り込むことができます。
- AWS Lambda トレース、拡張メトリクス、カスタムメトリクス、ログの取り込みに必要な [Datadog Forwarder Lambda 関数][2]をインストールします。

[AWS インテグレーション][1]と [Datadog Forwarder][2] をインストールしたら、手順に従ってアプリケーションをインスツルメントし、Datadog にメトリクス、ログ、トレースを送信します。

## コンフィギュレーション

### Install

1. Lambda 関数の [AWS X-Ray アクティブトレース][3]を有効にします。
2. [.NET 向け AWS X-Ray SDK][4] をインストールします。

### サブスクライブ

メトリクス、トレース、ログを Datadog へ送信するには、関数の各ロググループに Datadog Forwarder Lambda 関数をサブスクライブします。

1. [まだの場合は、Datadog Forwarder をインストールします][2]。
2. [Datadog Forwarder を関数のロググループにサブスクライブします][5]。

### タグ

これはオプションですが、Datadog は、[統合サービスタグ付けのドキュメント][6]に従って、サーバーレスアプリケーションに `env`、`service`、`version` タグをタグ付けすることを強くお勧めします。

### AWS サーバーレスリソースからログを収集

AWS Lambda 関数以外のマネージドリソースで生成されたサーバーレスログは、サーバーレスアプリケーションの問題の根本的な原因を特定するのに役立ちます。Datadog では、お使いの環境の以下のマネージドリソースからログを転送することをお勧めします。
- API: API Gateway、AppSync、ALB
- キューとストリーム: SQS、SNS、Kinesis
- データストア: DynamoDB、S3、RDS など

Lambda 以外の AWS リソースからログを収集するには、[Datadog Forwarder][9] をインストールして構成し、マネージドリソースの各 CloudWatch ロググループにサブスクライブさせます。

### カスタムビジネスロジックの監視

カスタムメトリクスの送信をご希望の場合は、以下のコード例をご参照ください。

```csharp
var myMetric = new Dictionary<string, object>();
myMetric.Add("m", "coffee_house.order_value");
myMetric.Add("v", 12.45);
myMetric.Add("e", (int)(DateTime.UtcNow - new DateTime(1970, 1, 1)).TotalSeconds);
myMetric.Add("t", new string[] {"product:latte", "order:online"});
LambdaLogger.Log(JsonConvert.SerializeObject(myMetric));
```

カスタムメトリクスの送信について、詳細は、[サーバーレスカスタムメトリクスに関するドキュメント][6]を参照してください。

## その他の参考資料

{{< partial name="whats-next/whats-next.html" >}}

[1]: /ja/integrations/amazon_web_services/
[2]: /ja/serverless/forwarder/
[3]: https://docs.aws.amazon.com/xray/latest/devguide/xray-services-lambda.html
[4]: https://docs.aws.amazon.com/xray/latest/devguide/xray-sdk-dotnet.html
[5]: /ja/logs/guide/send-aws-services-logs-with-the-datadog-lambda-function/#collecting-logs-from-cloudwatch-log-group
[6]: /ja/serverless/custom_metrics?tab=otherruntimes