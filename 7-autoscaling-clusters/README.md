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

1. Increase the maximum capacity to 4 instances and check the new values:

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
