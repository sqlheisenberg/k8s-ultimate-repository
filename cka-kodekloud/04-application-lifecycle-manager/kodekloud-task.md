```bash
kubectl -n elastic-stack logs kibana

kubectl describe pod app -n elastic-stack

kubectl -n elastic-stack exec -it app -- cat /log/app.log

```
