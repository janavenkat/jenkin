Setup Jenkins with JCasc on top Kubernetes

Requirements:
    - Minikube
        - Virtualbox or KVM
    - Kubectl
    - Helm

# Deployment 
1. Follow this official guide to deploy Minikube locally https://kubernetes.io/docs/tasks/tools/install-minikube/
2. `minikube start --cpus 2 --memory 4000`
3. After up and running you can find the kubeconfig file in ~/.kube/config
4. `kubectl get pods`
5. `helm init`
6. `helm install --name jenkins jenkins/.`
7.  Get credentials for jenkins. 
    username: ` admin`

    to get the password execute this command

    ` printf $(kubectl get secret --namespace default jenkins -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode);echo`
8. `minikube ip`
9. wait for untill jenkins pod up and running `kubectl get pods`
10. get the nodeport for jenkins to access it from browser `kubectl get svc`
    `NODE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services jenkins)`
11. Enter your minikube ip with port example `http://192.168.99.100:31995`


12. After successfully logged-in
    > Go to Manage Jenkins

> Local Helm chart is pre-configured with JCasc job and dependencies and also for the job project configure from this repo https://github.com/mendix/pluggable-widgets-typing-generator

> the workers are dynamically created on-demand in kubernetes as a pod

> All changes for this deployment are in jenkins/values.yaml