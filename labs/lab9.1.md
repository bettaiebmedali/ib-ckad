
# Helm

## Goal: 
- Deploy and update an application with Helm
### Duration:
0:10:00

## Cleanup

- Clean up the cluster with the following commands (ignore displayed errors) :

```bash
# Delete all Deployments, StatefulSets, DaemonSets, ReplicaSets, and Services
kubectl delete deploy,sts,ds,rs,svc --all

# Delete all Pods
kubectl delete pods --all

# Delete all PersistentVolumeClaims and PersistentVolumes
kubectl delete pvc,pv --all

# Prune all unused Docker images on each Minikube node
for node in $(kubectl get nodes -o name | cut -d'/' -f2); do
  minikube ssh --node "${node}" -- docker image prune --all --force
done


```

## Deploy the application



- Check that Helm is correctly installed:
```bash
helm version
``` 
- Create and use helm-dockercoins Namespace:
```bash
# Create the namespace
kubectl create namespace helm-dockercoins

# Set the current context to use the new namespace
kubectl config set-context --current --namespace=helm-dockercoins

# Alternatively, if you have kubens installed:
# kubens helm-dockercoins

```
- Get in the ~/workspaces/Lab9/dockercoins directory
- Check the contents of the supplied values.yaml file
- Inspect the contents of the fi les in the templates folder
- Get dependencies:
```bash
helm dependency update
```
- Deploy a release:
```bash
# Set the public IP (either from an environment variable or Minikube)
public_ip=${PUBLIC_IP}
# Or: public_ip=$(minikube ip)

# Display the public IP
echo "public_ip: ${public_ip}"

# Install the Helm chart
helm install dockercoins ./ \
  --set ingress.webui.host=dockercoins.${public_ip}.sslip.io

```
- Check the output produced by the install command
- Monitor the creation of Pods
- Check the app in your browser at http://dockercoins.<PUBLIC_IP>.sslip.io
- List releases with:
```bash
helm list
```
## Update the deployment
- Increase the number of worker replicas:

```bash
helm upgrade dockercoins ./ \
  --reuse-values \
  --set replicaCount.worker=2

```
- Update the worker:

```bash
helm upgrade dockercoins ./ \
  --reuse-values \
  --set image.worker.version=1.1

```


- Monitor the update with:
```bash
watch kubectl get pods,rs,ds,deploy,svc,ing -o wide
``` 

## Delete the application
- Delete the release:
```bash
helm delete dockercoins
```
- Exit and delete the namespace:
```bash
kubectl config set-context --current --namespace =default
kubectl delete namespace helm-dockercoins
```
