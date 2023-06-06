# Volumes

## Using volumes to share data between containers
- An emptyDir volume is especially useful for sharing files between containers running in the same pod.
- Build the image:
    - `cd fortune/`
    - `docker build -t leon11sj/fortune .`
    - `docker push leon11sj/fortune`
- Creating the pod: `kubectl apply -f ex01-fortune-pod.yaml`
- Forward a port from your local machine to the pod: `kubectl port-forward fortune 8080:80`
- Now you can access the Nginx server through port 8080 of your local machine. Use curl to do that: `curl http://localhost:8080`
- Delete: `kubectl delete pod fortune`

## Decoupling pods from the underlying storage technology
- K3s comes with Rancher’s Local Path Provisioner and this enables the ability to create persistent volume claims out of the box using local storage on the respective node.
- Run: `kubectl apply -f ex06-mongodb-pod-k3s.yaml`
- Get info: 
    - `kubectl get pod`
    - `kubectl get pv`
    - `kubectl get pvc`
- Save some data:
    - `kubectl exec -it mongodb -- mongo`
    - `use mystore`
    - `db.foo.insert({name:'foo'})`
    - `db.foo.find()`
    - `exit`
    - The document should be stored in your persistent disk now.
- Re-creating the pod and verifying that it can read the data persisted by the previous pod:
    - `kubectl delete pod mongodb`
    - `kubectl apply -f ex06-mongodb-pod-k3s.yaml`
    - `kubectl exec -it mongodb -- mongo`
    - `use mystore`
    - `db.foo.find()`
    - `exit`
- Delete: `kubectl delete -f ex06-mongodb-pod-k3s.yaml`