# Image Security

To use image from private registry within pod defitinion file:
1. Create secrets for the registry
`docker-registry` is built in secret type
```bash
kubectl create secret docker-registry regcred \
    --docker-server=private=registry.io       \
    --docker-username=registry-user           \
    --docker-password=registry-password       \
    --docker-email=registry-user@org.co       \
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
