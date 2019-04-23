# Cloudtrail

### Introduction:

AWS CloudTrail is an AWS service that helps you enable governance, compliance, and operational and risk auditing of your AWS account**. Actions taken by a user, role, or an AWS service are recorded as events in CloudTrail.** 

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

#### CloudWatch Logs and CloudTrail <a id="cloudtrail-concepts-cloudwatch-logs"></a>

Integration with CloudWatch Logs enables CloudTrail to send events containing API activity in your AWS account to a CloudWatch Logs log group. CloudTrail events that are sent to CloudWatch Logs can trigger alarms according to the metric filters you define.

You can optionally configure CloudWatch alarms to send notifications or make changes to the resources that you are monitoring based on log stream events that your metric filters extract.

{% hint style="info" %}
#### Multiple Trails per Region <a id="cloudtrail-concepts-trails-multiple-trails-per-region"></a>

If you have different but related user groups, such as developers, security  
personnel, and IT auditors, you can create multiple trails per region. This allows  
each group to receive its own copy of the log files.
{% endhint %}

{% hint style="warning" %}
### Global Service Events <a id="cloudtrail-concepts-global-service-events"></a>

For global services such as AWS Identity and Access Management \(IAM\), AWS STS, Amazon CloudFront, and Route 53, events are delivered to any trail that includes global services, and are logged as occurring in US East \(N. Virginia\) Region.
{% endhint %}

### CloudTrail Log File Name Format <a id="cloudtrail-log-filename-format"></a>

```text
AccountID_CloudTrail_RegionName_YYYYMMDDTHHmmZ_UniqueString.FileNameFormat 
```

### Limits in AWS CloudTrail:

| Trails per region | 5 | This limit cannot be increased. |
| :--- | :--- | :--- |


### CLI commands:

#### To see the ten latest events

```text
aws cloudtrail lookup-events
```





