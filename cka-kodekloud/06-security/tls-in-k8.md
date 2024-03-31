```bash
#Generate keys
openssl genrsa -out ca.key 2048

#Certificate signing request
openssl req -nww -key ca.key -subj "/CN=KUBERNETES-CA" -out ca.cs

#sign certifcates
openssl x509 -req -in ca.csr -signkey ca.key -out ca.crt

#Generating client certificates
## generate keys
openssl genrsa -out admim.key 2048

## Generate Certificate signing request
## Note: Name can be anything doesnt need to be kube-admin
openssl req -new -key admin.key -subj "/CN=kub-admin" -out admin.crs #note: Name can be anything doesnt need to be kube-admin

#sign certificates
openssl x509 -req -in admin.csr -CA ca.crt -CAkey ca.key -out admin.crt

# view certificate details
cat /etc/systemd/system/kube-apiserver.service

cat /etc/kubernetes/manifests/kube-apiserver.yaml

openssl x509 in /etc/kubernetes/pki/apiserver.crt -text -noout

# Run crictl ps -a command to identify the kube-api server container
crictl ps -a

# Run crictl logs container-id command to view the logs.
crictl logs container-id

crictl ps -a | grep kube-apiserver

crictl logs --tail=2 0ddeb67199bf0
```

```bash
# Use this command to generate the base64 encoded format as following
cat akshay.csr | base64 -w 0
```
CSR name `akshay`
```yaml
---
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: akshay
spec:
  groups:
  - system:authenticated
  request: <Paste the base64 encoded value of the CSR file>
  signerName: kubernetes.io/kube-apiserver-client
  usages:
  - client auth
```
with encoded csr
```yaml
apiVersion: certificates.k8s.io/v1
kind: CertificateSigningRequest
metadata:
  name: akshay
spec:
  groups:
  - system:authenticated
  request: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURSBSRVFVRVNULS0tLS0KTUlJQ1ZqQ0NBVDRDQVFBd0VURVBNQTBHQTFVRUF3d0dZV3R6YUdGNU1JSUJJakFOQmdrcWhraUc5dzBCQVFFRgpBQU9DQVE4QU1JSUJDZ0tDQVFFQTFlVjVmVWFQK2ZxRmNLMTdzUVI0aDdZaExTVEQxTWVoK3czRityV2xwdmx2Cm1EQzF5QzVYWDhxSjVVcWM4Tk5tY0FPRXVQczl5RWZ4QXJnaTE3NWJjMURTVGJTdFd6Uy85UVJQb2VHeHJkQkEKSVozOU5oKzlMb2NjOVlQTUJGNWMzd3ZVblNodGtwQ0RwUm9pZjNZbjZXSllNV2hGcHRHdHg2TG5HWFpDWkozUwowRGhEK1EyelBBUEhPUjd5Vm5VQ1g3TG93SDhMY2dEdm9WYURuNlFQd09vaW51YlJpSkc4WGRMSDVlOTNObU45CmJ1dW9ER1FvdG1EaEdBRzlheHdreEQyQlJFOWlXSEV4R1RWaVA3RGwzcERTZDhzNFhqWUtQU2VzYTA0dWZWN0kKdFRCMjhQWW43VFpVLzRjMGlXNGlQTlpuTTd2UmduSU02U2ladjUxWnFRSURBUUFCb0FBd0RRWUpLb1pJaHZjTgpBUUVMQlFBRGdnRUJBTHh1ejh1eUFqS2gxWWhDZ3RTRTlvaFlFMy9BZzJMdmVlaUhqZ2xsQ2x4NzBZdEFSeUNxCitJT2l0UVpXQ0VXUnc4OGZCZDlmMEFia2pFelMvTjRCQm9NVmJFRm9zbVA1MndhV1l3TzdMVXgxZ0o1TWRLbUwKNG9Pd283blE3QXFmb2VrNFFsOXR6SUZ6ZkYyMU9ydC8zb28wWnZ1eTRuZFRJSUpGeHg5MlF4VHZPT1M3N1Y5bgpDY09kendBNE1pZ21KRW1Xa2xUYVBJdGRGNEt4UlVsaXVFRjhEbjBGcDdZak8xZVFNVEtYa1JoME5pNGxQSXdxCnB3U2dBcmRNWHlxVTAvdHRZKzNHUjdWMG1pZ0V5R0ZSWjdPalYrV1lGbktXNW1jMDF4RmNLcmg3TDlGbTlTRFkKU2h6MUNxSFU1SkR3Zm03clR3MEg1MnBsQy9lQmpKZWdsNFk9Ci0tLS0tRU5EIENFUlRJRklDQVRFIFJFUVVFU1QtLS0tLQo=
  signerName: kubernetes.io/kube-apiserver-client
  usages:
  - client auth
```
```bash
kubectl apply -f akshay-csr.yaml

kubectl get csr  

kubectl certificate approve akshay  

kubectl get csr agent-smith -o yaml  

kubectl certificate deny agent-smith  

kubectl delete csr agent-smith
```
