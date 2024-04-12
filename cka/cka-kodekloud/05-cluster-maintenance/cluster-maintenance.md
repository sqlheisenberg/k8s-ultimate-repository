```bash
kubectl drain node-1

#mark it as uncordinated so that pods can be scheduled on it again
kubectl uncordon node-1 

#marks pod unschedulable
#this will not affect running pods
kubectl cordon node-2 

kubectl drain node01 --ignore-daemonsets

#Since there are no taints on the controlplane node, all the pods were started on it when we ran 
#the kubectl drain node01 command.
kubectl describe node controlplane | grep -i  taint

#cluster upgrade

# Taints: <none>
# This means that both nodes have the ability to schedule workloads on them.
kubectl describe nodes  node01 | grep -i taint


# https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/
kubeadm upgrade plan

kubeadm upgrade plan v1.29.0

kubeadm upgrade apply v1.29.0

#backup by queryig kubeapi server
kubectl get all --all-namespaces -o yaml > all-deploy-services.yaml

#ETCD has a snashoot utility included
ETCDCTL_API=3 etcdctl snapshot save snapshoot.db

#check status of the backup
ETCDCTL_API=3 etcdctl snapshot status snapshoot.db

#restore backup
#stop kube-apiserver
service kube-apiserver stop

# restore etcd snapshoot
# etcd github repo https://github.com/etcd-io/etcd
# https://github.com/etcd-io/website/blob/main/content/en/docs/v3.5/op-guide/recovery.md
ETCDCTL_API=3 etcdctl snapshot restore snapshoot.db --data-dir /var/lib/etcd-from-backup

#reload demon 
systemctl deamon-reload

#restart etcd service
service etcd restart

#start kub-apiserver
service kube-apiserver start

#with etcd commands you must use cert and key
ETCDCTL=3 etcdctl snapshot save snapshot.db \
                  --endpoints=https://127.0.0.1:2379 \
                  --cacert=/etc/etcd/ca.crt \
                  --cert=/etc/etcd/etcd-server.crt \
                  --key=/etc/etcd/etcd-server.key 

#check etcd version
kubectl -n kube-system logs etcd-controlplane | grep -i 'etcd-version'

kubectl -n kube-system describe pod etcd-controlplane | grep Image:

#At what address can you reach the ETCD cluster from the controlplane node?
kubectl -n kube-system describe pod etcd-controlplane | grep '\--listen-client-urls'

#Where is the ETCD server certificate file located?
kubectl -n kube-system describe pod etcd-controlplane | grep '\--cert-file'

#Where is the ETCD CA Certificate file located?
kubectl -n kube-system describe pod etcd-controlplane | grep '\--trusted-ca-file'

#Take a snapshot of the ETCD database using the built-in snapshot functionality.
ETCDCTL_API=3 etcdctl --endpoints=https://[127.0.0.1]:2379 \
                --cacert=/etc/kubernetes/pki/etcd/ca.crt \
                --cert=/etc/kubernetes/pki/etcd/server.crt \
                --key=/etc/kubernetes/pki/etcd/server.key \
                snapshot save /opt/snapshot-pre-boot.db

#Restore the snapshot
ETCDCTL_API=3 etcdctl  --data-dir /var/lib/etcd-from-backup \
                snapshot restore /opt/snapshot-pre-boot.db

#update the /etc/kubernetes/manifests/etcd.yaml
## When this file is updated, the ETCD pod is automatically re-created 
## as this is a static pod placed under the /etc/kubernetes/manifests directory.

### Note 1: As the ETCD pod has changed it will automatically restart, 
### and also kube-controller-manager and kube-scheduler. Wait 1-2 to mins for 
### this pods to restart. You can run the command: watch "crictl ps | grep etcd" 
### to see when the ETCD pod is restarted.

### Note 2: If the etcd pod is not getting Ready 1/1, then restart it 
### by kubectl delete pod -n kube-system etcd-controlplane and wait 1 minute.

### Note 3: This is the simplest way to make sure that ETCD uses 
### the restored data after the ETCD pod is recreated. You don't have to change anything else.
 volumes:
  - hostPath:
      path: /var/lib/etcd-from-backup
      type: DirectoryOrCreate
    name: etcd-data

#Practice Test Backup and Restore Methods 2

## How many clusters are defined in the kubeconfig on the student-node?
kubectl config view

kubectl config get-clusters 

## switch context
kubectl config use-context cluster1

## swtich context to cluster2
kubectl config use-context cluster2

## How is ETCD configured for cluster1?
### This means that ETCD is set up as a Stacked ETCD Topology
kubectl get pods -n kube-system | grep etcd

kubectl -n kube-system describe pod kube-apiserver-cluster2-controlplane 

## How many nodes are part of the ETCD cluster that etcd-server is a part of?
ETCDCTL_API=3 etcdctl \
                --endpoints=https://127.0.0.1:2379 \
                --cacert=/etc/etcd/pki/ca.pem \
                --cert=/etc/etcd/pki/etcd.pem \
                --key=/etc/etcd/pki/etcd-key.pem \
                member list
                f0f805fc97008de5, started, etcd-server, https://10.1.218.3:2380, https://10.1.218.3:2379, false

``` 
