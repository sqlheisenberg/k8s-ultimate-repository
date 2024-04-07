# Network Policies
Network policies `kubectl` commands:
```bash
kubectl get networkpolicies

```
Network policy configuration file.
Only ingress traffic is spefied below. Egress traffic is not affected. To affect it you have to specify it. 
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: db-policy
spec:
  podSelector:
    matchLabels:
        role: db
    policyTypes:
    - Ingress
    ingress:
    - from:
      - podSelector:
          matchLabels:
            name: api-pod
      ports:
      - protocol: TCP
        port: 3306
```
```bash
kubectl create -f policy-definition.yaml
```
Ingress network policy:
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: db-policy
spec: 

  podSelector:
    matchLabels:
      role: db

  policyTypes:
  - Ingress

  ingress:
  - from:
    - podSelector:
          matchLables:
            name: api-pod
      namespaceSelector:
          matchLabels:
            name: prod
    - ipBlock:
          cidr: 192.168.5.10/32
    ports:
    - protocol: TCP
      port: 3306
```
Ingress and Egress network policy:
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: db-policy
spec: 

  podSelector:
    matchLabels:
      role: db

  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - podSelector:
          matchLables:
            name: api-pod
    ports:
    - protocol: TCP
      port: 3306
  egress:
  - to:
    - ipBlock: 
          cidr: 192.168.5.10/32
    ports:
    - protocol: TCP
      port: 80
```
Network policy to allow traffic from the Internal application only to the payroll-service and db-service
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: internal-policy
  namespace: default
spec:
  podSelector:
    matchLabels:
      name: internal
  policyTypes:
  - Egress
  - Ingress
  ingress:
    - {}
  egress:
  - to:
    - podSelector:
        matchLabels:
          name: mysql
    ports:
    - protocol: TCP
      port: 3306

  - to:
    - podSelector:
        matchLabels:
          name: payroll
    ports:
    - protocol: TCP
      port: 8080

  - ports:
    - port: 53
      protocol: UDP
    - port: 53
      protocol: TCP
```
