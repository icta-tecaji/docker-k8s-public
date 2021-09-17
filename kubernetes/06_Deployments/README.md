# Del 6: Deployments

## Creating a Deployment
- Build and push the image:
    - `docker build -t leon11sj/simple-server:v1 .`
    - `docker push leon11sj/simple-server:v1`
- Creating the Deployment resource: `kubectl apply -f ex01-simple-server-deployment-v1.yaml --record`


## 2048 Game deploy
- To deploy a game called 2048 as a sample application, run the following commands:
    - `kubectl apply -f deployment-2048.yaml`
    - `kubectl apply -f node-port-service-2048.yaml`
    - `kubectl apply -f ingress-2048.yaml`
- To verify that the Ingress resource was created, wait a few minutes, and then run the following command:
    - `kubectl get ingress`
- Delete: `kubectl delete -f .`