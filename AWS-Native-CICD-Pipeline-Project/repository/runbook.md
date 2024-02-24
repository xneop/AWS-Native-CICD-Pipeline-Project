# CI/CD Pipeline Project Using AWS Native SDLC Automation Tools
![CompleteAWSNativeCICDProject!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/aws_native_project_v2.2.png)

###### Project ToolBox 🧰
- [CodeCommit](https://aws.amazon.com/codecommit/) CodeCommit is a secure, highly scalable, fully managed source control service that hosts private Git repositories.
- [CodeBuild](https://aws.amazon.com/codebuild/) CodeBuild is a fully managed continuous integration service that compiles source code, runs tests, and produces ready-to-deploy software packages.
- [CodeArtifact](https://aws.amazon.com/codeartifact/) CodeArtifact allows you to store artifacts using popular package managers and build tools like Maven, Gradle, npm, Yarn, Twine, pip, NuGet, and SwiftPM.
- [CodeDeploy](https://aws.amazon.com/codedeploy/) CodeDeploy is a fully managed deployment service that automates software deployments to various compute services, such as EC2, ECS, Lambda, and on-premises.
- [CodePipeline](https://aws.amazon.com/codepipeline/) CodePipeline is a fully managed continuous delivery service that helps you automate your release pipelines for fast and reliable application and infrastructure updates.
- [Amazon S3](https://aws.amazon.com/s3/) Amazon S3 is an object storage service offering industry-leading scalability, data availability, security, and performance.
- [EC2](https://aws.amazon.com/ec2/) EC2 allows users to rent virtual computers (EC2) to run their own workloads and applications.
- [CloudWatch Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html) Amazon CloudWatch can load all the metrics in your account (both AWS resource metrics and application metrics that you provide) for search, graphing, and alarms.
- [CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html) You can use CloudWatch Logs to monitor applications and systems using log data. 
- [Amazon SNS](https://aws.amazon.com/sns/) Amazon SNS sends notifications two ways and provides high-throughput, push-based, many-to-many messaging between distributed systems, microservices, and event-driven serverless applications. 
- [SonarCloud](https://sonarcloud.io/) SonarCloud is a cloud-based code analysis service designed to detect coding issues in 26 different programming languages.

**NOTE:** 
a) Navigate to a Region on AWS that has all the AWS Code services (CodeCommit, CodeBuild, CodeArtifact, CodeDeploy and CodePipeline)
b) You must Login as an IAM User before you can complete the below steps successfully (Login with a user that has Administrator Privileges)

## 1) Create a CodeCommit Project Repository
1.1) Navigate to CodeCommit to create a Project Repository
- Click on `Create Repository`
- Name your repository `AWS-Native-CICD-Pipeline-Project`
- Click `Create`
![CodeCommit!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-03%20at%206.11.38%20PM.png)

1.2) Configure SSH Connection On Local MacOs or Windows With CodeCommit
- Follow the steps provided in the Runbook below
- Runbook: https://docs.google.com/document/d/1vDu6TBcIrh2NbUtv5eayur1Cdo3Z8X5YSZ9t58ItlXo/edit?usp=sharing

1.3) Download the Project Zip Code From The Below Repository Link
- Project Code: https://github.com/awanmbandi/realworld-cicd-pipeline-project/tree/aws-native-cicd-pipeline-project
- Unzip and Copy everything to the Code Commit Repository you just cloned
- Push the Code Upstream to Your CodeCommit Project Repository and Confirm you have everything in the Repository 
- NOTE: Use the same Git commands you have always used to Push code to GitHub

## 2) Create A CodeBuild IAM Profile/Role
- Create a CodeBuild Service Role in IAM with Administrator Privilege 
![IAM!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-03%20at%206.20.44%20PM.png)
- Navigate to IAM
    - Click on `Roles`
    - Click on `Create Role`
    - Select `Service Role`
    - Search/Select `CodeBuild`
        - Click on `Next` 
        - Attach Policy: `AdministratorAccess`
        - Click `Next` 
        - Role Name: `AWS-CodeBuild-Admin-Role` 
        - Click `Create`

## 3) Create A CodeDeploy IAM Profile/Role
- Create a CodeBuild Service Role in IAM with Administrator Privilege 
![IAM!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-03%20at%206.20.44%20PM.png)
- Navigate to IAM
    - Click on `Roles`
    - Click on `Create Role`
    - Select `Service Role`
    - Search/Select `CodeDeploy`
        - Click on `Next` 
        - Attach Policy: `AdministratorAccess`
        - Click `Next` 
        - Role Name: `AWS-CodeDeploy-Deployment-Role`
        - Click `Create`

## 4) Create An S3 Bucket Where The Build Artifact Will Be Stored
- Navigate to Amazon S3
![S3!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-03%20at%206.15.44%20PM.png)
- Click `Create Bucket` 
    - Name: `java-webapp-project-artifact-YOUR_ACCOUNT_ID`
    - Region: `Select Your working Region`
    - Click: `CREATE Bucket`


## 5) Sign Up For SonarCloud Account
A) Sign up for SonarCloud using this URL: https://sonarcloud.io or https://sonarcloud.io/login 
![SonarCloud!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-06%20at%2012.59.04%20PM.png)
- Click on `SIGN UP`
- Click Sign Up With `GITHUB`
- Sign in with your `“GitHub” Account`
- Click on `“Authorize SonarCloud”`
    - **NOTE:** Once you Authorize SonarCloud, It’ll take you directly to Dashboard (Similar to Traditional SonarQube Server Dashboard but this is cloud based)

B) We have to Generate a Token which CodeBuild will use during the Maven Execution
![SonarCloud2!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-06%20at%201.03.17%20PM.png)
- Click on your Profile at the Top right and click on `My Account`
- Click on `Security`
- Generate Tokens (Provide a Name): `aws-native-cicd-pipeline-project`
- Copy the Token and Save it somewhere (on your NOTEPAD)

C) Click on the “+”  symbol at the Top Right
![SonarCloud3!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-06%20at%201.06.19%20PM.png)
- Click on Analyze new project
- Click on `create a project manually`.
    - Click on `Create another organization` 
        - Name (Must be Unique): `yourfirst-or-lastname-aws-devops-org`
        - Choose a plan: Select `“Free Plan”`
        - Click on `Create Organization`

D) Create a SonarCloud Project
- Click on `Create Project`
    - Project Key: `aws-native-cicd-pipeline-project`
    - Display name: `This Will Populate Automatically`
    - Public/Private: Public  (because we’re using a free account)
    - Click on `Next`
    ![SonarCloud4!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-09%20at%2011.07.09%20AM.png)
    - Select `Previous version`
    - Click on `Create Project`
