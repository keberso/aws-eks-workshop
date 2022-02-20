# AWS Elastic Kubernetes Service (EKS) Workshop

## [Building your Development Environment - Lab 0](./0-build-your-development-environment)

In this lab we will build your development envivronment using AWS Cloud9, a cloud-based integrated development environment (IDE) that lets you write, run and debug your code with just a browser.

## [Installing Kubernetes Tools - Lab 1](./1-install-kubernetes-tools)

In this lab, we will install Kubernetes tools into your Cloud9 Development Enviornment needed for the rest of the labs.

## [Building your First EKS Cluster - Lab 2](./2-build-eks-cluster-eksctl)

eksctl is a tool jointly devloped by AWS and Weaveworks that automates much of the experience of creating EKS Clusters. In this module, we will use eksctl to launch and configure your EKS cluster and nodes.

## [Update Console Credentials - Lab 3 (Optional)](./3-update-console-credentials)

This setp is optional, as nearly all of the workshop content is CLI-drive. However, if you'd like full access to your workshop cluster int he EKS console, this step is recomended.

The EKS console allows you to see not only the configuration aspects of your cluster, but also to view Kubernetes cluster objects such as Deployments, Pods, and Nodes. For this type of access, the console IAM User or Role needs to be granted permission within the cluster.