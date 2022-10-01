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

# Step to create 
### Step 1: create a connection to GitHub project
To link the GitHub repository with the AWS CodePipeline, a connection with required persmission needs to be established beforehand.
1. Go to AWS CodePipeline --> Settings(on the left pane) --> Connections
2. In this page click on "Create pipeline" --> will present a new page
3. In this page "select the GitHub" as the provider
4. Enter the connection name. Example "Your_name_GitHub_Connection" --> Click on "Connect to GitHub" --> this will redirect to a popup window.
6. In the new window key in your GitHub credentials and allow access to all your GitHub projects(Also optionally you can give permission on only one project too).
7. A new connection will be created between your account AWS and to your repos in GitHub.

A the end you see as below
 ![image](https://user-images.githubusercontent.com/15073157/193419729-f9feb50c-6d47-4317-86d0-ea7872c3310b.png)


### Step 1: provisioning of needed resources

### Step 3: provisioning of pipleline itself