**NOTE** **Save your `Project Name` as well on Notepad, Save your `Organization name` and the Sonarcloud url (`https://sonarcloud.io`). Make sure your `Token` has been saved also.

## 6) Create & Configure CodeArtifact Repository to Store and Manage All Application Maven Dependencies.
### A) Create CodeArtifact Project Repository
* Navigate to AWS `CodeArtifact` 
* Click on `Repository`
    * Click `Create Repositoy`
![CodeArtifact!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-08%20at%2011.55.53%20PM.png)
  * Repository Name: `java-webapp-maven-repo`
  * Public Upstream Repository: Select `maven-central-store`
  * Click on `Next`
    * AWS Account: Select `This AWS Account`
    * Domain Name: `java-webapp-maven-repo`
    * Click on `Next`
    * Click `Create Repository`
  * **NOTE:** Verify and Confirm both the `Repository` and the `Repository Domain` were created successfully
![CodeArtifact!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-09%20at%2012.13.04%20AM.png)

### B) Configure Your CodeArtifact Project Repository With Maven POM.xml and Settings.xml
  * Click on `Repositories` if you’ve not already
    * Click on `maven-central-store`
    * Click on `View Connection Instructions`
    ![CodeArtifact!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-09%20at%2012.45.21%20AM.png)
      * **Step 1:** Choose a package manager client: "Select (on the drop down):" `mvn`
      * **Step 3:** **COPY** and Run The `export` Command on your Local Terminal where `awscli` is installed
        * *NOTE:NOTE:NOTE:NOTE!!* 
        - The command will look like this 
        - BUT COPY YOUR OWN
        - Make sure your AWSCLI is configured (with a user with "Admin Priviledges")
        ```bash
        export CODEARTIFACT_AUTH_TOKEN=`aws codeartifact get-authorization-token --domain java-webapp-maven-repo --domain-owner 213424289791 --region us-east-1 --query authorizationToken --output text`
        ```
        - Also RUN: `echo $CODEARTIFACT_AUTH_TOKEN`
        * *NOTE:NOTE:*
            - Copy the `CODEARTIFACT_AUTH_TOKEN` Encrypted Credential and `SAVE` on your `NOTEPAD/Somewhere`
            - We’re going to store this Token in SSM Parameter Store from where our CodeBuild Job is going pick it up
        ![CodeArtifact!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-09%20at%2012.59.21%20AM.png)

