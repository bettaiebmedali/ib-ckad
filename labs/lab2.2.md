# Lab 2.2: Labels

## Goals:
- display and add labels on a Pod or Node
- select Pods with labels
- use labels to control scheduling
### Duration: 0:20:00
## Getting started

- Check help for kubectl label
- Check Pod labels of yaml-pod
- Add labels to the yaml-pod pod:
    - release=stable
    - stack=market
- Update release=stable → release=unstable
- Show labels and verify
- View full Pod description yaml-pod and observe labels

## Show Pod Labels
- Display all Pod labels
- Display run, release, stack labels in dedicated columns
- Select Pods with label stack
- (Optional) display Pods without release label
- (Optional) display Pods where run is whoami or yaml-pod
- (Optional) display Pods for which the run label is whoami or yaml-pod and for which stack is not specified

## Using a nodeSelector
- List cluster node labels
- Create descriptor 
    - name: light-sleeper
    - container image: debian:11-slim
    - command: bash -c 'sleep 3d'
    - labels:
        - from-descriptor=yaml
    - Constrain the placement of the Pod on a node with the label
        - cluster-distribution=minikube


- Use model: ~/workspaces/Lab2/pod--light-sleeper.model.yaml , and replace XXXX , YYYY and ZZZZ with the right values
- Create Pod
- List all Pods in another session with 
```yaml
watch kubectl get pods
```
- What is the status of the light-sleeper Pod? Why?
- Identify your nodes names with 
```yaml
kubectl get nodes
```
- Add the label cluster-distribution=minikube on one of the nodes
- Check that the light-sleeper Pod starts