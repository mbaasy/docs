# Integrations - Amazon SQS

Like [Webooks](/integrations/webhooks), Amazon SQS integrations are a way to deliver [Event payloads](/integrations/event_payloads) to your API or data consumer via an Amazon SQS Queue.

To enable an SQS integration, first [create a standard or FIFO queue](https://console.aws.amazon.com/sqs/home) on the AWS console and make note of the `ARN` and `URL`.

Next, [create a policy](https://console.aws.amazon.com/iam/home?#/policies$new?step=edit) with the following JSON policy, replacing `[QUEUE ARN]`, with the ARN of your queue.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "sqs:SendMessage",
      "Resource": "[QUEUE ARN]"
    }
  ]
}
```

`sqs:SendMessage` is the only permission Mbaasy needs to deliver messages to your queue.

Next, [create a new IAM user](https://console.aws.amazon.com/iam/home?#/users$new?step=details) with *Programmatic accsss* and attach the new policy.

Once your user is created, a new `Access Key ID` and `Secret Access Key` will be furnished for you.

Finally, visit the *[Mbaasy App Publisher Console](https://console.mbaasy.com) > [App] > Settings > Integrations* page and add a new SQS integration by selecting `AWS SQS` as the *target*, enter a unique `name`, the `AWS region`, `Access Key ID` and `Secret Access Key` and press save.

### Signature Validation

Every event that is sent to your SQS queue are signed using unique a `SHA256 HMAC Key` in the `X-SHA256-Digest` message attribute. You can verify the authenticity of the payload by generating a comparison signature using the HMAC Key, a `SHA256 digest` and the `JSON` body.

You will find your HMAC Key in the options menu beside each integration in the App General Settings page on the Mbaasy App Publisher Developer Console.
