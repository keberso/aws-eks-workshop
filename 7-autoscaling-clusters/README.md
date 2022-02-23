# Autoscaling Clusters - Lab 7

Cluster Autoscaler for AWS provides integration with Auto Scaling groups. It enables users to choose from four different options of deployment:

One Auto Scaling group
Multiple Auto Scaling groups
Auto-Discovery
Control-plane Node setup
Auto-Discovery is the preferred method to configure Cluster Autoscaler. [Click here for more information.](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler/cloudprovider/aws)

Cluster Autoscaler will attempt to determine the CPU, memory, and GPU resources provided by an Auto Scaling Group based on the instance type specified in its Launch Configuration or Launch Template.

## Configure the Auto Scaling group by setting the minimum, maximum and desired capacity. When we created the cluster we set these settings to 3.

1. Describe the current auto scaling group:

    ```bash
    aws autoscaling \
    describe-auto-scaling-groups \
    --query "AutoScalingGroups[? Tags[? (Key=='eks:cluster-name') && Value=='eksworkshop-eksctl']].[AutoScalingGroupName, MinSize, MaxSize,DesiredCapacity]" \
    --output table
    ```
    Sample Output:
    ![role-1](./images/role-1.png)

2. Increase the maximum capacity to 4 instances and check the new values:
    
    ```bash
    # we need the ASG name
    export ASG_NAME=$(aws autoscaling describe-auto-scaling-groups --query "AutoScalingGroups[? Tags[? (Key=='eks:cluster-name') && Value=='eksworkshop-eksctl']].AutoScalingGroupName" --output text)

    # increase max capacity up to 4
    aws autoscaling \
      update-auto-scaling-group \
      --auto-scaling-group-name ${ASG_NAME} \
      --min-size 3 \
      --desired-capacity 3 \
      --max-size 4

    # Check new values
    aws autoscaling \
      describe-auto-scaling-groups \
      --query "AutoScalingGroups[? Tags[? (Key=='eks:cluster-name') && Value=='eksworkshop-eksctl']].[AutoScalingGroupName, MinSize, MaxSize,DesiredCapacity]" \
      --output table
    ```
## IAM Roles for Service Accounts (IRSA)

Note: [Click here](https://www.eksworkshop.com/beginner/110_irsa/) if you would like more information for IAM Roles for Service Accounts.

With IAM roles for service accounts on Amazon EKS clusters, you can associate an IAM role with a Kubernetes service account. This service account can then provide AWS permissions to the containers in any pod that uses that service account. With this feature, you no longer need to provide extended permissions to the node IAM role so that pods on that node can call AWS APIs.

1. Associate IAM OIDC Provider with your Cluster:

    ```bash
    eksctl utils associate-iam-oidc-provider \
    --cluster eksworkshop-eksctl \
    --approve
    ```
2. Creating an IAM policy for your service account that will allow your CA pod to interact with the autoscaling groups:

    ```bash
    mkdir ~/environment/cluster-autoscaler

    cat <<EoF > ~/environment/cluster-autoscaler/k8s-asg-policy.json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Action": [
                    "autoscaling:DescribeAutoScalingGroups",
                    "autoscaling:DescribeAutoScalingInstances",
                    "autoscaling:DescribeLaunchConfigurations",
                    "autoscaling:DescribeTags",
                    "autoscaling:SetDesiredCapacity",
                    "autoscaling:TerminateInstanceInAutoScalingGroup",
                    "ec2:DescribeLaunchTemplateVersions"
                ],
                "Resource": "*",
                "Effect": "Allow"
            }
        ]
    }
    EoF

    aws iam create-policy   \
        --policy-name k8s-asg-policy \
        --policy-document file://~/environment/cluster-autoscaler/k8s-asg-policy.json
    ```

3. Create and IAM role for the cluster-autoscaler Service Account in the kube-system namespsce:

    ```bash
    eksctl create iamserviceaccount \
        --name cluster-autoscaler \
        --namespace kube-system \
        --cluster eksworkshop-eksctl \
        --attach-policy-arn "arn:aws:iam::${ACCOUNT_ID}:policy/k8s-asg-policy" \
        --approve \
        --override-existing-serviceaccounts
    ```
4. Verify that your service account with the ARN of the IAM role is annotated:

    ```bash
    kubectl -n kube-system describe sa cluster-autoscaler
    ```
    Sample Output:
    ![role-1](./images/role-2.png)