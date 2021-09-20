# Omejevanje virov

## Requesting resources for a pod’s containers
- Run: `kubectl apply -f ex01-requests-pod.yaml`
- Take a quick look at the process’ CPU consumption by running the top command inside the container: `kubectl exec -it requests-pod -- top`
- Inspecting a node’s capacity: `kubectl describe nodes`
- Run: `kubectl run requests-pod-2 --image=busybox --restart Never --requests='cpu=500m,memory=20Mi' -- dd if=/dev/zero of=/dev/null`
- Let’s see if it was scheduled: `kubectl get pod`
- Creating a pod that doesn’t fit on any node: `kubectl run requests-pod-3 --image=busybox --restart Never --requests='cpu=4,memory=20Mi' -- dd if=/dev/zero of=/dev/null`
- `kubectl get po requests-pod-3`
- `kubectl describe po requests-pod-3`
- `kubectl delete po requests-pod-3`
- `kubectl delete po requests-pod-2`
- `kubectl delete po requests-pod`

## Limiting resources available to a container
- Run: `kubectl apply -f ex02-limited-pod.yaml`
- Take a quick look at the process’ CPU consumption by running the top command inside the container: `kubectl exec -it limited-pod -- top`
- Delete: `kubectl delete po limited-pod`
