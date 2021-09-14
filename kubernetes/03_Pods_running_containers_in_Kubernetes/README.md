# Del 3: Pods

## Creating pods from YAML 
- To create the pod from your YAML file, use the kubectl create command: `kubectl apply -f ex01-simple-server-manual.yaml`
- Retrieving the whole definition of a running pod: 
    - `kubectl get pod simple-server-manual -o yaml`
    - `kubectl get pod simple-server-manual -o json`
- List the pods: `kubectl get pods`
- Viewing application logs: `kubectl logs simple-server-manual`
- Specifying the container name when getting logs of a multi-container pod: `kubectl logs simple-server-manual -c simple-server`

## Sending requests to the pod
- Forwarding a local network port to a port in the pod: `kubectl port-forward simple-server-manual 7000:8080`
- Connecting to the pod through the port forwarder: `curl localhost:7000`

## Specifying labels when creating a pod
- Create a new pod: `kubectl apply -f ex02-simple-server-manual-with-labels.yaml`
- List pods with lables: `kubectl get pod --show-labels`
- List pods and show the columns for the labels: `kubectl get pod -L creation_method,env`

## Modifying labels of existing pods
- Add a lable on a existing pod: `kubectl label pod simple-server-manual creation_method=manual`
- Change existing lables: `kubectl label pod simple-server-manual-v2 env=debug --overwrite`
- List pods and show the columns for the labels: `kubectl get pod -L creation_method,env`

## Stopping and removing pods
- Deleting a pod by name: `kubectl delete pod simple-server-manual-v2`
- Deleting pods using label selectors: `kubectl delete pod -l creation_method=manual`
- Deleting (almost) all resources in a namespace: `kubectl delete all --all`
