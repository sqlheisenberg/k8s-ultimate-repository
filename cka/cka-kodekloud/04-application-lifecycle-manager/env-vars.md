# Configure Environment Variables in Applications
```bash
#create config maps
kubectl create configmap <config-name> --from-literal=<key>=<value> #imprerative

kubectl create configmap <config-name> --from-file=<path-to-file>

kubectl create -f config-map.yaml #declerative

kubectl get configmaps

kubectl describe configmaps

kubectl create configmap  webapp-config-map --from-literal=APP_COLOR=darkblue --from-literal=APP_OTHER=disregard
```
