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



```
