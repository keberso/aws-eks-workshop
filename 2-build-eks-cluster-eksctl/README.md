# Building your First EKS Cluster! - Lab 2

## Clone the service repos
   
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