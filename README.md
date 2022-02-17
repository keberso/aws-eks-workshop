# AWS Elastic Kubernetes Service (EKS) Workshop

## [Building our Development Environment - Lab 0](./0-setup)

In this lab we will build your development envivronment using AWS Cloud9, a cloud-based integrated development environment (IDE) that lets you write, run and debug your code with just a browser.

## [Install Kubernetes Tools - Lab 1](./1-install-kubernetes-tools)

In this lab, we will install Kubernetes tools into your Cloud9 Development Enviornment needed for the rest of the labs.

## [Building your First EKS Cluster! - Lab 2](./2-build-eks-cluster-eksctl)

eksctl is a tool jointly devloped by AWS and Weaveworks that automates much of the experience of creating EKS Clusters. In this module, we will use eksctl to launch and configure your EKS cluster and nodes.

## [Containerize Tomcat Web Application - Lab 3](./3-containerize-web-application)

Securely containerize a Java Spring Boot MVC application with Tomcat Servlet for deployment into the cluster. Push containerized application to AWS Elastic Container Registry (ECR).

![airport-data](./3-containerize-web-application/airplane-data/app.png)

## [Deploying Application to EKS Cluster - Lab 4](./4-deploying-application-into-eks)

Deploy the above containerized application from ECR into the newly formed EKS cluster.

## [Update Deployment on EKS Cluster - Lab 5](./5-update-application-deployment)

Update the containerized application by adding additional airports then containerizes a new version for upload to ECR.