### B.1) Update the Settings.xml File With CodeArtifact Repository Configurations
  * Still on `“View Connection Instructions”` in `maven-central-store`
  ![CodeArtifact!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-09%20at%2012.51.13%20AM.png)
  * Under Step 5: 
    - `COPY` the Repository `id` and Paste it in the `settings.xml` file on `line 29` at the time of this
    - `COPY` the Repository `url` and Paste on `Line 18` and `Line 30` in the `settings.xml` at the time of this
    - `SAVE` the changes made in the file
    ![CodeArtifact!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-09%20at%2012.54.34%20AM.png)

### B.2) Update the POM.xml File With CodeArtifact Repository Configurations
  * Still on `“View Connection Instructions”` in `maven-central-store`
  ![CodeArtifact!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-09%20at%2012.51.13%20AM.png)
  * Under Step 5: 
    - `COPY` the Repository `id` and Paste it in the `POM.xml` file on `Line 75` at the time of this
    - `COPY` the Repository `url` and Paste on `Line 76` in the `POM.xml` at the time of this
    - `SAVE` the changes made in the file
    - `COMMIT` the changes and `PUSH` to UpStream to `CodeCommit`
    ![CodeArtifact!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-09%20at%201.03.00%20AM.png)

## 7) Store Your AWS CodeArtifact & SonarCloud Project Parameters/Values In SSM Parameter Store
- Navigate to SSM
- **NOTE!!** Make sure you create the parameters in the same Region as the bucket (same for all project resources)
![ssmps!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-06%20at%201.18.36%20PM.png)
- **a)** Click on `Parameter Store`
  - Click on `Create Parameter`
  - Name: `CODEARTIFACT_AUTH_TOKEN`
  - Type: Select `Secure/String`
  - Value: `provide your CodeArtifact Token` the one you copied when you ran `echo $CODEARTIFACT_AUTH_TOKEN`

- **a)** Click on `Parameter Store`
  - Click on `Create Parameter`
  - Name: `Organization`
  - Type: Select `String`
  - Value: `provide your SnarCloud Org name`

- **b)** Click on `Parameter Store`
  - Name: `HOST`
  - Type: Select `String`
  - Value: https://sonarcloud.io

- **c)** Click on `Parameter Store`
  - Name: `Project`
  - Type: Select `String`
  - Value: `provide your SonaCloud Project name`

- **d)** Click on `Parameter Store`
  - Name: `sonartoken`
  - Type: Select `Secure/String`
  - Value: `provide your SonaCloud Project Token`
**NOTE:** Confirm that these same parameter names exist in your `sonacloud_buildspec.yaml` configuration.
#### 7.1) Confirm That Have All Required Parameters Created With Their Respective Values
![ssmps!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-09%20at%2010.32.08%20AM.png)

## 8) Create The Project Build Job in CodeBuild
- Navigate To The AWS `CodeBuild` Service
![CodeBuild!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-03%20at%206.17.16%20PM.png)
- Click on `Create Build Project` 
    - Project Name: `Java-Webapp-CB-Build-Job`
    - Source Provider: Select `AWS CodeCommit`
    - Repository: Select `AWS-Native-CICD-Pipeline-Project`
    - Branch: `master`
    - Operating System: `Ubuntu`
    - Runtime: `Standard`
    - Image: MUST USE  (`aws/codebuild/standard:5.0`) or else it’ll BREAK
    - Image version: Select `Always use the latest for this runtime version` 
    - Environment type: Select `Linux EC2`
    - Service Role: `Existing Service Role`
        - Role name: Select `AWS-CodeBuild-Admin-Role` 
        - Allow AWS CodeBuild to modify this service role so it can be used with this build project: `Disable/Uncheck`
    - Build Specifications: Pass `buildspecs/buildspec.yml`
    - Artifacts:
        - Type: Select `Amazon S3`
        - Bucket name: Select your bucket `java-webapp-project-artifact-YOUR_ACCOUNT_ID`
        - Artifacts packaging: Select `Zip`
    - Logs
        - CloudWatch Logs: `Enable`
        - Group Name: `Java-Webapp-CodeBuild-Project-Logs`
        - Stream name: `Java-Webapp-CodeBuild-Build-Logs`
    - CLICK: Click `CREATE BUILD PROJECT`

