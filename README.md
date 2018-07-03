### Summary

This CloudFormation template will create a serverless solution to notify your slack channel when your projected usage cost (in dollars) is forecasted to be over 90% of your budgeted dollar amount. It uses the following AWS services:

- **Billing** to set a budget with notifications
- **SNS** to create at topic that will receive billing notifications
- **Lambda** to subscribe to the SNS topic and send notifications to Slack

### Prerequisites

1. Identify or create your destination Slack Channel
2. Configure incoming web hook for your slack workspace, and link to your Slack channel
3. Identify or create a role that has the following permissions:
  - Read/write permission to the S3 bucket you specify when deploying
  - Lambda basic execution role
4. Identify or create an S3 bucket to hold

### Parameters

- **myBudgetName** : The name of your budget that will be listed in the Budgets section of your billing.
- **budgetedAmount** : The dollar amount you would like to set as your maximum spend.
- **slackWebhookURL** : The unique path to your Slack webhook. This should be everything after the services/ path.
- **slackChannel** : The name of the Slack Channel you would like your notifications to arrive in. Don&#39;t include the hash.
- **s3BucketName** : This should be the bucket that is holding your snsToSlack .zip file, and should reside in the same region as your lambda function.
- **s3Key** : This should be the .zip file name for the snsToSlack code residing in your s3 Bucket.

### Steps

1. Create your Slack Webhook and record your Webhook URL somewhere. See this documentation for help on that: [https://get.slack.help/hc/en-us/articles/115005265063-Incoming-WebHooks-for-Slack](https://get.slack.help/hc/en-us/articles/115005265063-Incoming-WebHooks-for-Slack)
2. Upload the snsToSlack.zip file to your S3 bucket
3. Update the role ARN referenced in the CloudFormation template to the role you created in step 3 under prerequisites (line 42).
4. Call the cloudformation cli as follows:
  aws cloudformation deploy --template-file ./BudgetNotifications.template --stack-name pat-budget-stack --s3-bucket [your-bucket-name] --parameter-overrides myBudgetName=[budget-name] budgetedAmount=[budgeted number] slackWebhookURL=&quot;[webhook url]&quot; slackChannel=[slack-channel] s3BucketName=[your-bucket-name] s3Key=snsToSlack.zip