########################## use below to setup control plane, and 2 worker nodes ########################
Install Docker Destop.

Use Brew,

 brew update
 brew install kind
 clear
 vim kind-config.yaml

 paste the below in the file

 kind: Cluster
 apiVersion: kind.x-k8s.io/v1alpha4
 nodes:
   - role: control-plane
   - role: worker
   - role: worker
  
 kind create cluster --config kind-config.yaml
 kubectl cluster-info --context kind-kind
 kubectl get nodes -o wide
 kubectl config view 
 kubectl get all -A

###########################################################
Argo Setup

kubectl create namespace argocd

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

make the configmap to accept http instead of https
kubectl -n argocd patch configmap argocd-cmd-params-cm --type merge -p '{"data":{"server.insecure":"true"}}'

restart the pods 
kubectl -n argocd rollout restart deployment/argocd-server

watch --differences kubectl get all -n argocd

use this to get the password for admin to argo cd login
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

kubectl -n argocd get svc

do a port forward to access the svc in localhost - 

kubectl -n argocd port-forward service/argocd-server 9191:80


 
