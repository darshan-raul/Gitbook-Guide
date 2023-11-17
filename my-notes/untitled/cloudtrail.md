# Cloudtrail

### Introduction:

AWS CloudTrail is an AWS service that helps you enable governance, compliance, and operational and risk auditing of your AWS account**. Actions taken by a user, role, or an AWS service are recorded as events in CloudTrail.**&#x20;

{% hint style="info" %}
### Event

Events include actions taken in the AWS Management Console, AWS Command Line Interface, and AWS SDKs and APIs.
{% endhint %}

**CloudTrail is enabled on your AWS account when you create it.** When activity occurs in your AWS account, that activity is recorded in a CloudTrail event. You can easily view recent events in the CloudTrail console by going to **Event history.**

{% hint style="info" %}
### Event History

Event history allows you to view, search, and download the past 90 days of activity in your AWS account.
{% endhint %}

{% hint style="info" %}
### Trial

A trail is a configuration that enables delivery of events to an Amazon S3 bucket that you specify. You can also deliver and analyze events in a trail with Amazon CloudWatch Logs and Amazon CloudWatch Events. You can create a trail with the CloudTrail console, the AWS CLI, or the CloudTrail API.
{% endhint %}

#### CloudWatch Logs and CloudTrail <a href="#cloudtrail-concepts-cloudwatch-logs" id="cloudtrail-concepts-cloudwatch-logs"></a>

Integration with CloudWatch Logs enables CloudTrail to send events containing API activity in your AWS account to a CloudWatch Logs log group. CloudTrail events that are sent to CloudWatch Logs can trigger alarms according to the metric filters you define.

You can optionally configure CloudWatch alarms to send notifications or make changes to the resources that you are monitoring based on log stream events that your metric filters extract.

{% hint style="info" %}
#### Multiple Trails per Region <a href="#cloudtrail-concepts-trails-multiple-trails-per-region" id="cloudtrail-concepts-trails-multiple-trails-per-region"></a>

If you have different but related user groups, such as developers, security\
personnel, and IT auditors, you can create multiple trails per region. This allows\
each group to receive its own copy of the log files.
{% endhint %}

{% hint style="warning" %}
### Global Service Events <a href="#cloudtrail-concepts-global-service-events" id="cloudtrail-concepts-global-service-events"></a>

For global services such as AWS Identity and Access Management (IAM), AWS STS, Amazon CloudFront, and Route 53, events are delivered to any trail that includes global services, and are logged as occurring in US East (N. Virginia) Region.
{% endhint %}

### CloudTrail Log File Name Format <a href="#cloudtrail-log-filename-format" id="cloudtrail-log-filename-format"></a>

```
AccountID_CloudTrail_RegionName_YYYYMMDDTHHmmZ_UniqueString.FileNameFormat 
```

### Limits in AWS CloudTrail:

| Trails per region | 5 | This limit cannot be increased. |
| ----------------- | - | ------------------------------- |

## CLI commands:

### Get Events/Trial:

#### To see the ten latest events

```
aws cloudtrail lookup-events
```

#### To see last specified number of events:

```
aws cloudtrail lookup-events --max-results <integer>
```

#### To see events by time range :

```
aws cloudtrail lookup-events --start-time <timestamp> --end-time <timestamp>
```

#### To describe a trial:

`aws cloudtrail describe-trails`

`aws cloudtrail get-trail-status --name awscloudtrail-example`

### Create Trial:

#### Creating a single-region trail

`aws cloudtrail create-trail --name my-trail --s3-bucket-name my-bucket`

**Start logging for the trail**

&#x20;After the `create-trail` command completes, run the `start-logging` command to start logging for that trail.

`aws cloudtrail start-logging --name my-trail`

```
aws cloudtrail stop-logging --name awscloudtrail-example
```

#### Creating a trail that applies to all regions**:** <a href="#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-mrt" id="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-mrt"></a>

`aws cloudtrail create-trail --name my-trail --s3-bucket-name my-bucket --is-multi-region-trail`

### **Update trial:**

#### Converting a trail that applies to one region to apply to all regions <a href="#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-convert" id="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-convert"></a>

`aws cloudtrail update-trail --name my-trail --is-multi-region-trail`

#### Converting a multi-region trail to a single-region trail <a href="#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-reduce" id="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-reduce"></a>

`aws cloudtrail update-trail --name my-trail --no-is-multi-region-trail`

#### Enabling and disabling logging global service events <a href="#cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-gses" id="cloudtrail-create-and-update-a-trail-by-using-the-aws-cli-examples-gses"></a>

```
aws cloudtrail update-trail --name my-trail --no-include-global-service-events
```

### Delete Trial:

```
aws cloudtrail delete-trail --name awscloudtrail-example
```

## Creating a Trail for an Organization <a href="#creating-trail-organization" id="creating-trail-organization"></a>

![](<../../.gitbook/assets/image (53).png>)

```
aws organizations enable-all-features
aws organizations enable-aws-service-access --service-principal cloudtrail.amazonaws.com
aws cloudtrail create-trail --name my-trail --s3-bucket-name my-bucket --is-organization-trail --is-multi-region-trail

```

## Finding Cloudtrial files:

This is the default file location structure:

`bucket_name/prefix_name/AWSLogs/AccountID/CloudTrail/region/YYYY/MM/DD/file_name.json.gz`

## Configuring CloudTrail to Send Notifications <a href="#configure-cloudtrail-to-send-notifications" id="configure-cloudtrail-to-send-notifications"></a>

You can configure a trail to use an Amazon SNS topic.

## Limits:

| Resource                          | Default Limit                             | Comments                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| --------------------------------- | ----------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Trails per region                 | 5                                         | This limit cannot be increased.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| Get, describe, and list APIs      | 10 transactions per second (TPS)          | <p>The maximum number of operation requests you can make per second without being throttled. The <code>LookupEvents</code> API is not included in this category.</p><p>This limit cannot be increased.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| LookupEvents API                  | 2 transactions per second (TPS)           | <p>The maximum number of operation requests you can make per second without being throttled.</p><p>This limit cannot be increased.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| All other APIs                    | 1 transaction per second (TPS)            | <p>The maximum number of operation requests you can make per second without being throttled.</p><p>This limit cannot be increased.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Event selectors                   | 5 per trail                               | This limit cannot be increased.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| Data resources in event selectors | 250 across all event selectors in a trail | <p>The total number of data resources cannot exceed 250 across all event selectors in a trail. The limit of number of resources on an individual event selector is configurable up to 250. This upper limit is allowed only if the total number of data resources does not exceed 250 across all event selectors.</p><p>Examples:</p><ul><li>A trail with 5 event selectors, each configured with 50 data resources, is allowed. (5*50=250)</li><li>A trail with 5 event selectors, 3 of which are configured with 50 data resources, 1 of which is configured with 99 data resources, and 1 of which is configured with 1 data resource, is also allowed. ((3*50)+1+99=250)</li><li>A trail configured with 5 event selectors, all of which are configured with 100 data resources, is not allowed. (5*100=500)</li></ul><p>This limit cannot be increased.</p> |
