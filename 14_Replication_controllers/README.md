# Replication controllers

## Defining a ReplicaSet
- Run: `kubectl apply -f ex03-simple-server-replicaset.yaml`
- Examine the ReplicaSet:
    - `kubectl get rs`
    - `kubectl describe rs simple-server`
- List the pods: `kubectl get pods`
- Seeing the ReplicaSet respond to a deleted pod: `kubectl delete pod <POD_NAME>`
- Listing the pods again shows four of them, because the one you deleted is terminating, and a new pod has already been created: `kubectl get pods`. 
- Getting information about a ReplicaSet:
    - `kubectl get rs`
    - `kubectl describe rs simple-server`

## Horizontally scaling pods
- Scaling up a replicationcontroller: 
    - `kubectl scale rs simple-server --replicas=10`
    - `kubectl scale rs simple-server --replicas=5`
- Scaling a ReplicaSet by editing its definition:
    - Scale it in a declarative way by editing the ReplicaSet definition: `kubectl edit rs simple-server`
    - When the text editor opens, find the spec.replicas field and change its value to 3.
    - `kubectl get rs`

- Delete ReplicaSet: `kubectl delete rs simple-server`
