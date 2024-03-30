```bash
#Generate keys
openssl genrsa -out ca.key 2048

#Certificate signing request
openssl req -nww -key ca.key -subj "/CN=KUBERNETES-CA" -out ca.cs


```
