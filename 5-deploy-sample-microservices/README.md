# Deploy Sample Microservices - Lab 5

In this file, we are defining our deployment. Remember earlier today, we said that Kubernetes is declarative. In this deployment file, we declare what we want to deploy. We then write this definition to the Kubernetes api using kubectl and Kubernetes works to both get and keep us in the state which we define. 

![role-1](./images/role-1.png)

Remember from our earlier discussion, that a deployment declares what our application should look like and a service declares how other things in the cluster gain access to the application.

## Deploy the NodeJS Backend API/Application

1. Change directories to where our deployment and service definitions for the NodeJS application and service are located:

    ```bash
    cd ~/environment/ecsdemo-nodejs
    ```
2. Run the following commands to deploy the NodeJS service:

    ```bash
    kubectl apply -f kubernetes/deployment.yaml
    kubectl apply -f kubernetes/service.yaml
    ```
3. Run the following commands to see the deployment status:

    ```bash
    kubectl get deployment ecsdemo-nodejs
    ```
## Deploy Cyrstal Backend API/Application

1. Change directories to where our deployment and service definitions for the NodeJS application and service are located:

    ```bash
    cd ~/environment/ecsdemo-crystal
    ```

2. Run the following commands to deploy the Crystal service:

    ```bash
    kubectl apply -f kubernetes/deployment.yaml
    kubectl apply -f kubernetes/service.yaml
    ```
3. Run the following commands to see the deployment status:

    ```bash
    kubectl get deployment ecsdemo-crystal
    ```

## Contgratulations!
   You have deployed the Kubernetes Dashboard! 
