---
title: Troubleshooting Serverless Instrumentation
kind: documentation
further_reading:
- link: '/serverless/installation/'
  tag: 'Documentation'
  text: 'Installing Serverless Monitoring'
- link: '/serverless/guide/'
  tag: 'Documentation'
  text: 'Troubleshoot Common Issues'
---

This guide helps you troubleshoot general serverless monitoring data collection issues, such as missing metrics, traces, logs or tags. Be sure to also check out the other [troubleshooting guides][1] for common issues, such as function code size is too large, and webpack compatibility.

## Understand the basics

To follow the instructions from the rest of the guide, Datadog recommends reading the [how serverless monitoring works guide][2] to understand the basic concepts and data collection mechanisms. A better understanding may help you identify the missing pieces or narrow down the troubleshooting surface.

## Use Datadog Lambda Extension instead of the Forwarder

If you are still using the [Datadog Forwarder Lambda function][3] for data collection, consider adopting the [Datadog Lambda Extension][4] instead. Many known issues due to the technical limitations from the Forwarder Lambda can be resolved automatically after migrating to the Lambda Extension.

If you are unsure whether your Lambda is using the Extension, check your Lambda function's [layer configurations][5] and see if any Lambda layer is named `Datadog-Extension`.

If you are unsure whether your Lambda is using the Forwarder, check the [subscription filters][6] of your Lambda function's log group, and see if the log group is subscribed by a Lambda named `Datadog Forwarder` or something similar.

Refer to this [comparison guide][7] to understand the benefits of using the Extension and this [migration guide][8] for the general migration steps. Be sure to test the changes in your _dev_ or _staging_ environment first!

If you do want to continue using the Forwarder, refer to the [Forwarder troubleshooting guide][9] for additional help as well.

## Use Datadog APM instead of AWS X-Ray

If you are collecting traces AWS X-Ray through the [Datadog X-Ray integration][10],

## Ensure the configurations are up to date

Be sure to check out the [troubleshooting guides][1] for common issues, such as function code size too large, webpack configuration issues and you may find a guide that addresses your specific problem.

## Try a different configuration method

## Enable debugging logs

## Check the AWS integration configuration

## Get help

Slack

Github

Support

If you need the Datadog support team to help investigate, include the following information in your ticket:

1. The function's configured Lambda layers (name and version, or ARNs).
2. The function's deployment package (or a screenshot showing the content and size of the uncompressed package) to be uploaded to AWS.
3. The project configuration files, with **redacted hardcoded secrets**: `serverless.yaml`, `package.json`, `package-lock.json`, `yarn.lock`, `tsconfig.json` and `webpack.config.json`.

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: /serverless/guide/
[2]: /serverless/guide/how_serverless_monitoring_works/
[3]: /serverless/libraries_integrations/forwarder/
[4]: /serverless/libraries_integrations/extension/
[5]: https://docs.aws.amazon.com/lambda/latest/dg/invocation-layers.html
[6]: https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/SubscriptionFilters.html
[7]: /serverless/guide/extension_motivation/
[8]: /serverless/guide/forwarder_extension_migration/
[9]: /logs/guide/lambda-logs-collection-troubleshooting-guide/
[10]: /integrations/amazon_xray/
