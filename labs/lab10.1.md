# Kustomize
## Goal:
- Deploy and update an application with Kustomize
### Duration:
0:10:00
### Notice
: For the next steps of the lab, replace FIXME with the IP of your minikube machine:

💻 (local) : minikube ip


- Check the structure of the fi les in the ~/workspaces/Lab10 directory
- View the contents of the supplied ~/workspaces/Lab10/base/kustomization.yaml file
- Replace the FIXME value in ~/workspaces/Lab10/base/kustomization.yaml file:
- Create and use kustomize-dockercoins Namespace:
```bash
kubectl create namespace kustomize-dockercoins

kubectl config set-context --current --namespace=kustomize-dockercoins
# or:
# kubens kustomize-dockercoins

```
- Launch this command and check the result:
```bash
kubectl kustomize ~/workspaces/Lab10/base
```
- Apply base confi guration:
```bash
kubectl apply --kustomize ~/workspaces/Lab10/base
```
- Check the app in your browser at http://dockercoins.<PUBLIC_IP>.sslip.io 
- Apply the perfs overlay configuration:
```bash
kubectl apply --kustomize ~/workspaces/Lab10/overlays/perfs
```
- Monitor the creation of Pods
- Apply the upgrade overlay configuration:
```bash
kubectl apply --kustomize ~/workspaces/Lab10/overlays/upgrade
```
- Delete the application:
```bash

kubectl delete --kustomize ~/workspaces/Lab10/overlays/upgrade 
```
- Exit and delete the namespace:
```bash

kubectl config set-context --current --namespace=default
kubectl delete namespace kustomize-dockercoins
```