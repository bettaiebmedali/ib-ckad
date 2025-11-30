# Lab 2.3: Annotations and Namespaces

## Goals:
- view and add annotations
- display Namespaces
- create a Namespace
- use a Namespace
### Duration: 0:20:00
## Annotations

- Check help for kubectl annotate command
- View existing annotations of yaml-pod
- Add annotation: super.mycompany.com/ci-build-number=722 to yaml-pod pod
- Check that the annotation is present on the Pod
## Namespaces
- Display the list of all Namespaces
- Show all Pods in the kube-system Namespace
- Show all Pods from all Namespaces
- List all pods with label run=yaml-pod from all namespaces
## Creating a new Namespace
- Create a new namespace my-sandbox with a manifest file defining the label sandbox="true"
## Using the new Namespace
- Instantiate the yaml-pod Pod in the my-sandbox Namespace by modifying the Pod descriptor
- List the Pods in the my-sandbox Namespace
- Delete the my-sandbox Namespace