# Jenkins

Creating a Jenkins Pipeline to build an Angular 7 project and deploy on S3 bucket as a static website.

## Prerequisites:

1. cloned Angular project: [https://github.com/darshan-raul/awsdashboard](https://github.com/darshan-raul/awsdashboard)
2. Jenkins Server/ubuntu with min 1gb ram configured with plugins \[All suggested plugins + Blueocean,github \] [https://linuxize.com/post/how-to-install-jenkins-on-ubuntu-18-04/](https://linuxize.com/post/how-to-install-jenkins-on-ubuntu-18-04/) 
3. AWS cli installed and configured with a IAM user \(Having S3FullAccess permissions\) credentials.
4. Node Js / npm installed `apt install npm nodejs`

### Steps:

![](../../.gitbook/assets/image%20%284%29.png)

1. Click on  new item on the left side bar in Jenkins main dashboard and on the next page give the job a name.
2. Select the type as Pipeline.

![](../../.gitbook/assets/image%20%2861%29.png)

1. Give the job a description
2. This option determines when, if ever, build records for this project should be discarded. Build records include the console output, archived artifacts, and any other metadata related to a particular build.
3. Here you can mention the project url for the github project

