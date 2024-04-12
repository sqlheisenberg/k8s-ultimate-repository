# Persistent Volume Claim (PVC)
`pvc-definition.yaml`
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
    name: myclaim
spec: 
    accessModes:
        - ReadWriteOnce
    resources:
        requests:
            storage: 500Mi
```
```bash
kubectl create -f pvc-definition.yaml

kubectl get persistentvolumeclaim
```
When claim is created, k8 is looking to the volume created previously via `pv-definition.yaml`:
```yaml
apiVersion: v1
kind: PersistentVolume
metadata: 
    name: pv-vol1
spec: 
    accessModes:
        - ReadWriteOnce
    capacity:
        storage: 1Gi
    awsElasticBlockStore:
        volumeID: <volume-id>
        fsType: ext4
```
