```bash
curl --header "Content-Type:application/json" --request POST --data '{"apiVersion":"v1", "kind": "Binding"}

kubectl create -f nginx.yaml

#select pod with the label
kubectl get pods --selectro app=App1

#filter pods by label env=dev
kubectl get pods -l "env=dev"

#list all resources wihtin environment production
kubectl get all -l "env=prod"

#filter by multiple labels
kubectl get pods -l "bu=finance, env=prod, tier=frontend"

#taints - node
kubectl taint nodes node-name key=value:taint-effect

kubectl taint nodes node1 app=blue:NoSchedule

kubectl describe node kubemaster | grep Taint

kubectl get nodes

kubectl describe nodes node01

kubectl taint nodes node01 spray=mortein:NoSchedule

#untaint the node
kubectl taint nodes controlplane node-role.kubernetes.io/control-plane:NoSchedule-

kubectl get pods -o wide

# kubectl label nodes <node-name> <lable-key>=<label-value>
kubectl label nodes node-1 size=Large

kubectl label node node01 color=blue

kubectl label --help

kubectl get pods --show-labels

kubectl get pods -L creation_method, env #show pods with multiple specific labels

kubectl label pods <pod-name> label-key=label-value

kubectl label pods <pod-name> label-key=label-value

#update label
kubectl label pods <pod-name> label-key=label-value --overwrite

kubectl create deployment blue --image=nginx --replicas=3

# to get yaml manifest file
kubectl create deployment red --image=nginx --replicas=2 --dry-run=client -o yaml 

kubectl describe node node01 | grep -i taints

kubectl create -f deamon-set-definition.yaml

kubectl get deamonsets

kubectl get daemonsets --all-namespaces

kubectl describe deamonstes monitoring-app

kubectl describe daemonset kube-proxy --namespace=kube-system

kubectl describe daemonset kube-flannel-ds --namespace=kube-flannel
```
