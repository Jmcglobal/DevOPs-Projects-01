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
        
