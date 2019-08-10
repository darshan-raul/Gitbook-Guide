# AWS CDK - python

## Why AWS CDK:

 _AWS Cloud Development Kit \(AWS CDK\) :_

So Cloudformation does still have two disadvantages over terraform 

1. Closed Source
2. AWS provider native

> But the biggest disadvantaqe of not actually being  Infrastructure as CODE and state management. That's gone for good thanks to AWS CDK.

You can use the AWS CDK to define your cloud resources in a familiar programming language. The AWS CDK supports TypeScript, JavaScript, and Python.

#### Advantages of AWS CDK:

* AWS CDK is **open source**
* Use logic \(if statements, for-loops, etc\) **FINALLY!!**
* object-oriented techniques to create a model of your system
* Organize your project into logical modules
* Share and reuse your infrastructure as a library
* Use your existing **code review workflow**
* State management \( **diff** against a deployed stack to understand the impact of a code change\)

> There is no charge for using the AWS CDK, however you might incur AWS charges forcreating or using AWS [chargeable resources](http://docs.aws.amazon.com/general/latest/gr/glos-chap.html#chargeable-resources)

## Prerequisites:

> The AWS CDK is developed in TypeScript and transpiled to JavaScript. Bindings for the other supported languages make use of the AWS CDK back-end running on Node.js

* [Node.js \(&gt;= 8.11.x\)](https://nodejs.org/en/download)
* You must specify both your credentials and an AWS Region to use the AWS CDK CLI

## Installation and Configuration needed

### Installation

First install the aws-cdk package

`npm install -g aws-cdk`

Then check the cdk version with:

```text
cdk --version
```

### AWS Configuration:

The CDK looks for credentials and region in the following order:

* Using the **--profile** option to **cdk** commands.
* Using environment variables.
* Using the default profile as set by the AWS Command Line Interface \(AWS CLI\)

## concepts

## practical:

1. Initialize a python cdk app

```text
cdk init --language python
```

2. Create a virtual environment

```text
python3 -m venv .env
source .env/bin/activate
```

3. Install all the packages

```text
pip install -r requirements.txt
```

This would be the initial folder structure:

![](../../../.gitbook/assets/image%20%2870%29.png)

Lets have a look at `app.py`

```text
#!/usr/bin/env python3

from aws_cdk import core


from aws_cdk_example.aws_cdk_example_stack import AwsCdkExampleStack

app = core.App()
AwsCdkExampleStack(app, "aws-cdk-example")

app.synth()

```

### Important parts of the code:

1. `from aws_cdk import core` Used to import the core cdk package
2.   ```text
   from aws_cdk_example.aws_cdk_example_stack import AwsCdkExampleStack

   app = core.App()
   AwsCdkExampleStack(app, "aws-cdk-example")
   ```

       This section is to import our stack app package created during init and then initialze the app and give it a name. You can customize it to use in different accounts and regions during deployments like this:

```text
new MyStack(app, 'Stack-One-W', { env: { account: 'ONE', region: 'us-west-2' }}); ##in code
cdk deploy Stack-Two-W ## during deploying
```

3. `app.synth()` to create a cfn template of this.

Lets modify the code to create a bucket. But first lets install the package for S3.

`pip install aws-cdk.aws-s3`

Now the new code will look like this:

```text
#!/usr/bin/env python3

from aws_cdk import (
    aws_s3 as s3,
    core
)

from aws_cdk_example.aws_cdk_example_stack import AwsCdkExampleStack

class AwsCdkExampleStack(core.Stack):
    def __init__(self, scope: core.App, id: str, **kwargs) -> None:
        super().__init__(scope, id, **kwargs)
        bucket = s3.Bucket(self, 
        "MyFirstBucket", 
        bucket_name='my-bucket-sdfgdhbfghfjg',
        versioned=True)

        



app = core.App()
AwsCdkExampleStack(app, "aws-cdk-example")

app.synth()

```

 Just added a class \(Thanks to the github python cdk examples&gt;&gt; Their documentation still needs some work\) but the main part is this.

```text
bucket = s3.Bucket(self, 
        "MyFirstBucket", 
        bucket_name='my-bucket-sdfgdhbfghfjg',
        versioned=True)
```

*  [Bucket](https://docs.aws.amazon.com/cdk/api/latest/typescript/api/aws-s3/bucket.html) is a construct. This means its initialization signature has `scope`, `id`, and `props` and it is a child of the stack.
* **MyFirstBucket**  is the **id** of the bucket construct, not the physical name of the Amazon S3 bucket. The logical ID is used to uniquely identify resources in your stack across deployments.
* **bucket\_name** parameter is used to give the bucket a name.

Lets start with the commands

 `cdk ls` To view the stacks

You should be able to view the stacks you will be creating `aws-cdk-example` in my case

`cdk synth` to view the cloudformation template that will be created from this code.

![](../../../.gitbook/assets/image%20%2871%29.png)

`cdk deploy` to deploy the stack

![](../../../.gitbook/assets/image%20%2878%29.png)

You will be able to see the changes in the cloudformation console and the bucket will also be created.

![](../../../.gitbook/assets/image%20%2821%29.png)

Now this is the **MOST INTERESTING THING** about cdk and gives a tough to Terraform.

Although there was drift management in Cloudformation it did not compare to `terraform state`

Here comes `cdk diff`

Lets modify the code a bit to add KMS protection to the bucket

```text
bucket = s3.Bucket(self, 
        "MyFirstBucket", 
        bucket_name='my-bucket-sdfgdhbfghfjg',
        versioned=True,
        encryption=s3.BucketEncryption.KMS_MANAGED,)
```

Now run `cdk diff`

![](../../../.gitbook/assets/image%20%2851%29.png)

You will see that it actually tracks that resource and shows what is going to change!! Somewhat similar to terraform state.

Now lets deploy this new change using `cdk deploy`

![](../../../.gitbook/assets/2019-08-10-18_13_55-window.png)

 Thats it!! ofcourse this is just the tip of the iceberg about how powerful this thing is .Essentially it can do everything that Cloudformation could do and more!!

Lets destroy the stack using `cdk destroy`

![](../../../.gitbook/assets/image%20%2827%29.png)

It will ask for your permission before deleting . Press 'y' and the whole stack will be deleted. I have some plans for how to use this as this is python and I can leverage my python skills to create maybe a form type app which will create a IAC cfn template based on the inputs \( Ofcourse its done already :\) [https://github.com/tongueroo/lono](https://github.com/tongueroo/lono)\) But I can still do it as a personal project

