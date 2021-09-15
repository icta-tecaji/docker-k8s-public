# Del 4: Replikacijski in ostali kontrolerji

## Creating an HTTP-based liveness probe
- Build the image: `docker build -t leon11sj/simple-server-unhealthy .`
- Push the image to the repository: `docker push leon11sj/simple-server-unhealthy`
- Run the pod: `kubectl apply -f simple-server-liveness-probe.yaml`
- Check the restart counter: `kubectl get pod simple-server-liveness`
- More info: `kubectl describe pod simple-server-liveness`

## Creating a ReplicationController
- Run the replicationcontroller: `kubectl apply -f ex02-simple-server-rc.yaml`
- List the pods: `kubectl get pods`
- Seeing the replicationcontroller respond to a deleted pod: `kubectl delete pod <POD_NAME>`
- Listing the pods again shows four of them, because the one you deleted is terminating, and a new pod has already been created: `kubectl get pods`. 
- Getting information about a replicationcontroller:
    - `kubectl get rc`
    - `kubectl describe rc simple-server`

## Horizontally scaling pods
- Scaling up a replicationcontroller: `kubectl scale rc simple-server --replicas=10`
- Scaling a replicationcontroller by editing its definition:
    - Scale it in a declarative way by editing the ReplicationController’s definition: `kubectl edit rc simple-server`
    - When the text editor opens, find the spec.replicas field and change its value to 3.
    - `kubectl get rc`

## Deleting a ReplicationController
- `kubectl delete rc simple-server`

## Defining a ReplicaSet
- Run: `kubectl apply -f ex03-simple-server-replicaset.yaml`
- Examine the ReplicaSet:
    - `kubectl get rs`
    - `kubectl describe rs simple-server`
- Delete ReplicaSet: `kubectl delete rs simple-server`