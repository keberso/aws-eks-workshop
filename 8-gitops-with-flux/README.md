# GitOps with Weave Flux - Lab 8

GitOps, a term coined by Weaveworks, is a way to do continuous delivery. Git is used as single source of truth for deploying into your cluster. This is easy for a development team as they are already familiar with git and do not need to know other tools. Weave Flux is a tool that runs in your Kubernetes cluster and implements changes based on monitoring Git and image repositories.

In this module, we will create a Docker image build pipeline using AWS CodePipeline for a sample application in a GitHub repository. We will then commit Kubernetes manifests to GitHub and monitor Weave Flux managing the deployment.

Below is a diagram of what will be created:

Sample Output:

![role-1](./images/role-1.png)

## Install and validate Prerequisites

1. We will use helm to install Weave Flux and a sample Helm chart. Check to see if helm is installed:

    ```bash
    helm version
    ```

    AWS CodePipeline and AWS CodeBuild both need AWS Identity and Access Management (IAM) service roles to create a Docker image build pipeline.

    In this step, we are going to create an IAM role and add an inline policy that we will use in the CodeBuild stage to interact with the EKS cluster via kubectl.

2. Create the required bucket and roles with the commands below:

    ```bash
    # Use your account number below
    ACCOUNT_ID=$(aws sts get-caller-identity | jq -r '.Account')
    aws s3 mb s3://eksworkshop-${ACCOUNT_ID}-codepipeline-artifacts

    cd ~/environment

    wget https://eksworkshop.com/intermediate/260_weave_flux/iam.files/cpAssumeRolePolicyDocument.json

    aws iam create-role --role-name eksworkshop-CodePipelineServiceRole --assume-role-policy-document file://cpAssumeRolePolicyDocument.json 

    wget https://eksworkshop.com/intermediate/260_weave_flux/iam.files/cpPolicyDocument.json

    aws iam put-role-policy --role-name eksworkshop-CodePipelineServiceRole --policy-name codepipeline-access --policy-document file://cpPolicyDocument.json

    wget https://eksworkshop.com/intermediate/260_weave_flux/iam.files/cbAssumeRolePolicyDocument.json

    aws iam create-role --role-name eksworkshop-CodeBuildServiceRole --assume-role-policy-document file://cbAssumeRolePolicyDocument.json 

    wget https://eksworkshop.com/intermediate/260_weave_flux/iam.files/cbPolicyDocument.json

    aws iam put-role-policy --role-name eksworkshop-CodeBuildServiceRole --policy-name codebuild-access --policy-document file://cbPolicyDocument.json

    ```