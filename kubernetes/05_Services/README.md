# Del 5: Services

## Introducing services
- Create the ReplicationController: `kubectl apply -f ex01-simple-server-replicaset.yaml`
- Create the service: `kubectl apply -f ex02-simple-server-svc.yaml`
- List all Service resources: `kubectl get svc`
- Remotely executing commands in running containers: `kubectl exec simple-server-tq49v -- curl -s http://10.43.10.125`
- Delete service: `kubectl delete svc simple-server-svc`
- Delete ReplicaSet: `kubectl delete rs simple-server`

## Using a NodePort service
- Create the ReplicationController: `kubectl apply -f ex01-simple-server-replicaset.yaml`
- Creating a NodePort service: `kubectl apply -f ex04-simple-server-svc-nodeport.yaml`
- Let’s see the basic information of your service to learn more about it: `kubectl get svc simple-server-nodeport`
- Delete nodePort service: `kubectl delete svc simple-server-nodeport`

## Creating an Ingress resource
- Creating a NodePort service: `kubectl apply -f ex04-simple-server-svc-nodeport.yaml`
- Create an Ingress: `kubectl apply -f ex06-simple-server-ingress.yaml`
- To look up the IP, you need to list Ingresses: `kubectl get ingresses`
- Ensuring the host configured in the Ingress points to the Ingress’ IP address
    - Once you know the IP, you can then either configure your DNS servers to resolve simple-server.example.com to that IP or you can add the following line to /etc/hosts (or C:\windows\system32\drivers\etc\hosts on Windows)
- Delete Ingress service: `kubectl delete ingress simple-server-ingress`
- Delete nodePort service: `kubectl delete svc simple-server-nodeport`
- Delete ReplicaSet: `kubectl delete rs simple-server`