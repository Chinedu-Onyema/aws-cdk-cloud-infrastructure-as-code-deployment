# Provisioning AWS Infrastructure using AWS Cloud Development Kit (AWS CDK) in Python

This is a step-by-step repository guide on setting up the AWS Cloud Development Kit (AWS CDK) with Python inside GitHub Codespaces, bootstrapping the target environment, synthesizing CloudFormation templates, and deploying SQS infrastructure.

### PDF GUIDE: [AWS CLOUD DEVELOPMENT KIT.pdf](https://github.com/user-attachments/files/32160001/AWS.CLOUD.DEVELOPMENT.KIT.pdf)

### WATCH VIDEO WALKTHROUGH HERE: https://youtu.be/ZKAq915rfxE


## OVERVIEW & KEY CONCEPTS
The AWS Cloud Development Kit (AWS CDK) is an open-source software development framework to define AWS cloud infrastructure using familiar programming languages (Python, TypeScript, Java, C#, or Go). 
Instead of writing raw YAML or JSON templates manually, CDK allows Cloud and DevOps engineers to write code that programmatically synthesizes into native AWS CloudFormation templates.



## PREREQUISITES

An active AWS Account with programmatic administrative access (Access Key ID & Secret Access Key).

A GitHub Account to launch GitHub Codespaces.


### STEP-BY-STEP IMPLEMENTATION

### Phase 1: GitHub Codespaces & CLI Setup

1) Create a GitHub repository named aws-cdk-cloud-deployment or use any name of your choice and open a GitHub Codespace on main.


2) Install the AWS CDK CLI globally using npm

<PRE>npm install -g aws-cdk</PRE>
<PRE>cdk --version</PRE>


3) Install the AWS CLI v2 inside your Codespace:

<PRE>curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"</PRE>
<PRE>unzip awscliv2.zip</PRE>
<PRE>sudo ./aws/install</PRE>
<PRE>aws --version</PRE>
<PRE>rm awscliv2.zip</PRE>


### Phase 2: AWS Credentials & Environment Bootstrapping

1) Authenticate with your AWS Account:

<PRE>aws configure</PRE>

(Enter your AWS Access Key ID, AWS Secret Access Key, default region e.g., eu-north-1, and json output format).


2) Verify authentication identity:

<PRE>aws sts get-caller-identity</PRE>


3) Bootstrap the AWS CDK Environment:
Run the bootstrapping process to deploy the required CDKToolkit stack into your AWS Account:

<PRE>cdk bootstrap aws://<YOUR_ACCOUNT_ID>/<YOUR_REGION></PRE>
<PRE>cdk bootstrap aws://140023390772/eu-north-1</PRE>


4) What does cdk bootstrap do?

It creates a CloudFormation stack named CDKToolkit provisioning:

An S3 Bucket for hosting CDK assets and code packages.

An ECR Repository for container images (if applicable).

IAM Roles granting CDK the permissions to manage resources on your behalf.



### Phase 3: Python CDK Project Initialization

1) Create and enter a new project folder:

<PRE>mkdir cdk-python && cd cdk-python</PRE>


2) Initialize an AWS CDK application scaffolding with Python:

<PRE>cdk init app --language python</PRE>


### Phase 4: Understanding CDK Project FilesFile / FolderFunction & Description

1) app.pyThe main application entry point that instantiates stacks and synthesizes templates.cdk.json
   Configuration file telling CDK how to execute the app (e.g., "app": "python app.py").

2) cdk_python/Core project module containing the stack definitions (e.g., cdk_python_stack.py).

3) requirements.txtPython dependencies manifest (e.g., aws-cdk-lib, constructs).

4) tests/Directory housing unit tests (using pytest) to assert stack outputs prior to deployment.

5) .venv/Isolated Python virtual environment containing specific runtime libraries.



### Phase 5: Stack Definition & Infrastructure Code

1) Open cdk_python/cdk_python_stack.py located in this repo and update the construct definition to define an Amazon SQS Queue:


### Phase 6: Virtual Environment Setup & Dependencies

1) Activate the project-specific virtual environment and install project packages:

<PRE>source .venv/bin/activate</PRE>
<PRE>which python3</PRE>
<PRE>pip install --upgrade pip</PRE>
<PRE>pip install -r requirements.txt</PRE>


### Phase 7: Synthesis & Deployment

1) Synthesize the CloudFormation Template:
Convert Python code into a CloudFormation template artifact (saved inside cdk.out/):

<PRE>cdk synth</PRE>


2) Inspect Infrastructure Drift & Changes:
Compare the local CDK state against the running AWS environment:

<PRE>cdk diff</PRE>


3) Deploy the Infrastructure:
Provision the stack into your AWS account:

<PRE>cdk deploy</PRE>


4) Verification:

Open the AWS CloudFormation Console in your target region (eu-north-1).

Confirm that CdkPythonStack has completed provisioning with status CREATE_COMPLETE.

Open the Amazon SQS Console to view the newly created SQS queue.



### Phase 8: Immutable Resource Replacement (Queue Renaming)

1) Certain AWS resource logical ID changes force full resource replacement. To rename the queue construct:

2) Edit cdk_python/cdk_python_stack.py and update the construct ID:

3) Evaluate the deployment diff:

<PRE>cdk diff</PRE>

Output indicates that the old queue (CdkPythonQueue) will be destroyed and replaced by the new queue (QueueInfrastructure).

4) Deploy the replacement:

<PRE>cdk deploy</PRE>


## INFRASTRUCTURE TEARDOWN

1) To destroy the SQS queue stack and remove all provisioned CloudFormation resources:

<PRE>cdk destroy</PRE>

Confirm stack removal when prompted in the terminal.







