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