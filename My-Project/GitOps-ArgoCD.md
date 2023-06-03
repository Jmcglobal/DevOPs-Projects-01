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
            Select connect repo
            Choose via HTTPS
            
           UNDER CONNECT REPO USING HTTPS
           select git as type
            under project select default
            enter repository url (https://github.com/Jmcglobal/argocd-yamls)
            If your project git repo is private , then enter password and username of user github account
            Then click connect

![connect-repo](https://github.com/Jmcglobal/DevOPs-Projects-01/assets/101070055/45df66bf-c311-4ecc-ac46-b0004c246551)

![connect-repo-successfull](https://github.com/Jmcglobal/DevOPs-Projects-01/assets/101070055/43029be6-c883-453e-b096-3d8cc10e5c77)

#### ADD APPLICATION

       Select create application

- UNDER GENERAL

            Enter application name (in small letters)
            select default under project
            under sync, select manual or automatic

![general](https://github.com/Jmcglobal/DevOPs-Projects-01/assets/101070055/d0bab156-c199-4e90-b2bb-9415f9d82271)

- Configure Source

            select the repository url 
            Enter path, that contains the yaml file

![source](https://github.com/Jmcglobal/DevOPs-Projects-01/assets/101070055/732de0d2-5c2b-4b54-8e11-a1d1fbf46ca4)

- Configure Destination

      Select default cluster
      Enter default as namespace
      
![destination](https://github.com/Jmcglobal/DevOPs-Projects-01/assets/101070055/2ae1a8c7-27aa-4af5-8aa8-804ab44c8624)

Click create Button at the top left

![application-created](https://github.com/Jmcglobal/DevOPs-Projects-01/assets/101070055/b8008b1a-ae40-4800-aa39-413454fe9f55)

Picture above shows the application is created, but it is out of sync, because i selected manual sync when configuring the application.

- Select/Click on the application to see the full overview of resources it will be deployed once it is sync

      Click App details and App diff for more detailed information

![select-application](https://github.com/Jmcglobal/DevOPs-Projects-01/assets/101070055/13796766-5af9-4e12-911f-092ac7522372)

- Click on sync to deploy the resources to kubernetes cluster

![sync-resource](https://github.com/Jmcglobal/DevOPs-Projects-01/assets/101070055/3055f0fb-3c03-4132-b875-1a6a5c96483e)

- Click on synchronize

![synchronized](https://github.com/Jmcglobal/DevOPs-Projects-01/assets/101070055/36e26678-4e72-4db8-b3ad-e8a071404eee)

Above shows the resources is successfully synchronized

kubectl get all  >> this will display nginx apps been deployed, the replicas, service, pods, deployment.

Any changes that is made to the manifest file, or kubectl resources using kubectl, argocd will dictate and it will point it as "OutOfSynC"

### TO ENABLE AUTO SYNC

- Click on APP Details

      Scroll down and enable auto sync
      
- On sync policy, enable Prune resource and Self heal (prune resource will delete extra resource)

- Any changes made to the manifest file will be automatically deployed to the kubernetes cluster

- Delete the application by clicking on delete

### ARGOCD USING HELM CHART

- Click on settings, select repositories and click connect repo
 
      Here i will be adding grafana charts
      https://grafana.github.io/helm-charts 

- connect method via HTTPS

      Select type helm
      Enter name
      select  Project
      Enter repository URL  ( https://grafana.github.io/helm-charts )

      Click connect
      
![grafana-repo](https://github.com/Jmcglobal/DevOPs-Projects-01/assets/101070055/71271555-352d-4559-9df7-b7c5f735663e)

### CREATE APPLICATION

- UNDER GENERAL

      Enter preferred application name
      Project name = default 
      Sync policy = ( manual / automatic )

![general-application](https://github.com/Jmcglobal/DevOPs-Projects-01/assets/101070055/dd5cb6e9-0cd4-4aef-b146-1335f61ced57)

- Configure Source and Destination

      Select repository URL
      Select chart from list of available chart  (choose grafana)
      Select version of grafana you want

      Select kubernets cluster url
      Enter default namespace or preferred namespace

      Then select create

![source-destina-appl](https://github.com/Jmcglobal/DevOPs-Projects-01/assets/101070055/7fce0a4a-479c-4c5d-86ad-d744c708bbc6)

#### Helm Application is created

![helm-app-created](https://github.com/Jmcglobal/DevOPs-Projects-01/assets/101070055/616597bb-e6f7-495e-b730-0e1f0ada0225)

- Click on it, to see all the objects that will be deployed

![grafana-view-app](https://github.com/Jmcglobal/DevOPs-Projects-01/assets/101070055/809b0b92-3b20-4466-b404-7902ee8a2309)

- Click on sync, then select synchronize to deploy to the kubernetes cluster

![synk-okay](https://github.com/Jmcglobal/DevOPs-Projects-01/assets/101070055/1914de53-63df-4e1c-a00e-acb2bb9b0b81)

- To Edit, click on app details, select parameters, then click on edit

      Scroll down and change service.type port to NodePort, 
      if there is any changes to make, also include it then save
      
- Then sync it again, it will be updated on kubernetes cluster


### ARCGOCD CLI USE

- Download argocd binary
	
      pacman -S argocd
      curl -sSL -o argocd-linux-amd64 https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
      sudo install -m 555 argocd-linux-amd64 /usr/local/bin/argocd
      rm argocd-linux-amd64

- Before running a command against the server, use cli command to login


      argocd login <nodeport-ip:37673> --insecure --password jmcglobal
      Then enter username ( with this command, --insecure “skip server auth certificate”)
      argocd login 192.168.49.2:32465 --password jmcglobal --username admin --insecure

      argocd cluster list 	>> List out default cluster cluster
      argocd proj list		>> list project
      argocd repo list 		>> List repositories
      argocd app list 		>> list applications
      argocd logout 192.168.49.2:32465		>> logout from server
      argocd proj create jmcglobal 		>> create project
      argocd account get-user-info 		>> see user info
      argocd account update-password 	>> Change password
      argocd app create  | less 		>> Follow guides to create apps
      argocd app get <app-name>		>> get and see app status/synced 
      argocd app resources <app-name> 	>> see all resources contain in the app
      argocd app delete grafana	>> Delete app grafana
      
- Reach out to: mmadubugwuchibuife@gmail.com         >>>>>>>>> If you have any enquiry of having some difficulty
