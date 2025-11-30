# StatefulSets
## Goal:
- Create a StatefulSet and understand its usage
### Duration: 
0:10:00
- Create a StatefulSet from the following descriptor (source:~/workspaces/Lab8/statefulset--db-cluster--8.1.yml).
```bash
---
apiVersion: v1
kind: Service
metadata:
  name: db-cluster-headless-svc
  labels:
    app: db-cluster
spec:
  selector:
    app: db-cluster
  clusterIP: None
  ports:
    - port: 8080
      name: db
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: db-cluster
  labels:
    app: db-cluster
spec:
  replicas: 2
  serviceName: db-cluster-headless-svc
  selector:
    matchLabels:
      app: db-cluster
  template:
    metadata:
      labels:
        app: db-cluster
    spec:
      containers:
        - name: db
          image: bitboxtraining/k8s-training-statefulset:v2
          ports:
            - name: db
              containerPort: 8080
          volumeMounts:
            - name: data
              mountPath: /data
  volumeClaimTemplates:
    - metadata:
        name: data
      spec:
        accessModes:
          - ReadWriteOnce
        resources:
          requests:
            storage: 50Mi

```
- Check the state of the StatefulSet, associated Pods and PersistentVolumeClaims.
- Connect to the gateway Pod.
- Check that the Pod of the StatefulSet can be reached using its index:
```bash
curl -s db-cluster-0.db-cluster-headless-svc.default.svc.cluster.local:8080
```
- This DNS mechanism is used by the nodes of our stateful clustered application to reach eachother: they only have to know the index of other nodes.
- Exit the gateway Pod
- Create a fi le in the /data directory of the db-cluster-1 Pod:
```bash
kubectl exec db-cluster-1 -- touch /data/i-was-here
```
- Downscale the StatefulSet (keep only one replica)
- Check that the db-cluster-1 has been removed, but the data-db-cluster-1 PersistentVolumeClaim still exists.
- Upscale the StatefulSet (bring it back to 2 replicas)
- Check that a db-cluster-1 Pod has been created and that it uses the data-db-cluster-1 PersistentVolumeClaim.
- Check the presence of the file /data/i-was-here:
```bash
kubectl exec db-cluster-1 -- ls -l /data
```

- This mechanism is used by the nodes of our stateful clustered application to make sure it willget its storage back in case of crash, re-scheduling or scaling of the StatefulSet.
- Remove the StatefulSet.
- Check that PersistentVolumeClaims were not removed.
- Manually remove the PersistentVolumeClaims (use labels to do it with one command!).