# Jobs and CronJobs
## Goal: 
- create a Job and a CronJob
### Duration: 
0:15:00
## First Job

- Create a Job from the following descriptor (source :~/workspaces/Lab3/job--compute-pi--3.3.yml):
```yaml
---
apiVersion: batch/v1
kind: Job
metadata:
  name: pi
spec:
  template:
    metadata:
      name: pi
    spec:
      containers:
        - name: pi
          image: perl:5.20-stretch
          command:
            - perl
            - -Mbignum=bpi
            - -wle
            - print bpi(2000)
      restartPolicy: Never
```
- Check the Job logs once it is finished
- Show detailed information about the Job
## A bit of parallelism

- Make the changes so that the Job is launched 5 times, with 2 occurrences in parallel

## CronJob 
- Create the CronJob from the following descriptor (source :~/workspaces/Lab3/cron--hello-from-k8s-cluster--3.3.yml):
```yaml
---
apiVersion: batch/v1
kind: CronJob
metadata:
  name: hello
spec:
  schedule: "* * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
            - name: hello
              image: debian:11-slim
              args:
                - bash
                - -c
                - date; echo Hello from the Kubernetes cluster
          restartPolicy: OnFailure
```

- After creating the CronJob, retrieve its status with:
```yaml
kubectl get cronjob
```
- Watch the fi rst associated Job:
```yaml
kubectl get jobs --watch
```
- Retrieve the status of the CronJob once the first Job has finished 
- Suspend the CronJob to stop the execution of Jobs for the rest of the training