# Service accounts
Two types of service accounts:  
- user
- service

```bash
kubectl create serviceaccount dashboard-sa

kubectl get serviceaccounts

kubectl describe serviceaccount dashboard-sa
```
Pod definition with service account:
```yaml
apiVersion: v1
kind: Pod
metadata:
    name: my-kubernetes-dashboard
spec:
    containers:
        - name: my-kubernetes-dashboard
          image: my-kubernetes-dashboard
    serviceAccountName: dashboard-sa
```
Prevent default service account to be mounted to the pod by setting up: `automountServiceAccountToken: false`
```yaml
apiVersion: v1
kind: Pod
metadata:
    name: my-kubernetes-dashboard
spec:
    containers:
        - name: my-kubernetes-dashboard
          image: my-kubernetes-dashboard
    automountServiceAccountToken: false
```
From kubernetes  `v1.24` when runing `kubectl create serviceaccount dashboard-sa`
default token would not be created automaticaly. To create token you have to run:
`kubectl create token dashboard-sa` to generate token for the service account.
If you decode token by using:
`jq -R 'split(".") | select(lenght > 0) | .[0],.[1] | @base64 | fromjson <<< eysasaGSAOIsmcs...`

Secret definition file for token after version `v1.24`
```yaml
apiVersion: v1
kind: Secret
type: kubernetes.io/service-account-token
metadata:
    name: mysecretname

    annotations:
        kubernetes.io/service-account.name: dashboard-sa
```
