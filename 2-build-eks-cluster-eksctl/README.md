# Building your First EKS Cluster! - Lab 2

## Clone the service repos

1. Clone the service repos:

    ```bash
    cd ~/environment
    git clone https://github.com/aws-containers/ecsdemo-frontend.git
    git clone https://github.com/aws-containers/ecsdemo-nodejs.git
    git clone https://github.com/aws-containers/ecsdemo-crystal.git
    ```
## Install and verify the eksctl bianaries

1. Download and install the eksctl binary:

    ```bash
    curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
    sudo mv -v /tmp/eksctl /usr/local/bin
    ```

2. Confirm the eksctl command works:

    ```bash
    eksctl version
    ```

3. Enable eksctl bash-completion

    ```bash
    eksctl completion bash >> ~/.bash_completion
    . /etc/profile.d/bash_completion.sh
    . ~/.bash_completion
    ```
## Deploy the cluster leveraging eksctl

1. Create an eksctl deployment file (eksworkshop.yaml) which will be used to create your cluster using the following syntax:

    ```bash
    cat << EOF > eksworkshop.yaml
    ---
    apiVersion: eksctl.io/v1alpha5
    kind: ClusterConfig

    metadata:
      name: eksworkshop-eksctl
      region: ${AWS_REGION}
      version: "1.20"

    availabilityZones: ["${AZS[0]}", "${AZS[1]}", "${AZS[2]}"]

    managedNodeGroups:
    - name: nodegroup
      desiredCapacity: 3
      instanceType: t3.small
      ssh:
        enableSsm: true

    # To enable all of the control plane logs, uncomment below:
    # cloudWatch:
    #  clusterLogging:
    #    enableTypes: ["*"]

    secretsEncryption:
      keyARN: ${MASTER_ARN}
    EOF
    ```
3. Use the file you created previously as input to create your first EKS cluster

    ```bash
    eksctl create cluster -f eksworkshop.yaml
    ```
4. Confirm your eks worker nodes were created

    ```bash
    kubectl get nodes # if we see our 3 nodes, we know we have authenticated correctly
    ```
5. Export the worker role name for use throughout the workshop

    ```bash
    STACK_NAME=$(eksctl get nodegroup --cluster eksworkshop-eksctl -o json | jq -r '.[].StackName')
    ROLE_NAME=$(aws cloudformation describe-stack-resources --stack-name $STACK_NAME | jq -r '.StackResources[] | select(.ResourceType=="AWS::IAM::Role") | .PhysicalResourceId')
    echo "export ROLE_NAME=${ROLE_NAME}" | tee -a ~/.bash_profile
    ```
## Congratulations!
   You now have a fully working Amazon EKS cluster that is ready to use! 
   