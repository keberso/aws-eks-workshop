# CI/CD with AWS CodePipeline - Lab 8 (Optional)

Continuous integration (CI) and continuous delivery (CD) are essential for deft organizations. Teams are more productive when they can make discrete changes frequently, release those changes programmatically and deliver updates without disruption.

In this module, we will build a CI/CD pipeline using AWS CodePipeline. The CI/CD pipeline will deploy a sample Kubernetes service, we will make a change to the GitHub repository and observe the automated delivery of this change to the cluster.

## Signup for a GitHub Account

1. [Visit GitHub](https://github.com/)

2. Click "Signup"

    ![role-1](./images/role-1.png)

3. Enter your e-mail and select "Continue"

    ![role-2](./images/role-2.png)

3. Create a password and select "Continue"

    ![role-3](./images/role-3.png)

4. Enter and username and select "Continue"

    ![role-4](./images/role-4.png)

5. Type "n" to not recive product updates and annoucements and select "Continue"

    ![role-5](./images/role-5.png)

6. Select "Create account"

    ![role-6](./images/role-6.png)

Note: You will recive an launch code to the e-mail you specified. Enter the code.

7. Select "Just me", "Student" and select "Continue"

    ![role-7](./images/role-7.png)

    Note: Skip all personalization’s.

## Creat IAM Role

In an AWS CodePipeline, we are going to use AWS CodeBuild to deploy a sample Kubernetes service. This requires an AWS Identity and Access Management (IAM) role capable of interacting with the EKS cluster.

In this step, we are going to create an IAM role and add an inline policy that we will use in the CodeBuild stage to interact with the EKS cluster via kubectl.

1. Create the role by running the following command:

    ```bash
    cd ~/environment

    TRUST="{ \"Version\": \"2012-10-17\", \"Statement\": [ { \"Effect\": \"Allow\", \"Principal\": { \"AWS\": \"arn:aws:iam::${ACCOUNT_ID}:root\" }, \"Action\": \"sts:AssumeRole\" } ] }"

    echo '{ "Version": "2012-10-17", "Statement": [ { "Effect": "Allow", "Action": "eks:Describe*", "Resource": "*" } ] }' > /tmp/iam-role-policy

    aws iam create-role --role-name EksWorkshopCodeBuildKubectlRole --assume-role-policy-document "$TRUST" --output text --query 'Role.Arn'

    aws iam put-role-policy --role-name EksWorkshopCodeBuildKubectlRole --policy-name eks-describe --policy-document file:///tmp/iam-role-policy
    ```

    Now that we have the IAM role created, we are going to add the role to the aws-auth ConfigMap for the EKS cluster.

    Once the ConfigMap includes this new role, kubectl in the CodeBuild stage of the pipeline will be able to interact with the EKS cluster via the IAM role.

2. To add the role to the aws-auth ConfigMap for your cluster, run the following command:

    ```bash
    cd ~/environment

    ROLE="    - rolearn: arn:aws:iam::${ACCOUNT_ID}:role/EksWorkshopCodeBuildKubectlRole\n      username: build\n      groups:\n        - system:masters"

    kubectl get -n kube-system configmap/aws-auth -o yaml | awk "/mapRoles: \|/{print;print \"$ROLE\";next}1" > /tmp/aws-auth-patch.yml

    kubectl patch configmap/aws-auth -n kube-system --patch "$(cat /tmp/aws-auth-patch.yml)"
    ```