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

3. (Optional) Execute the [launch-setup.sh](./launch-setup.sh) script in your workspace only if you don't want to manually type the commands above:

    ```bash
    chmod +x ./launch-setup.sh
    ```

    ```bash
    ./launch-setup.sh
    ```

    ```bash
    eksctl create cluster -f airports.yaml
    ```

    ```bash
    kubectl get nodes
    ```
