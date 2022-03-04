# Install Kubernetes Tools - Lab 1

This lab installs the tools into your development environment required for the remaining labs. 

1. Install kubectl

    ```bash
    sudo curl --silent --location -o /usr/local/bin/kubectl \
      https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
    sudo chmod +x /usr/local/bin/kubectl
    ```
    What is kubectl? The Kubernetes command-line tool, kubectl, allows you to run commands against Kubernetes clusters. You can use kubectl to deploy applications, inspect and manage cluster resources, and view logs. ... kubectl is installable on a variety of Linux platforms, macOS and Windows.

2. Update the awscli

    ```bash
      curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
      unzip awscliv2.zip
      sudo ./aws/install
    ```
    The AWS Command Line Interface (CLI) is a unified tool to manage your AWS services. With just one tool to download and configure, you can control multiple AWS services from the command line and automate them through scripts.

3. Install jq, envsubst (from GNU gettext utilities) and bash-completion

    ```bash
    sudo yum -y install jq gettext bash-completion moreutils
    ```
    Jq is a Linux command line utility that allows you to extract data from JSON documents. Bash completion is a functionality through which bash helps users type their commands faster and easier and more utils is a collection of useful utilities.

4. Install yq for yaml processing

    ```bash
    echo 'yq() {
      docker run --rm -i -v "${PWD}":/workdir mikefarah/yq "$@"
    }' | tee -a ~/.bashrc && source ~/.bashrc
    ```
    Yq is a lightweight and portable command-line YAML, JSON and XML processor. yq uses jq like syntax but works with yaml files as well as json and xml.

5. Verify the binaries are in the path and executable

    ```bash
    for command in kubectl jq envsubst aws
    do
      which $command &>/dev/null && echo "$command in path" || echo "$command NOT FOUND"
    done
    ```
6. Enable kubectl bash_completion

    ```bash
    kubectl completion bash >>  ~/.bash_completion
    . /etc/profile.d/bash_completion.sh
    . ~/.bash_completion
    ```
7. Set the AWS Load Balancer Controller version

    ```bash
    echo 'export LBC_VERSION="v2.3.0"' >>  ~/.bash_profile
    .  ~/.bash_profile
    ```
8. To ensure temporary credentials aren’t already in place we will remove any existing credentials files and disable AWS managed temporary credentials:

    ```bash
    rm -vf ${HOME}/.aws/credentials
    ```
9. Configure the aws cli with our current region as default:

    ```bash
    export ACCOUNT_ID=$(aws sts get-caller-identity --output text --query Account)
    export AWS_REGION=$(curl -s 169.254.169.254/latest/dynamic/instance-identity/document | jq -r '.region')
    export AZS=($(aws ec2 describe-availability-zones --query 'AvailabilityZones[].ZoneName' --output text --region $AWS_REGION))
    ```
10. Validate that AWS_REGION is set to the us-east-1 region:

    ```bash
    test -n "$AWS_REGION" && echo AWS_REGION is "$AWS_REGION" || echo AWS_REGION is not set
    ```
10. Validate that the your Cloud9 Developer Environment is using the correct IAM Role (Should see IAM Role Valid):

    ```bash
    aws sts get-caller-identity --query Arn | grep eksworkshop-admin -q && echo "IAM role valid" || echo "IAM role NOT valid"
    ```
10. Creat an AWS KMS Customer Managed Key (CMK) to use when encrypting Kubernetes secrets:

    ```bash
    aws kms create-alias --alias-name alias/eksworkshop --target-key-id $(aws kms create-key --query KeyMetadata.Arn --output text)
    export MASTER_ARN=$(aws kms describe-key --key-id alias/eksworkshop --query KeyMetadata.Arn --output text)
    echo "export MASTER_ARN=${MASTER_ARN}" | tee -a ~/.bash_profile
    ```

## Congratulations!
   You now have the tools required for the remaining labs installed and configured in your Cloud9 Development Environment!