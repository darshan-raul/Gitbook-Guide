# EFS

Go to the EFS console home page:

![](../../.gitbook/assets/image%20%2864%29.png)

An Amazon EFS file system is accessed by EC2 instances running inside one of your VPCs. Instances connect to a file system by using a network interface called a mount target. Each mount target has an IP address, which we assign automatically or you can specify.

![](../../.gitbook/assets/image%20%2854%29.png)

1. Provide the VPC for the mount target
2. Create a mount target in the available subnet's. I have one created.

![](../../.gitbook/assets/screencapture-console-aws-amazon-efs-home-2019-04-23-17_37_34.png)

on the next page:

1. Add Tags
2. **Choose performance mode: General Mode or High IO**
3. **Choose throughput mode: Bursting or Provisioned**
4. Enable Encryption : all data on your file system will be encrypted at rest.\
5. **Enable lifecycle management:**automatically moves files to the lower-cost Infrequent Access storage class based on a predefined lifecycle policy.

Next will be the review page and click 'Create' 

![](../../.gitbook/assets/image%20%28139%29%20%281%29.png)

The following Success page will be visible .

![](../../.gitbook/assets/image%20%2823%29.png)

Mount target will be in Creating state.

![](../../.gitbook/assets/image%20%2822%29.png)

Click this link for the instructions to attach it to EC2 instance.

![After a while the mount target will be available.](../../.gitbook/assets/image%20%2826%29.png)

Time to create a EC2 instance and connect it to the EFS mount target.

Followed the standard procedure to connect to the server and SSH to it.





