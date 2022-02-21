# Deploy the Kubernetes Dashboard - Lab 4

The official Kubernetes dashboard is not deployed by default, but there are instructions in the [official documentation](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/):

We can deploy the dashboard with the following command:

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
