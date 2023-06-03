# GITOPS ARGOCD

Argocd is a declarative continuos deployment tools, in other words ArgoCD is a declarative, GitOps continuous delivery tool for Kubernetes.

ArgoCD is implemented as a kubernetes controller which continuously monitors running applications and compares the current, live state against the desired target state (as specified in the Git repo)
. It maintains sync between GIT and kubernetes, whatever manifest on git is pulled and deployed on kubernetes

#### Argocd Architecture

![Architecture-cd](https://github.com/Jmcglobal/DevOPs-Projects-01/assets/101070055/0dada98e-8ef7-4d05-9989-e24d0c9f291d)

## Components of ArgoCD
#### API Server

The API server is a gRPC/REST server which exposes the API consumed by the Web UI, CLI, and CI/CD systems. It has the following responsibilities:

      application management and status reporting
      invoking of application operations (e.g. sync, rollback, user-defined actions)
      repository and cluster credential management (stored as K8s secrets)
      authentication and auth delegation to external identity providers
      RBAC enforcement
      listener/forwarder for Git webhook events

#### Repository Server

The repository server is an internal service which maintains a local cache of the Git repository holding the application manifests. It is responsible for generating and returning the Kubernetes manifests when provided the following inputs.

      repository URL
      revision (commit, tag, branch)
      application path
      template specific settings: parameters, helm values.yaml

#### Application Controller

The application controller is a Kubernetes controller which continuously monitors running applications and compares the current, live state against the desired target state (as specified in the repo). It detects OutOfSync application state and optionally takes corrective action. It is responsible for invoking any user-defined hooks for lifecycle events (PreSync, Sync, PostSync).

### GETTING STARTED WITH ARGOCD

#### Set up kubernetes cluster up and running

- How to setup minikube cluster

      https://github.com/Jmcglobal/Kubernetes-MyProject/tree/master/local-minikub

- How to setup KOPS cluster using AWS

      https://github.com/Jmcglobal/Kubernetes-MyProject/tree/master/KOPS
  
- How to setup EKS Cluster

      https://github.com/Jmcglobal/Kubernetes-MyProject/tree/master/EKS

## Here i will be using minikube cluster

![mini-kub-cluster](https://github.com/Jmcglobal/DevOPs-Projects-01/assets/101070055/7a6d0397-86c7-45fa-b97a-83f1ce20ffcb)

#### Install ArgoCD inside the cluster

- Create a namespace "argocd"

        kubectl create namespace argocd
        kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
       
- Get all resources on argocd namespace

        kubectl get all -n argocd

![argocd-resources](https://github.com/Jmcglobal/DevOPs-Projects-01/assets/101070055/68e2b637-6b5b-433d-88a8-2c7ed7fcb788)

This shows that the installation of argocd is successfull, all the necessary services, pod and servers as well as replicaset is created

### ACCESS ARGOCD WEB UI

- Edit argocd-server service type to NodePort, Loadbalancer or use Ingress

            kubectl edit svc argocd-server -n argocd
            
- Scroll down Change service type to NodePort or LoadBalancer (use appropraite command to save and exit)

- Use kubectl get svc argocd-server -n argocd >> to see port number 

![argocd-port-number](https://github.com/Jmcglobal/DevOPs-Projects-01/assets/101070055/cf190d5d-e8ba-4e3f-9788-cccfe3c7e573)

- From above output, we have two different port number to access the argocd-server web ui ( 30264 mapped to port 80 and 30859 mapped to port 443) 

Access the argocd server, using cluster node public IP or loadbalancer, if on minikube cluster user minikube ip

      http://<minikubeip or node public ip>:30264

![argocd-home-page](https://github.com/Jmcglobal/DevOPs-Projects-01/assets/101070055/0da53f75-c29b-48cc-90b5-825a2033ad20)

- Username = admin

- Password

      kubectl get secret argocd-initial-admin-secret -n argocd -o yaml | grep password
      
- Copy the encrypted 

      echo <paste-th-password> | base64 -d
      
- Copy the password and enter it as password

![argocd-page](https://github.com/Jmcglobal/DevOPs-Projects-01/assets/101070055/6bac791c-0025-4e80-b8e7-a88b6d3faa92)

#### Argocd uses dedicated git repo, where the declartive manifest files are hosted

- https://github.com/Jmcglobal/argocd-yamls

### ADD/CONNECT REPOSITORY

            Click settings , select repository
            Select connect via HTTPS
            If your repo is private , then enter password and username
            Then click connect


