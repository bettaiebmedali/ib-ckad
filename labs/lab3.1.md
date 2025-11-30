# ReplicaSets
## Goals :
- create a ReplicaSet
- scale up the Pod replica count
- migrate Pods from a ReplicaSet to another
### Duration: 
0:25:00
## Clean up
- Delete all Pods from the default Namespace with:
```yaml
kubectl delete pod --all
```
- Optional: view the resources successive states with:
```yaml
watch kubectl get pod --show-labels
```
## First ReplicaSet

- Create ReplicaSet named nginx from the following descriptor (source :~/workspaces/Lab3/rs--nginx-simple--3.1.yml) :
```yaml
---
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
      level: novice
  template:
    metadata:
      name: nginx
      labels:
        app: nginx
        level: novice
    spec:
      containers:
        - name: nginx
          image: nginx:alpine
          ports:
            - containerPort: 80
```
- Monitor the progress of the creation of the RS and associated Pods with the command
```yaml
watch kubectl get pod,rs --show-labels
```
- Delete one of the created Pods and check that the RS is doing its job
## ReplicaSet and Pod selector
- Create a ReplicaSet from the following descriptor (source :~/workspaces/Lab3/rs--nginx-cannot-create--3.1.yml) :

```yaml
---
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: frontend
spec:
  replicas: 2
  selector:
    matchLabels:
      app: frontend
      level: intermediate
  template:
    metadata:
      name: frontend
      labels:
        app: frontend
        level: novice
    spec:
      containers:
        - name: nginx-fe
          image: nginx:alpine
          ports:
            - containerPort: 80
```
- What is wrong ?
- Fix the selector in the descriptor and create the RS
- Check that the Replicaset has been created successfully

## Modifying a ReplicaSet
- In the RS frontend template, modify the nginx image used to nginx:stable-alpine
- Apply the new confi guration of the RS
- Check that no Pod are being recreated
- With kubectl label modify the labels on the frontend-xxxxx Pods by overriding thevalue for the level key to advanced
- What is going on ? Why ?
- Check that the docker image used by the new Pods created is nginx:stable-alpine
## Scaling a ReplicaSet
- Change the number of replicas of the RS frontend to 3 using <kubectl scale>
- In the RS frontend template, change the number of replicas to 4 and apply the new configuration
- Use kubectl describe pod frontend-xxxx on one of the Pods associated with thefrontend ReplicaSet and note the value of the
Controlled By field
- Use kubectl get pod frontend-xxxx -o yaml on one of the Pods associated with theReplicaSet frontend and note the value of the
.metadata.ownerReferences field

## Deleting a ReplicaSet
- Delete the RS nginx with its Pods 
- Delete the RS frontend without deleting its Pods
- Use kubectl describe pod frontend-xxxx on one of the Pods associated with thefrontend ReplicaSet and note that the
Controlled By field is no longer present
- Use kubectl get pod frontend-xxxx -o yaml on one of the Pods associated with thefrontend ReplicaSet and note that the
.metadata.ownerReferences field is no longerpresent

## Migrating Pods from one ReplicaSet to another
- We now have several orphaned frontend-* Pods, i.e. without ReplicaSet. The goal is to attachthem to a new ReplicaSet
named frontend-rebirth.
    - Create a ReplicaSet (called frontend-rebirth) to manage the Pods previously handled by the RS frontend (app=frontend,level=novice)
    - Check ReplicaSet status with:

```yaml
     kubectl get rs frontend-rebirth
```
- 
    - Use kubectl describe pod frontend-xxxx on one of the Pods previously associatedwith the frontend ReplicaSet and note the value of the Controlled By field
    - Use kubectl get pod frontend-xxxx -o yaml on one of the Pods previouslyassociated with the frontend ReplicaSet and note the value of the .metadata.ownerReferences field