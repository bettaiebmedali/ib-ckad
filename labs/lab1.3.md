# Lab 1.3: kubectl run

## Goal:
- create a Pod without a descriptor
- troubleshoot a Pod's start
### Duration: 0:15:00
## Getting started
- Display the help for the kubectl run command:
```yaml
kubectl run --help
```
## Start Pod
- Launch the following command that will:
    - start a whoami Pod
    - from the traefik/whoami:v1.10 image
    - and reference the 80 port
```yaml
kubectl run whoami --image=traefik/whoami:v1.10 --port=80
```
- Check the Pod start with (ctrl+c to exit the watch loop)
```yaml
watch kubectl get po
```
- What can you see?

- Display more details on the Pod with kubectl describe and get its IP address which you will need later

- Check that the application is working with the following command:
```yaml
# We have to enter in a node of the cluster
minikube ssh -- curl <ip-of-the-pod>:80/api
```
## Troubleshoot a Pod startup
- Launch the following command that will:
    - start a Pod (we will cover the Pod's detail later)
    - with the name faulty-whoami
    - from the image traefik/whoami:nil
    - and reference the 80 port
```yaml
kubectl run faulty-whoami --image=traefik/whoami:nil --port=80
```
- Check the Pod's startup with (ctrl+c to exit the watch loop)
```yaml
watch kubectl get po
```
- Check that the Pod fails to start

- Display more info on the Pod and check the Events with kubectl describe to determine the error cause

- Delete this Pod with:
```yaml

kubectl delete pod faulty-whoami
```
## Start a Pod shell
- Launch the following command that will:
    - start a training-shell Pod
    - from the image bitbowtraining/k8s-training-tools:v5
    - whose main process will be sleep infinity
```yaml
kubectl run training-shell --image=bitboxtraining/k8s-training-tools:v5 --command -- sleep infinity
```

- Check the Pod's startup

- List all created Pods

- Launch:
```yaml
kubectl exec training-shell -- curl -s <ip-of-the-pod>:80/api
```
- Display the help for kubectl exec

## K9s
is a TUI ("Text User Interface") used to browse Kubernetes resources without kubectl commands.
Launch k9s to find the created Pod and browse the interface. The keyboard shortcuts are displayed on top of the screen. Use
CTRL+C
to exit.
## Optional: Dashboard usage
- The Kubernetes dashboard is used to browse Pods and other resources from a web UI.y
- To launch it:
- Check that the minikube addon dashboard is enabled, enable it if needed:
```yaml
minikube addons enable dashboard
```
- Expose the dashboard with
```yaml
minikube dashboard &
```
- Open the provided URL with your browser if it's not done automatically
- Browse the interface to find the Pod which was just created. 