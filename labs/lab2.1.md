# Lab 2.1: Pods

### Goal: create a Pod and check its behaviour
Duration: 0:30:00
## Getting started with 'kubectl apply'

- Display the help for kubectl apply and browse the available options
## Use a YAML descriptor

- Create a yaml Pod manifest:
    - name: yaml-pod
    - with image traefik/whoami:v1.10
    - reference container port 80
    - Note: use the following command to create a descriptor example in the file yaml-pod.yml :
```yaml
kubectl run POD_NAME --image=IMAGE_NAME --port=PORT_VALUE --dry-run=client -o yaml > yaml-pod.yml
```

- Instantiate the Pod using the descriptor file


- Check that the application is working with:
```yaml

minikube ssh -- curl -s <ip-du-pod-yaml-pod>:80
```
## Adding a container in the Pod
- Delete the pod yaml-pod
- Edit the descriptor of yaml-pod to add a second container:
    - name: shell-in-pod
    - image: bitboxtraining/k8s-training-tools:v5
    - the main process of the container should be sleep infinity 
    - force Kubernetes to use the last version of the image with imagePullPolicy
    - Use file model: ~/workspaces/Lab2/pod--yaml-pod-2.model.yaml
    - Create the updated Pod
    -Check application:
```yaml 
minikube ssh -- curl -s <ip-du-pod-yaml-pod>:80
```
## Test whoami from the sidecar container
- Display help for kubectl exec
- Connect in the container shell-in-pod of Pod yaml-pod to open a bash session with kubectl exec
- Check that the whoami application is reachable on port 80 of localhost
## Edit a Pod
- Display detailed information for the Pod yaml-pod with kubectl describe and with kubectl get pod POD_NAME -o yaml
- Edit the manifest to add a label from-descriptor: "yaml" (use the following example,source : ~/workspaces/Lab2/     pod--debian-with-label--2.1.yml ) [label usage willbe covered in the next section]
- Apply the modifi cations on the Pod yaml-pod and check that the changes are correctlyapplied

## Display the defi nition of a Pod
- Launch the command
```yaml
kubectl get pod yaml-pod -o yaml
```
- Check the sections of the manifest