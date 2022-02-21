# Deploy Sample Microservices - Lab 5

The official Kubernetes dashboard is not deployed by default, but there are instructions in the [official documentation](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/):

## Deploy the Kubernetes dashboard

1. Deploy the dashboard with the following command:

    ```bash
    export DASHBOARD_VERSION="v2.0.0"
    kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/${DASHBOARD_VERSION}/aio/deploy/recommended.yaml
    ```
2. Since this is deployed to our private cluster, we need to access it  via a proxy. kube-proxy is available to proxy our requests to the dashboard service. In your workspace, run the following command:

    ```bash
    kubectl proxy --port=8080 --address=0.0.0.0 --disable-filter=true &
    ```
    Note: This will start the proxy, listen on port 8080, listen on all interfaces, and will disable the filtering of non-localhost requests. This command will continue to run in the background of the current terminal’s session.

## Access the Kubernetes Dashboard

1. In your Cloud9 environment, click Tools / Preview / Preview Running Application

    ![role-1](./images/role-1.png)

2. Scroll to the end of the URL and append:

    ```bash
    /api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
    ```
3. The Cloud9 Preview browser doesn’t support the token authentication, so once you have the login screen in the cloud9 preview browser tab, press the Pop Out button to open the login screen in a regular browser tab, as shown below:

    ![role-2](./images/role-2.png)

4. Open a New Terminal Tab and enter:

    ```bash
    aws eks get-token --cluster-name eksworkshop-eksctl | jq -r '.status.token'
    ```

5. Copy the output of this command and then click the radio button next to Token. In the text field below paste the output from the last command.

    ![role-3](./images/role-3.png)

5. Select "SIGN IN"

## Contgratulations!
   You have deployed the Kubernetes Dashboard! 
