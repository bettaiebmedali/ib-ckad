## Lab 1.2: kubectl

### Goal: Getting started with the kubectl CLI
Duration: 0:05:00
### Getting started
- Display the list of all available commands:
```yaml
kubectl
kubectl get --help
```
- Check the completion is working by typing: kubectl g<tab>
- Display client (kubectl) and cluster versions:
```yaml
kubectl version --output yaml
```

- Display the common options:
```yaml
kubectl options
```
### Cluster info
- Display the name of the currently used cluster:
```yaml
kubectl config current-context
```
- Display the cluster nodes:
```yaml
kubectl get nodes
```
### Check available groups and resources
- Check resources available on the cluster with:
```yaml
kubectl api-resources
```