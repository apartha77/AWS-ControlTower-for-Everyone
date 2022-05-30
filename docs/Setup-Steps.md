# AWS-ControlTower-for-Everyone - Set-up Steps

## Website

Website Link [https://apartha77.github.io/AWS-ControlTower-for-Everyone](https://apartha77.github.io/AWS-ControlTower-for-Everyone)


I will explain the steps involved in setting up a Greenfield or Browfield AWS Control Tower Implementation with CfCT and AFT. 
### Follow the steps and read instructions (tips & tricks)
1. 

### Follow the steps and read instructions (tips & tricks)
1. Configure AFT - Accounts: There are activities in CT Managment account and then in AFT Managment Account
- CT Managment Account - 
- AFT Management Account - 
    1. Each account is provisioned via **account request repository**. This stage runs in the AFT management account.
    2. **Optional customizations** are run in the AFT Management account as a part of provisioning (referred to as the (provisioning framework).
    3. **Global customizations** are run in each vended account. This stage runs in the pipeline that’s set up for each vended account.
    4. **Optional Account customizations** are run in each vended account, if they were referenced in the initial account provisioning request.

- From inside CT Managment Account - 
2. Create AFT Management Account - Login with AWS SSO role with **Administrator access** to create the AFT Managment Account under existing or create new OU e.g., OU- Infrastructure
3. AFT is deployed with a Terraform module, available in the [AFT repository](https://github.com/aws-ia/terraform-aws-control_tower_account_factory/tree/main). AFT Terraform module must be called using AWS credentials with **Administrator access** in AWS Control Tower management account.
4. Setup Cloud9 environment to run the Terraform Module for AFT. Now, to setup the Cloud9 environment run the clone [GitRepo](https://github.com/aws-samples/aft-workshop-sample.git) select the branch "aft-Cloud9) visit the [Repo](https://github.com/aws-samples/aft-workshop-sample/tree/aft-cloud9). This will create an Cloud9 environment with Terraform installed. 
5. Open Cloud9 IDE, stop AWS temporary credentials from prefrence--> AWS Settings-->Credentials. Then check EC2 IAM Instance role is used ``` aws sts get-caller-identity --query Arn | grep cloud9-aft-role -q && echo "IAM role valid" || echo "IAM role NOT valid" ``` the out put should come as IAM role valid, else debug. 
6. Bootstrap AFT Deployment - deploy AFT module, which will launch the ***AFT pipeline into AFT Management account***. This will use Terraform Module [module "aft_pipeline"](https://github.com/aws-ia/terraform-aws-control_tower_account_factory) and require to **"main.tf"** file with account details of master, AFT, audit, log etc. This will create required pipelines, repositories, ssm parameters etc., in the AFT Managment Account. Check the required repositories, ssm params, pipelines etc., were created. 
7. Grant **Account Factory portfolio** (in CT Managment Account) to **AFT-Management role**. AFT process the account provisioning requests from the AFT Management account. To do that, AFT requires cross-account access to the AWS Control Tower account factory portfolio in AWS Service Catalog. To enable AFT to access portfolio in the AWS Control Tower management account, we are going to share it to **AWSAFTExecution** IAM role. If this role not granted permission to call service catalog then AFT account will not be provisiond. 








---
[Home](index.md)  
[Next](CfCT.md)  
[Previous](Introduction.md)  
[Top](Setup-Steps.md)