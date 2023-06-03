# GITOPS ARGOCD

Argocd is a declarative continuos deployment tools, in other words ArgoCD is a declarative, GitOps continuous delivery tool for Kubernetes.

ArgoCD is implemented as a kubernetes controller which continuously monitors running applications and compares the current, live state against the desired target state (as specified in the Git repo)
. It maintains sync between GIT and kubernetes, whatever manifest on git is pulled and deployed on kubernetes
#### Argocd Architecture

![Architecture-cd](https://github.com/Jmcglobal/DevOPs-Projects-01/assets/101070055/0dada98e-8ef7-4d05-9989-e24d0c9f291d)

