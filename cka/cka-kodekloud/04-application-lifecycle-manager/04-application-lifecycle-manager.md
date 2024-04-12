 ```bash
 #rollout command
 kubectl rollout status deployment/myapp-deployment

 kubectl rollout history deployment/myapp-deloyment

 kubectl create -f deployment-definition.yml

 kubectl get deployments

 #rolling updates and rollbacks
 kubectl apply -f deployment-definition.yml

 kubectl set image deployment/myapp-deployment nginx-container=nginx:1.9.1

 kubectl rollout status deployment/myapp-deployment

 kubectl describe deployment myapp-deployment

 kubectl rollout undo deployment myapp-deployment

 kubectl edit deployment frontend
 ```
