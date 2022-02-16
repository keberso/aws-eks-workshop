# Project Setup - Lab 0

This lab prepares your workspace to interact with AWS EKS by installing the required utilities. This configuration assumes you're using AWS Cloud9 and the lab has already been cloned to your Cloud9 Workspace per [here](../README.md).

## Preparation

1. Verify you're in the correct working directory of Lab 0:

    ```text
    cd aws-eks-workshop/0-setup
    ```

2. Execute the [setup.sh](./setup.sh) script on your workspace:

    ```bash
    chmod +x ./setup.sh
    ```

    ```bash
    ./setup.sh
    ```

    The [setup.sh](./setup.sh) script accomplishes the following:

    ```bash
    # Setup SSH Key
    ssh-keygen -t rsa -N "" -f ~/.ssh/id_rsa
    aws ec2 import-key-pair --key-name "eks-workshop" --public-key-material file://~/.ssh/id_rsa.pub
    
    # Install Dependencies
    sudo yum update -y
    sudo yum install -y jq gettext bash-completion moreutils
    curl -o aws-iam-authenticator https://amazon-eks.s3.us-west-2.amazonaws.com/1.21.2/2021-07-05/bin/linux/amd64/aws-iam-authenticator
    sudo mv -v aws-iam-authenticator /usr/local/bin/
    sudo chmod +x /usr/local/bin/aws-iam-authenticator
    
    # Set AWS Environment Variables
    export ACCOUNT_ID=$(aws sts get-caller-identity --output text --query Account) 
    export AWS_REGION=$(curl -s 169.254.169.254/latest/dynamic/instance-identity/document | jq -r '.region') 
    echo "export ACCOUNT_ID=${ACCOUNT_ID}" >> ~/.bash_profile 
    echo "export AWS_REGION=${AWS_REGION}" >> ~/.bash_profile 
    aws configure set default.region "${AWS_REGION}" 
    aws configure get default.region
    
    # Prepare for EC2 Role
    rm -vf "${HOME}/.aws/credentials"
    ```

## Attach EC2 Role to Cloud9 Workspace

1. Disable Cloud9 Temporary Credentials: ![role-1](./images/role-1.png)

2. Navigate to IAM:

    ![role-6](./images/role-6.png)

3. Select "Roles":

    ![role-7](./images/role-7.png)

4. Select "Create Role":

    ![role-8](./images/role-8.png)

5. Select "AWS Service" and "EC2":

    ![role-9](./images/role-9.png)

6. Filter on  "Administrator" and select "AdministratorAccess":

    ![role-10](./images/role-10.png)

7. Name the role "eksworkshop-admin" and select "Create role":

    ![role-11](./images/role-11.png)

8. Attach EC2 Role to Cloud9:

    Click Manage EC2 Instance in Cloud9 ![role-3](./images/role-3.png)

    Click Actions -> Security -> Modify IAM Role ![role-4](./images/role-4.png)

    Attach the eksworkshop-admin role created previously to the instance and select "Save" ![role-5](./images/role-5.png)

9. Return to Cloud9 Workspace in the AWS Console.
