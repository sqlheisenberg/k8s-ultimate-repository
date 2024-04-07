# Image Security

To use image from private registry within pod defitinion file:
1. Create secrets for the registry
`docker-registry` is built in secret type
```bash
kubectl create secret docker-registry private-reg-cred \
    --docker-server=myprivateregistry.com:5000       \
    --docker-username=dock_user           \
    --docker-password=dock_password       \
    --docker-email=dock_user@myprivateregistry.com       
```
2. Specify secret inside pod defintion file
```yaml
apiVersion: v1
kind: Pod
metadata: 
    name: nginx-pod
spec: 
    containers:
    - name: nginx
      image: private-registry.io/apps/internal-app
    imagePullSecrets:
    - name: regcred
```