## 9) Create The SonarCloud Code Analysis Job in CodeBuild
- Navigate To The AWS `CodeBuild` Service
![CodeBuild!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/patisdsd_dah_sada.png)
- Click on `Create Build Project` 
    - Project Name: `Java-Webapp-CB-SonarCloud-Job`
    - Source Provider: Select `AWS CodeCommit`
    - Repository: Select `AWS-Native-CICD-Pipeline-Project`
    - Branch: `master`
    - Operating System: `Ubuntu`
    - Runtime: `Standard`
    - Image: MUST USE  (`aws/codebuild/standard:5.0`) or else it’ll BREAK
    - Image version: Select `Always use the latest for this runtime version` 
    - Environment type: Select `Linux EC2`
    - Service Role: `Existing Service Role`
        - Role name: Select `AWS-CodeBuild-Admin-Role` 
        - Allow AWS CodeBuild to modify this service role so it can be used with this build project: `Disable/Uncheck`
    - Build Specifications: Pass `buildspecs/sonarcloud_buildspec.yml`
    - Artifacts:
        - Type: Select `No Artifact`
    - Logs
        - CloudWatch Logs: `Enable`
        - Group Name: `Java-Webapp-CodeBuild-Project-Logs`
        - Stream name: `Java-Webapp-CodeBuild-SonarCloud-Logs`
    - CLICK: Click `CREATE BUILD PROJECT`
![CodeBuild!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-09%20at%2012.04.26%20PM.png)

## 10) Create Staging Deployment Area/Environment
- Navigate to EC2
![EC2!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-03%20at%206.34.34%20PM.png)
- Click `Launch Instances`
    - Name: `Stage-Env`
    - Click `Add additional tags`
        - Click `Add new tag`
            - Key: `Env`
            - Value: `Stage`
    - Number of Instances: `1`
    - AMI: `Amazon Linux 2 (HVM)`
    - Instance type: `t2.micro`
    - Key pair: `Select an existing Key` or `Create New`
    - Network Settings: 
        - VPC: `Default` or a network that has Internet access
        - Auto-assign public IP: `Enable`
        - Firewall (security groups): Open the following Ports
            - Name: `Tomcat-App-SG`
            - Description: `Tomcat-App-SG`
            - Open Port `8080` to `0.0.0.0/0`
            - Open Port `22` to eith your Network or Internet
    - Edvance Details:
        - IAM instance profile: `Select an EC2 Admin Role`
            - NOTE: `If you do not have one, please go ahead and create before creating instance`
            - NOTE: `If Not Yours Will Break`
        - User data: 
        ```bash
        #!/bin/bash
        sudo yum update
        sudo yum install ruby -y
        sudo yum install wget -y
        cd /home/ec2-user
        wget https://aws-codedeploy-us-west-2.s3.us-west-2.amazonaws.com/latest/install
        chmod +x ./install
        sudo ./install auto
        sudo service codedeploy-agent status
        ```

        - Click `Launch Instance`

## 11) Create Production Deployment Area/Environment
![EC2!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-03%20at%206.34.34%20PM.png)
- Navigate to EC2
- Click `Launch Instances`
    - Name: `Prod-Env`
    - Click `Add additional tags`
        - Click `Add new tag`
            - Key: `Env`
            - Value: `Prod`
    - Number of Instances: `1`
    - AMI: `Amazon Linux 2 (HVM)`
    - Instance type: `t2.micro`
    - Key pair: `Select an existing Key` or `Create New`
    - Network Settings: 
        - VPC: `Default` or a network that has Internet access
        - Auto-assign public IP: `Enable`
        - Firewall (security groups): Open the following Ports
            - Click on `Select existing security group`
            - Security group: Select `Tomcat-App-SG`
    - Edvance Details:
        - IAM instance profile: Select an EC2 Admin Role
            - NOTE: `If you do not have one, please go ahead and create before creating instance`
            - NOTE: `If Not Yours Will Break`
        - User data: 
        ```bash
        #!/bin/bash
        sudo yum update
        sudo yum install ruby -y
        sudo yum install wget -y
        cd /home/ec2-user
        wget https://aws-codedeploy-us-west-2.s3.us-west-2.amazonaws.com/latest/install
        chmod +x ./install
        sudo ./install auto
        sudo service codedeploy-agent status
        ```

        - Click `Launch Instance`
#### 11.1) Confirm that you have both the Stage and Prod Environments
![Stage&ProdInstances!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-09%20at%2010.25.51%20AM.png)

