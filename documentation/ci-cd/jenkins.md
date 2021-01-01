# Jenkins

Creating a Jenkins Pipeline to build an Angular 7 project and deploy on S3 bucket as a static website.

## Prerequisites:

1. cloned Angular project: [https://github.com/darshan-raul/awsdashboard](https://github.com/darshan-raul/awsdashboard)
2. Jenkins Server/ubuntu with min 1gb ram configured with plugins \[All suggested plugins + Blueocean,github \] [https://linuxize.com/post/how-to-install-jenkins-on-ubuntu-18-04/](https://linuxize.com/post/how-to-install-jenkins-on-ubuntu-18-04/) 
3. AWS cli installed and configured with a IAM user \(Having S3FullAccess permissions\) credentials.
4. Node Js / npm installed `apt install npm nodejs`

Else you can use the following userdata/startup script \(for ubuntu 18.04\)

```text
#! /bin/bash
sudo apt-get update
sudo apt install -y npm nodejs python-pip openjdk-8-jdk docker.io
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt install jenkins
pip install awscli
sudo ufw enable
sudo ufw allow 8080
sudo ufw allow 22


```

After this all you need to do is configure awscli and jenkins thats it.

### Steps:

![](../../.gitbook/assets/image%20%288%29%20%281%29.png)

1. Click on  new item on the left side bar in Jenkins main dashboard and on the next page give the job a name.
2. Select the type as Pipeline.

![](../../.gitbook/assets/image%20%28149%29.png)

1. Give the job a description
2. This option determines when, if ever, build records for this project should be discarded. Build records include the console output, archived artifacts, and any other metadata related to a particular build.
3. Here you can mention the project url for the github project

![](../../.gitbook/assets/image%20%28137%29.png)

1. I am selecting the build periodically so that every 12 hours the job is run
2. You can choose the github hook trigger using Github plugin
3. or Poll SCM 

![](../../.gitbook/assets/image%20%28163%29.png)

1. You can either choose to write the pipeline script here or choose the pipeline script from SCM option
2. Here you mention the pipeline script..else you can create one by using the Pipeline Syntax button given below  

Once done you can build the job and open blueocean and see the progress according to stage's you set in pipeline syntax.

![](../../.gitbook/assets/image%20%28156%29%20%281%29.png)

After a while both the stages are built successfully and you can view the logs too.

![](../../.gitbook/assets/image%20%286%29.png)

This was the code:

```text
node {
  
   stage('Preparation') { 
      
      git 'https://github.com/darshan-raul/awsdashboard'
      
   }
   
   stage('Build') {
   dir("folder") {
        sh 'npm install'
        sh 'npm build --prod'
      }
    }
    
}
```

1. In the preparation stage I am just cloning the repo in my workspace directory
2. In the Build stage I am changing the directory to the cloned repo and then running the npm install and npm build commands

Now its time to add another stage where the code is deployed to S3 bucket

![](../../.gitbook/assets/image%20%28165%29.png)

In package.json of the Angular app , I have added a additional script to build as well as copy the files to S3 bucket where the static website is hosted.

Make sure that you have AWS CLI installed on the Jenkins server configured with S3 full access user access keys. 

Try aws s3 ls to confirm.

Now lets edit the JenkinsPipeline code.

```text
node {
  
   stage('Preparation') { 
      
      git 'https://github.com/darshan-raul/awsdashboard'
      
    }
    
   stage('Build') {
    dir("awsdashboard") {
        sh 'pwd'
        sh 'npm install'
     
    }
       
   }
    stage('Deploy') {
   
     
         sh 'npm run deploy'

    }
    
}
```

Build it and then you will see 3 stages now. Let the pipeline complete.

![](../../.gitbook/assets/image%20%28147%29.png)

![](../../.gitbook/assets/image%20%2858%29.png)

This failed as for some reason even after configuring the correct AWS credentials the s3 upload command was failing.

Enter S3 plugin.

In available plugins search for S3 and install S3 publisher.

![](../../.gitbook/assets/image%20%28104%29.png)

Go to Manage jenkin's &gt; Configure System

Scroll Down to  `Amazon S3 profiles` 

![](../../.gitbook/assets/image%20%28100%29.png)

1. Give a profile name
2. Give Access key
3. Give Secret Access key and add profile

Now configure the Pipeline job again and under the pipeline code, Click 'pipeline syntax' . It can be used to create pipeline code

![](../../.gitbook/assets/image%20%28152%29.png)

1. In sample step select S3upload
2. Select the S3 profile you created before
3. give the source folder
4. give the destination bucket
5. give the bucket region

![](../../.gitbook/assets/image%20%28131%29.png)

Copy the pipeline script that is created. Edit the pipeline script and build again.

![](../../.gitbook/assets/image%20%2828%29.png)

It runs smoothly :\)





