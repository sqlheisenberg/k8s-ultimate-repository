# RBAC

 Role object
 - `developer-role.yaml`
 ```yaml
 apiVersion: rbac.authorization.k8s.io/v1
 kind: Role
 metadata:
    name: developer
 rules:
  - apiGroups: [""]
    resources: ["pods"]
    verbs: ["list", "get", "create", "update", "delete"]
  - apiGroups: [""]
    resources: ["ConfigMap"]
    verbs: ["create"]
 ```

 ```bash
 #create role
 kubectl create -f developer-role.yaml
 ```

 `RoleBinding` - `devuser-developer-binding.yaml` object links user with the role.
 ```yaml
 apiVersion: rbac.authorization.k8s.io/v1
 kind: RoleBinding
 metadata:
    name: deviser-developer-binding
 subjects:
  - kind: User
    name: dev-user
    apiGroup: rbac.authorization.k8s.io
  roleRef:
    kind: Role
    name: developer
    apiGroup: rbac.authorization.k8s.io
 ```
 ```bash
 #create rolebinding
 kubectl create -f devuser-developer-binding.yaml
 
 #view created roles
 kubectl get roles

 #check access
 kubectl auth can-i create deployments

 kubectl auth can-i delete nodes
 
 #impresionate another user
 kubectl auth can-i create deployments --as dev-user

 kubectl auth can-i create pods --as dev-user

 kubectl auth can-i create deployments --as dev-user --namespace test

 #check roles in all namespaces
 kubectl get roles --all-namespaces

 kubectl describe role kube-proxy -n kube-system
 
 kubectl describe rolebinding kube-proxy -n kube-system

 kubectl create role developer --namespace=default --verb=list,create,delete --resource=pods

 kubectl create rolebinding dev-user-binding --namespace=default --role=developer --user=dev-user
 ```

