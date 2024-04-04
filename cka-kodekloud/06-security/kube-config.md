# Kube Config

 ```bash
 kubectl get pods --kubeconfig config

 kubectl config view

 kubectl config view --kubeconfig=my-custom-config

 # Update / Change context
 kubectl config use-context prod-user@production

 kubectl config -h

 # To use context, run the command
 # since kubeconfig is not on default location ~/.kube/config. we are providing path to it
 kubectl config --kubeconfig=/root/my-kube-config use-context research

 #To know the current context, run the command
 kubectl config --kubeconfig=/root/my-kube-config current-context
 ```
