# Volumes

```yaml
apiVersion: v1
kind: Pod
metadata: 
    name: random-number-generator
spec: 
    containers:
    - image: alpine
      name: alpine
      command: ["bin/sh","-c"]
      args: ["shuf -i 0-100 -n 1 >> /opt/number.out;"]
      volumeMounts:
      - mountPath: /opt
        name: data-volume

    volumes:
    - name: data-volume
      hostPath:
        path: /data
        type: Directory
```
If you want to specify EBS as an volume
```yaml
apiVersion: v1
kind: Pod
metadata: 
    name: random-number-generator
spec: 
    containers:
    - image: alpine
      name: alpine
      command: ["bin/sh","-c"]
      args: ["shuf -i 0-100 -n 1 >> /opt/number.out;"]
      volumeMounts:
      - mountPath: /opt
        name: data-volume

    volumes:
    - name: data-volume
      awsElasticBlockStore:
        volumeOD: <volume-id>
        fsType: ext4
```
