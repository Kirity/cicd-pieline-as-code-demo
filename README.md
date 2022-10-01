# Summary

[Introduction](https://github.com/Kirity/cicd-pieline-as-code-demo/blob/master/README.md#introduction)



# Introduction
In this project CICD pipeline is created with CloudFormation templates for a AWS Lambda function.
To achive this the following tools, services are used.

[GitHub](https://github.com/): as a version control system and code repository.

[AWS CodePipeline](https://aws.amazon.com/codepipeline/): AWS native CICD orchestrating service.

[AWS CodeBuild](https://aws.amazon.com/codebuild/): AWS native fully managed build sevice

[AWS CodeDeploy](https://aws.amazon.com/codedeploy/): AWS native fully managed deployment service.

[AWS CloudFormation](https://aws.amazon.com/cloudformation/): AWS native infrastructure as code(Iac) service.

# Prerequisite
1. An existing AWS account.
2. Existing codebase for the Lambda function. In this example an existing project [print-s3-buckets-lambda](https://github.com/Kirity/print-s3-buckets-lambda) is used.

# Architecture

# Steps to create the pipeline
### Step 1: create a connection to GitHub project

To link the GitHub repository with the AWS CodePipeline, a connection with required persmission needs to be established beforehand.
1. Go to AWS CodePipeline --> Settings(on the left pane) --> Connections
2. In this page click on "Create pipeline" --> will present a new page
3. In this page "select the GitHub" as the provider
4. Enter the connection name. Example "Your_name_GitHub_Connection" --> Click on "Connect to GitHub" --> this will redirect to a popup window.
6. In the new window key in your GitHub credentials and allow access to all your GitHub projects(Also optionally you can give permission on only one project too).
7. A new connection will be created between your account AWS and to your repos in GitHub.

One completed successfully it looks as below

 ![image](https://user-images.githubusercontent.com/15073157/193419783-be140835-2a3b-45cf-acc5-11935cb9f59b.png)

### Step 2: provision the needed resources
To provision a CodePipe line some IAM roles and a S3 bucket are needed. The needed resources are in ```infrastructure/cicd-pipeline-iam-roles-policies.yaml```.

The exact resources are:

```CODEBUILD_CICD_ROLE```:  IAM role need for the CodeBuild service.

```CLOUDFORMATION_CICD_ROLE```: IAM role need for the CloudFomation service.

```CODEPIPELINE_CICD_ROLE```: IAM role need for the CodePipeline service.

```cicd-pipeline-bucket-demo```: a S3 bucket needed to store the build articacts.

Provisioning can be done in two ways: CloudFormation UI or AWS CLI

CLI commond to run is 
```aws cloudformation create-stack --template-body file://cicd-pipeline-iam-roles-policies.yaml --stack-name cicd-pieline-iam-roles-policies --capabilities CAPABILITY_NAMED_IAM```

The resources created are

![image](https://user-images.githubusercontent.com/15073157/193423493-224cf9a8-26ca-4e99-b6c8-208744b6d99e.png)

By the end of this step needed resources are provisioned successfully

### Step 3: provision the pipleline 

In this step we provision the actual pipeline.

The code for this in the file ```cicd-print-s3-buckets-lambda-pipeline.yaml```. This file contains the below resources:

```AWS::CodeBuild::Project```: we need environment to build and run the commands needed to deploy the Lambda. 
- A Linux container evn is provisioned with AWS provided standard linux image
- Env variables are defined to inject the values at run time rathe than hard coding the things
- Service role used is ```CODEBUILD_CICD_ROLE```, which was created in Step 2
- The source for this project will be from CodePipeline
- BuildSpec is the file in which build commands will be present and the CodeBuild will execute them in the provisioned env.

```AWS::CodePipeline::Pipeline```: CodePipe line is a umberalla service that can define the source code connections, build stage, and deploy stage.
- Actions-Source: 
  - This is the first stage of the pipeline, where the path to the source code is defined. The connection created in Step 1 is used here.
  - Full repo path and the branch-name(on which pipeline should be triggered) are provided

![image](https://user-images.githubusercontent.com/15073157/193424548-57f83432-5ecb-4d50-aadf-0e1b7c4bd661.png)

- Actions-BuildProject: 
  - This is the second stage of the pipeline, where the actual build will take place.
  - The above created CodeBuild(in Step 3) project is set as the build path.

![image](https://user-images.githubusercontent.com/15073157/193424566-7de26187-31da-4d6f-a660-f2e2959a3ac6.png)

- Actions-Deploy:   
  - This is the third stage of the pipeline, where the deployment will take place.
  - In this case deployment is with CloudFormation.
  - Stack name, role to use by the CloudFormation, template path and needed config details are provided.

![image](https://user-images.githubusercontent.com/15073157/193424576-f2cb2e36-5690-468e-86e8-d5f765143e81.png)

There are two ways to execute the template file one with CloudForamtion UI and with CLI. Command to run in CLI is ```aws cloudformation create-stack --template-body file://cicd-print-s3-buckets-lambda-pipeline.yaml --stack-name cicd-print-s3-buckets-lambda-pipeline```

The view from CloudFormation service

![image](https://user-images.githubusercontent.com/15073157/193424726-3a316c5f-aa2e-4d61-9868-2cd34addb174.png)


The view the pipeline from CodePipeline service

![image](https://user-images.githubusercontent.com/15073157/193424658-50332c58-e05a-40a0-a6d9-305da3785c7b.png)


A detailed look into the pipeline

![image](https://user-images.githubusercontent.com/15073157/193424671-2a95f33e-feea-407c-b222-3b6b96ed19de.png)





