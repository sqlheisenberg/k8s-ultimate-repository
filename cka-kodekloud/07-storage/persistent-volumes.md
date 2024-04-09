# Persistant Volumes
`pv-defintion.yaml`
```yaml
apiVersion: v1
kind: PersistentVolume
metadata: 
    name: pv-vol1
spect:
    accessModes:
        - ReadWriteOnce
    capacity:
        storage: 1Gi
    hostPath:
        path: /tmp/data
```
```bash
kubectl create -f pv-definition.yaml
kubectl get persistentvolume
```
```yaml
apiVersion: v1
kind: PersistentVolume
metadata: 
    name: pv-vol1
spect:
    accessModes:
        - ReadWriteOnce
    capacity:
        storage: 1Gi
    awsElasticBlockStore:
        volumeID: <volume-id>
        fsType: ext4
```
To delete persistent volume claim run:
`kubectl delete persistentvolumeclaim myclaim`

## Using PVC in Pods
Once you create a PVC use it in a POD definition file by specifying the PVC Claim name under persistentVolumeClaim section in the volumes section like this:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
    - name: myfrontend
      image: nginx
      volumeMounts:
      - mountPath: "/var/www/html"
        name: mypd
  volumes:
    - name: mypd
      persistentVolumeClaim:
        claimName: myclaim
```
The same is true for ReplicaSets or Deployments. Add this to the pod template section of a Deployment on ReplicaSet.  
Reference URL: https://kubernetes.io/docs/concepts/storage/persistent-volumes/#claims-as-volumes