## 12) Create CodeDeploy Application
- Navigate to CodeDeploy
![CDApp!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-03%20at%205.11.57%20PM.png)
- Click on `Applications`
    - Click `Create Application`
        - Name: `Java-Webapp-CodeDeploy-Application`
        - Compute Platform: `EC2/On-premises`
        - Click `Create Application`

## 13) Create A CodeDeploy Deployment Group To Deploy Staging Env
- Navigate to CodeDeploy
![CDApp!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-03%20at%206.26.42%20PM.png)
- Click on `Applications`
    - Click on `Java-Webapp-CodeDeploy-Application`
    - Click on `Create deployment group`
        - Deployment group name: `Java-Webapp-CodeDeploy-Stage-DG`
        - Service role: `AWS-CodeDeploy-Deployment-Role`
        - Deployment type: Select `In-place`
        - Environment configuration: Select `Amazon EC2 instances`
            - Key: `Env`
            - Value: `Stage`
        - Agent configuration with AWS Systems Manager: `Now and schedule updates`
            - Basic Scheduler
        - Deployment settings: Select `CodeDeployDefault.AllAtOnce`
            - **NOTE:** CONFIRM THAT YOU SELECTED `CodeDeployDefault.AllAtOnce` IF Not, yours will break.
        - Load balancer: Uncheck the box to `Disable`
        - Click `Create deployment group`

## 14) Create A CodeDeploy Deployment Group For The Production Env
- Navigate to CodeDeploy
![CDApp!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-03%20at%206.26.42%20PM.png)
- Click on `Applications`
    - Click on `Java-Webapp-CodeDeploy-Application`
    - Click on `Create deployment group`
        - Deployment group name: `Java-Webapp-CodeDeploy-Prod-DG`
        - Service role: `AWS-CodeDeploy-Deployment-Role`
        - Deployment type: Select `In-place`
        - Environment configuration: Select `Amazon EC2 instances`
            - Key: `Env`
            - Value: `Prod`
        - Agent configuration with AWS Systems Manager: `Now and schedule updates`
            - Basic Scheduler
        - Deployment settings: Select `CodeDeployDefault.AllAtOnce`
            - **NOTE:** CONFIRM THAT YOU SELECTED `CodeDeployDefault.AllAtOnce` IF Not, yours will break.
        - Load balancer: Uncheck the box to `Disable`
        - Click `Create deployment group`

## 15) Create The CI/CD Automation Pipeline With CodePipeline
- Navigate to `CodePipeline`
![CP!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Pipeline.png)
- Click on `Create Pipeline`
    - Name: `AWS-Native-Java-Webapp-CICD-Pipeline-Automation`
    - Service role: `New service role`
        - Role name: `The name will populate automatically`
    - Allow AWS CodePipeline to create a service role so it can be used with this new pipeline: Chech box to `Enable`
        - Click `Next`
    - **SOURCE PROVIDER**
    - Source provider: Select `AWS CodeCommit`
        - Repository name: `AWS-Native-CICD-Pipeline-Project`
        - Branch name: `Master`
        - Change detection options: `Amazon CloudWatch Events (recommended)`
        - Output artifact format: `CodePipeline default`
        - Click `Next`
    - **BUILD PROVIDER**
    - Build provider: `AWS CodeBuild`
        - Region: `Your region will populate`
        - Project name: `Java-Webapp-CB-Build-Job`
        - Build type: `Single build`
        - Click `Next`
    - **DEPLOY PROVIDER**
    - Deploy provider: `AWS CodeDeploy`
        - Region: `Your region will populate`
        - Application name: `Java-Webapp-CodeDeploy-Application`
        - Deployment group: `Java-Webapp-CodeDeploy-Stage-DG`
        - Click `Next`
    
    - Click `CREATE PIPELINE`
    - NOTE: Once you create the pipeline, it'll start Running Immediate. Just wait for all the various stages to complete
    - **NOTE2:** The Deployment Will Only Take Place In The Staging Environment (With Continuous Deployment)

## 16) Add The SAST Test Stage With SonarCloud
![EditPipeline!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/dsdsdsdsd.png)
- Click on `Edit` to add the following Pipeline Stages;
    - The `Testing Stage`
    - The `Manual Approval Stage`  
    - The `Prod Deployment Stage`
