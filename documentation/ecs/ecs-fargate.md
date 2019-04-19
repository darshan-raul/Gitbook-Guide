# ECS Fargate

AWS Fargate :Run containers without managing servers or clusters

 AWS Fargate is a compute engine for Amazon ECS that allows you to run [containers](http://aws.amazon.com/what-are-containers) without having to manage servers or clusters. With AWS Fargate, you no longer have to provision, configure, and scale clusters of virtual machines to run containers. This removes the need to choose server types, decide when to scale your clusters, or optimize cluster packing. AWS Fargate removes the need for you to interact with or think about servers or clusters. Fargate lets you focus on designing and building your applications instead of managing the infrastructure that runs them.

That's just the copy pasted paragraph from AWS's article but that give's a bird's eye view of the whole thing :\)

![The benifits look amazing compared to running normal ECS on EC2](../../.gitbook/assets/image%20%2812%29.png)

![](../../.gitbook/assets/image%20%287%29.png)

Lets get our hands dirty :

![](../../.gitbook/assets/image.png)

Go to the Get started page and click edit on the container definition section.

![](../../.gitbook/assets/image%20%2811%29.png)

1. Give the container a name
2. Give the image name. My AWSapi image on dockerhub in this case
3. Give the port no's which are being exposed/used by the container

![](../../.gitbook/assets/image%20%281%29.png)

If you click the Advanced configuration option than many more option's like healthcheck/environment's appear.

I am avoiding them and click update.

![](../../.gitbook/assets/image%20%283%29.png)

You should be able to see your container highlighted here. Confirm the image/cpu settings before moving ahead. We will be changing that in the next step.

![](../../.gitbook/assets/image%20%2810%29.png)

Click edit on the task definition.

![](../../.gitbook/assets/image%20%282%29.png)

1. Give the task a name
2. Your default network mode is awsvpc // thats the docker networking mode
3. Role given to ECS to pull private images etc
4. Launch type is  FARGATE
5. This is the Memory allocated to single task.. you can change it according to your container needs
6. Same thing applies to CPU specifications.

I choose the default ones and move ahead.

![](../../.gitbook/assets/image%20%285%29.png)

Next is the service definition.. Service is basically the instance of the task definition. Click 'Edit'

![](../../.gitbook/assets/image%20%284%29.png)

1. Give the service a name
2. Give the number of task's you want running in a service. I am selecting 2.
3. Give the security group details here.
4. Select the load balancer type or none.
5. Select the port no for the Load balancer









1. 


