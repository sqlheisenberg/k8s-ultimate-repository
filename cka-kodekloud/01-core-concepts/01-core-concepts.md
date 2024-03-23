```bash
kubectl get pods

kubectl explain pods

kubectl explain pod.spec

#to check logs of the pod
kubectl logs <pod-name>

#if you have multiple containers within the pod
kubectl logs <pod-name> -c <container-name>

kubectl run --help

kubectl run redis --image=redis123 --dry-run=client -o yaml

kubectl run redis --image-redis --namespace=finance #run redis image in namespace finance

kubectl run custom-nginx --image=nginx --port=8080

kubectl run httpd --image=httpd:alpine

kubectl expose pod httpd --type=ClusterIP --port=80 --name=httpd

kubectl create -f redis.yaml

kubectl create -f rc-definition.yml

kubectl get replicationcontroller

kubectl create -f replicaset-fefinition.yml

kubectl get replicaset

kubectl replace -f replicaset-definition.yml

kubectl scale --replicas=6  -f replicaset-definition.yml

kubectl scale --replicas=6 replicaset myapp-replicaset 

kubectl create -f deployment-definition.yml

kubectl get deployments

kubectl get all # show all created objects

kubectlk create deployment --help

kubectl create deployment httpd-frontedn --image httpd-frontend --replicas=3

kubectl create -f service-definition.yml

kubectl expose pod redis --port=6370 --name=redis-service --dry-run=client -o yaml

kubectl get services

kubectl describe services redis-service 

kubectl create service --help

kubectl create -f namespace-dev.yml

kubectl create namespace dev

kubectl config set-context --current --namespace=<insert-namespace-name-here>
# Validate it
kubectl config view --minify | grep namespace:

kubectl config set-context ($kubectl config current-context) --namespace=dev

kubectl get pods --all-namespaces

kubectl create -f compute-quota.yaml

```
