```bash
eksdemo get network-interface

kubectl get pods -o wide -A

kubectl logs -n kube-system aws-node-z7llk -c aws-vpc-cni-init

kubectl exec -it alpine-7ddf8fb9 -- /bin/sh

kubectl get networkpolicy -A
```

Outputs:
```
NodeAutoScalingGroup = "eks-cluster-stack-NodeGroup-U0gBRswTo8Yp"
NodeInstanceRole = "arn:aws:iam::730335214540:role/eks-demo-node"
NodeSecurityGroup = "sg-0eb334bfae9b9fe60"
```
###  update kubeconfig
`aws eks update-kubeconfig --region us-east-1 --name demo-eks`

`cat .kube/config`

### Download the node authentication ConfigMap:
`curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/cloudformation/2020-10-29/aws-auth-cm.yaml`

### apply ConfigMap
`kubectl apply -f aws-auth-cm.yaml`

### verify the nodes
`kubectl get nodes -o wide`

### Use the following AWS CLI command to get the VPC CIDR range
`aws ec2 describe-vpcs --query 'Vpcs[*].{VpcId:VpcId,CidrBlock:CidrBlock}' --output table`

### Describe the pod to get the IP address:
`kubectl get pod nginx -o jsonpath='{.status.podIP}'`

### VPC CNI
`curl -o aws-k8s-cni.yaml https://raw.githubusercontent.com/aws/amazon-vpc-cni-k8s/master/config/master/aws-k8s-cni.yaml`

### After you made changes on env var `ENABLE_PREFIX_DELEGATION`, `ConfigMap`, `DaemonSet`
`kubectl apply -f eks/manifests/aws-k8s-cni.yaml`

### Verify ENI and prefix assigment
`kubectl apply -f eks/manifests/nginx-pod.yaml`

### Inspect the node;s ENIs and routes
```
kubectl get nodes
kubectl describe node <node-name>
aws ec2 describe-network-interfaces --query 'NetworkInterfaces[*].{ID:NetworkInterfaceId,PrivateIpAddress:PrivateIpAddress}'
aws ec2 describe-route-tables --query 'RouteTables[*].Routes'
```

### Describe pods
`kubectl describe pods/nginx`

### Exec pod
`kubectl exec -it test-pod -- ping <nginx-pod-ip>`
`kubectl exec -it alpine-0 -- /bin/sh`

# EKS Storage

Get `StorageClass`

`kubectl get storageclasses`

`kubectl get persistentvolumes`

## EBS CSI LAB


## Create an EVS Volume using AWS CLI
`aws ec2 create-volume --size 10 --region us-east-1 --availability-zone us-east-1a --volume-type gp2`

```
eks/manifests/ebs.yaml
eks/manifests/storage-class.yaml
eks/manifests/pod-to-use-storage-class.yaml
```

# EKS Loadbalancer

# Compute and Scaling
