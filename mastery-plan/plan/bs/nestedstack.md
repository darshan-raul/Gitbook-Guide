## Nested Cloudformation Stack

Nested stacks are stacks created as part of other stacks. You create a nested stack within another stack by using the *AWS::CloudFormation::Stack resource*.
Nested stacks can themselves contain other nested stacks, resulting in a hierarchy of stacks
- Stack A is the root stack for all the other, nested, stacks in the hierarchy.

- For stack B, stack A is both the parent stack, as well as the root stack.

- For stack D, stack C is the parent stack; while for stack C, stack B is the parent stack.

    ![nested stacks](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/images/cfn-console-nested-stacks.png)


## Steps:

- First lets start by creating a **nested stack** which will create a single resource: S3 bucket.Then will upload the template to a S3 bucket and copy the Url to be used in the root stack.
- Next I will create a **root stack** which will have this nested stack as one of the resource and provide the nested stack's s3 url as a template URL.
- Then I will just launch the root stack and the nested stack should be created automatically.

## Creating Nested Stack:

Here is the Yaml template for a Single S3 bucket:
- In Resources field a single S3 bucket is created with type:   *AWS::S3::Bucket*
- In Outputs field the bucketname of the S3 bucket is referred

![](https://thor1345r75.s3.ap-south-1.amazonaws.com/nestedcfn1.png)

## Uploading to S3:

Created an empty bucket to upload the template to.

![](https://thor1345r75.s3.ap-south-1.amazonaws.com/nestedcfn2.png)

Then upload the nested stack yaml here 

![](https://thor1345r75.s3.ap-south-1.amazonaws.com/nestedcfn3.png)

Make sure that you have permission on the object

![](https://thor1345r75.s3.ap-south-1.amazonaws.com/nestedcfn4.png)

Once uploaded, Copy the url of the template
![](https://thor1345r75.s3.ap-south-1.amazonaws.com/nestedcfn5.png)

## Creating the master stack:

Here
- We are giving a parameter for the s3 template url of the nested stack
- Then in the resources field we are declaring the stack with name Bucketstack and type: *AWS::CloudFormation::Stack* 
- And in the properties we provide the TemplateUrl of the nested stack yml file
![](https://thor1345r75.s3.ap-south-1.amazonaws.com/nestedcfn6.png)

## Uploading the main stack in Cloudformation

Now go to the **NEW** AWS CLoudformation console and click on ***Create Stack***

On the next window, upload the main stack template file
![](https://thor1345r75.s3.ap-south-1.amazonaws.com/nestedcfn7.png)

Next page, Give a name to the stack and press next.

![](https://thor1345r75.s3.ap-south-1.amazonaws.com/nestedcfn8.png)

Follow the next steps and then Click create stack . you should be able to see that the main stack is being created.

![](https://thor1345r75.s3.ap-south-1.amazonaws.com/nestedcfn9.png)

After a while the nested stack will also be created and the status property of the main stack also shows: **CREATE_COMPLETE** 

![](https://thor1345r75.s3.ap-south-1.amazonaws.com/nestedcfn10.png)
