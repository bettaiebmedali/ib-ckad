# Liveness probes
## Goal: 
- implement a liveness probe and verify operation
### Duration: 
0:15:00
## Liveness
- Check the following descriptor (source:~/workspaces/Lab2/pod--liveness-probe-starter--2.7.yml):
        The agnhost container launched with the liveness command allows to have a"healthy" service for 10 seconds then "unhealthy" thereafter.
```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: liveness-http
  labels:
    test: liveness
  annotations:
    what-do-you-like: crash-with-error-500
spec:
  containers:
    - name: i-am-alive
      image: registry.k8s.io/e2e-test-images/agnhost:2.21
      args:
        - liveness
```
- Add a liveness type probe for the i-am-alive container:
    - of type httpGet
    - on port 8080 of the container
    - which tests the path /healthz
    - initial delay: 10s
    - timeout: 1s
    - the minimum number of failures for the probe to be considered in failed state: 2
- You can use the model provided in ~/workspaces/Lab2/pod--liveness-probe-starter--2.model.yml, and replace
XXXX,YYYY and ZZZZ with the right properties and values
- Create the Pod
- Observe the successive states of the Pod
- Check events associated with the Pod