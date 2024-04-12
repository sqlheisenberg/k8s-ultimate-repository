```bash
kubectl top node

kubectl top pods

kubectl create -f event-simulator.yaml

kubectl logs -f event-simulator-pod #use -f to stream logs live

#if there are multiple containers running in the pod you need to specify container name
kubectl logs -f event-simulator-pod event-simulator #event-simulator is container name
```
