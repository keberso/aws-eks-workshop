# Console Credentials - Lab 3

This step is optional, as nearly all of the workshop content is CLI-driven. But, if you’d like full access to your workshop cluster in the EKS console this step is recommended.

The EKS console allows you to see not only the configuration aspects of your cluster, but also to view Kubernetes cluster objects such as Deployments, Pods, and Nodes. For this type of access, the console IAM User or Role needs to be granted permission within the cluster.

By default, the credentials used to create the cluster are automatically granted these permissions. Following along in the workshop, you’ve created a cluster using temporary IAM credentials from within Cloud9. This means that you’ll need to add your AWS Console credentials to the cluster.

## Import your EKS Console credentials to your new cluster

IAM Users and Roles are bound to an EKS Kubernetes cluster via a ConfigMap named aws-auth. We can use eksctl to do this with one command.

You’ll need to determine the correct credential to add for your AWS Console access. Ask your instructor to help you identify the user or role used for console access.

1. Identify the ARN of the user or role that you are accessing the console with. If you need assistance, ask your instructor

2. With your ARN in hand, you can issue the command to create the identity mapping within the cluster.

    ```bash
    eksctl create iamidentitymapping --cluster eksworkshop-eksctl --arn ${rolearn} --group system:masters --username admin
    ```
    Note: The ARN of an IAM role can't include the path. The format of the value you provide must be arn:aws:iam::111122223333:role/role-name

2. Verify your entry in the AWS auth map

    ```bash
    kubectl describe configmap -n kube-system aws-auth
    ```
