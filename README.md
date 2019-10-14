Setup Jenkins with JCasc on top Kubernetes

Requirements and tested on these versions:

    - Minikube(v1.1.1)
        - Virtualbox or KVM
    - Kubectl(v1.11.7)
    - Helm(v2.11.0)

# Deployment 
1. Follow this official guide to deploy Minikube locally https://kubernetes.io/docs/tasks/tools/install-minikube/
2. `minikube start --cpus 2 --memory 4000`
3. After up and running you can find the kubeconfig file in ~/.kube/config
4. `minikube status` make sure all components are up and running

    Clone the repo

    `git clone https://github.com/janavenkat/jenkin.git`

    `cd jenkin`

5. Install tiller in minikube by following this command 

    `helm init`
6. Install helm jenkins chart 

    `helm install --name jenkins jenkins/.`

7. To get minikube IP
    
     `minikube ip`

8. Wait for until jenkins pod up and running 

    `kubectl get pods`

9. Get the nodeport for jenkins to access it from browser 

    `kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services jenkins`

10. Enter your minikube ip with port example `http://192.168.99.100:31995`
11. Get credentials for jenkins.

    username: ` admin`

    to get the password execute this command

    ` printf $(kubectl get secret --namespace default jenkins -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode);echo`



12. After successfully logged-in have to approve our script to run our job
> Go to Manage Jenkins 

> Now search for in process script approval need to approve the script in order to run our job
![Alt text](images/1.png?raw=true "Get")


![Alt text](images/2.png?raw=true "Get")

13. Now it's time to run the job

14. After successfull job run, pipeline view

![Alt text](images/3.png?raw=true "Get")

#Information
> Local Helm chart is pre-configured with JCasc job and dependencies and also for the job project configure from this repo https://github.com/mendix/pluggable-widgets-typing-generator

> the workers are dynamically created on-demand in kubernetes as a pod

> All changes for this deployment are in jenkins/values.yaml
  1. Changes for minikube deployment https://github.com/janavenkat/jenkin/blob/213245f536efaf2b94dfab4cfe35e0dbcd592196/jenkins/values.yaml#L93
  2. JcasC changes https://github.com/janavenkat/jenkin/blob/213245f536efaf2b94dfab4cfe35e0dbcd592196/jenkins/values.yaml#L206
  3. Pipeline job configuration https://github.com/janavenkat/jenkin/blob/213245f536efaf2b94dfab4cfe35e0dbcd592196/jenkins/values.yaml#L234



#Additional one deployed on GKE k8's
> Password is encrypted like kubernetes secret you know how to use :) it 