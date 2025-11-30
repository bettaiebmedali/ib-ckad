
# Init Containers
## Goals: add an Init Containers
### Duration: 0:15:00
## Experiments
- Check Init Containers specs with
```yaml
kubectl explain
``` 
- Duplicate the whoami-and-clock Pod descriptor (source:~/workspaces/Lab2/pod--whoami-and-clock--2.4.yml) to create a new
whoami-and-clock-with-init Pod:
- Add an Init Container:
    - called timer
    - which uses the debian:11-slim image
    - which loops displaying the date every 1 second for 15 seconds (with the following bash command for i in {1..15}; do date; sleep 1s; done forexample)
- You can use the model in ~/workspaces/Lab2/pod--whoami-and-clock-with-init.model.yml and replace XXXX,YYYY and ZZZZ

- Launch the whoami-and-clock-with-init Pod
- Check that the Pod takes more than 15 seconds to launch with:
```yaml
watch kubectl get po whoami-and-clock-with-init
```
- Access the logs of the timer container and check that they contain the date display asexpected