- Click on `Add stage`
- **NOTE:** Make sure to add this Stage in between the `Build` and `Desploy` Stage
![SASTStage!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-07%20at%201.33.14%20PM.png)
- Stage name: `SAST-Test-SonarCloud`
- Click on `Add action group`
  - Action name: `SAST-Test-SonarCloud`
  - Action provider: `AWS CodeBuild`
  - Region: `Select your project region`
  - Input artifact: `SourceArtifact`
  - Project name: `Select Your SonarCloud CodeBuild Job/Project`
  - Build type: `Single build`
  - Click `Done`

  - Click on `Done` again

## 17) Add The Manual Approval Stage (To Achieve Continuous Delivery To Production)
- Click on `Add stage`
![AddMA!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-07%20at%202.07.16%20PM.png)
- Stage name: `Manual-Approval`

- Click on `Add action group`
![AddMA!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-06%20at%202.00.26%20PM.png)
  - Action name: `Manual-Approval`
  - Action provider: `Manual approval`
  - Click `Done`

  - Click on `Done` again

## 18) Add The Deploy To Production Stage With CodeDeploy
- Click on `Add stage`
![DeployProd!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-06%20at%202.10.51%20PM.png)
- Stage name: `Deploy-Prod`
- Click on `Add action group`
  - Action name: `Deploy-Prod`
  - Action provider: `AWS CodeDeploy`
  - Region: `Select your project region`
  - Input artifact: `BuildArtifact`
  - Application name: `Java-Webapp-CodeDeploy-Application`
  - Deployment group: `Java-Webapp-CodeDeploy-Prod-DG`
  - Click `Done`

  - Click on `Done` to save changes
  - `SCROLL UP` and Click on `SAVE`
  - Click `SAVE`

### 19) RE-RUN YOUR PIPELINE and CONFIRM THE APP IS AVAILABLE IN STAGING ENV BEFORE APPROVING PRODUCTION
- CLICK on `Release Change`
![ReRunPipeline!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-09%20at%2012.45.43%20PM.png)

#### 19.1) Test To The Application Running In The Staging Environment
* Navigate to EC2 
* Copy the Public IP Addresses of the `Stage Instance` and Try Accessing the Application
* URL: http://INSTANCE_PUBLIC_IP:8080/javawebapp 
![WebApp!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/user_login_application_test_successful.png)

### REVIEW AND APPROVE PRODUCTION DEPLOYMENT
- Once you Confirm that The Application is working as Expected...
- You Then `APPROVE PROD` Deployment
![SuccessPipeResults!](https://lucid.app/publicSegments/view/747b2c4f-4f8b-4e1b-b83e-377c91a09cd8/image.png)

### 20) REVIEW ALL JOBS (Whle The Pipeline Is Running)
- Go through the *`CodeArtifact Downloaded Dependencies`*
- Go through the *`CodeBuild Build & Test Job Outputs`*
- Go through the *`CodeDeploy Stage & Prod Prod Deployment Results`*
- Go through the *`SonarCloud Project/Analysis` etc*

#### 20A) CodeArtifact Maven Project Repository
i) CodeArtifact `maven-central-store` (These Dependencies Where All Downloaded From `Maven Central` and Stored Here)
![CodeArtifact!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-09%20at%203.35.20%20AM.png)

ii) CodeBuild Project Logs (Build and Test Jobs)
![CodeArtifact!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-09%20at%2012.58.45%20PM.png)

#### 20B) CodeBuild Build Job Results
![CodeBuildBuildJob!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-09%20at%203.12.18%20AM.png)

#### 20C) CodeBuild SonaCloud SAST Job Results
![CodeBuildSASTJob!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-09%20at%203.17.50%20AM.png)

#### 20D) SonaCloud SAST Test Results
![CodeBuildSASTtestResults!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-09%20at%203.21.56%20AM.png)

#### 20E) CodeDeploy Deployment Results (Stage&Prod)
- Navigate to `CodeDeploy`
![CodeDeploy!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-09%20at%203.28.15%20AM.png)

### 21) CONFIRM THAT THE APPLICATION VALIDATE TEST PASSED
![CodeDeploy!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/Screen%20Shot%202023-10-09%20at%209.26.28%20AM.png)

### 22) TEST ACCESS TO THE APPLICATION
* Navigate to EC2 
* Copy the Public IP Addresses of the Instances and Try Accessing the Application
* URL: http://INSTANCE_PUBLIC_IP:8080/javawebapp 
![WebApp!](https://github.com/awanmbandi/realworld-cicd-pipeline-project/blob/zdocs/images/user_login_application_test_successful.png)

## 👨‍💻😃 CONGRATULATIONS TEAM!! CONGRATULATIONS TEAM!! 👨‍💻😃



