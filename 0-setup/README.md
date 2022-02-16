# Project Setup - Lab 0

This lab prepares your workspace to interact with AWS EKS by installing the required utilities. 

## Attach EC2 Role to Cloud9 Workspace

1. Disable Cloud9 Temporary Credentials: ![role-1](./images/role-1.png)

2. Navigate to IAM:

    ![role-6](./images/role-6.png)

3. Select "Roles":

    ![role-7](./images/role-7.png)

4. Select "Create Role":

    ![role-8](./images/role-8.png)

5. Select "AWS Service" and "EC2":

    ![role-9](./images/role-9.png)

6. Filter on  "Administrator" and select "AdministratorAccess":

    ![role-10](./images/role-10.png)

7. Name the role "eksworkshop-admin" and select "Create role":

    ![role-11](./images/role-11.png)

8. Attach EC2 Role to Cloud9:

    Click Manage EC2 Instance in Cloud9 ![role-3](./images/role-3.png)

    Click Actions -> Security -> Modify IAM Role ![role-4](./images/role-4.png)

    Attach the eksworkshop-admin role created previously to the instance and select "Save" ![role-5](./images/role-5.png)

9. Return to Cloud9 Workspace in the AWS Console.
