# TLS Introduction

```bash
#asymmetric encryption - ssh
## id_rsa id_rsa.pub
ssh-keygen

ssh -i id_rsa user@server1

openssl genrsa -out my-bank.key 1024

openssl rsa -in my-bank.key -oubout > mybank.pem

#certificate signing request
openssl req -new -key my-bank.key -out my-bank.csr -subj "/C=US/ST=CA/O=MyOrg, Inc./CN=my-bank.com"

```
