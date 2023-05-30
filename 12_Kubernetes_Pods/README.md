# Kubernetes Pods

## Pod theory
- The atomic unit of scheduling in the virtualization world is the **Virtual Machine (VM)**. This means deploying applications in the virtualization world is done by scheduling them on VMs.
- In the Docker world, the atomic unit is the **container**. This means deploying applications on Docker is done by scheduling them inside of containers.
- In the Kubernetes world, the atomic unit is the **Pod**. Ergo, deploying applications on Kubernetes means stamping them out in Pods.

A Pod is the **basic execution unit of a Kubernetes application–the smallest and simplest unit in the Kubernetes object model that you create or deploy**. 
- A Pod represents processes running on your Cluster.
- Kubernetes wraps the container in the Pod.
- A Pod is a unit of compute, which runs on a **single node** in the cluster.
- A Pod is a shared execution environment for **one or more containers**.

## Understanding pods
- One container shouldn’t contain multiple processes
- How a pod combines multiple containers
- Splitting a multi-tier application stack into multiple pods

## Sidecar containers
Placing several containers in a single pod is only appropriate if the application consists of a primary process and one or more processes that complement the operation of the primary process. **The container in which the complementary process runs is called a sidecar container**.

When deciding whether to use the sidecar pattern and place containers in a single pod, or to place them in separate pods, ask yourself the following questions:
- Do these containers have to run on the same host?
- Do I want to manage them as a single unit?
- Do they form a unified whole instead of being independent components?
- Do they have to be scaled together?
- Can a single node meet their combined resource needs?

If the answer to all these questions is yes, put them all in the same pod. As a rule of thumb, **always place containers in separate pods unless a specific reason requires them to be part of the same pod.**

## Creating pods

### Creating a YAML manifest for a pod
- `cd ~/docker-k8s-public/18_Kubernetes_Pods/examples/`
- `cat 01_example.yaml`
- `kubectl apply -f 01_example.yaml`
- `kubectl get pod myapp -o yaml`

Let’s use the basic kubectl commands to see how the pod is doing:
- `kubectl get pod`
- `kubectl get pod -o wide`
- `kubectl describe pod myapp`

The following listing shows all the events that were logged after creating the pod.
- `kubectl get events`

1. GETTING THE POD’S IP ADDRESS:
    - `kubectl get pod myapp -o wide`
2. GETTING THE PORT THE APPLICATION IS BOUND TO:
3. CONNECTING TO THE POD FROM THE WORKER NODES:
    - `curl <IP>:8080`
4. CONNECTING FROM A ONE-OFF CLIENT POD:
    - `kubectl run --image=curlimages/curl -it --restart=Never --rm client-pod curl <IP>:8080`
5. CONNECTING TO PODS VIA KUBECTL PORT FORWARDING:
    - `kubectl port-forward myapp 8080`
    - `curl localhost:8080`

## Viewing application logs
- [kubectl logs](https://jamesdefabia.github.io/docs/user-guide/kubectl/kubectl_logs/)

Run:
- `kubectl logs myapp`
- `kubectl logs myapp -f`
- Press ctrl-C to stop streaming the log when you’re done.
- `kubectl logs myapp --timestamps`
- Kubernetes keeps a separate log file for each container. They are usually stored in `/var/log/containers` on the node that runs the container. 

## Copying files to and from containers
For example, to copy the `/etc/hosts` file from the container of the pod to the `/tmp` directory on your local file system, run the following command:
- `kubectl cp myapp:/etc/hosts /tmp/myapp-hosts`
- `cat /tmp/myapp-hosts`
- `rm /tmp/myapp-hosts`

To copy a file from your local file system to the container, run the following command:
- `kubectl cp /path/to/local/file myapp:path/in/container`

## Executing commands in running containers
1. INVOKING A SINGLE COMMAND IN THE CONTAINER:
    - `kubectl exec myapp -- ps aux`
    - `kubectl exec myapp -- curl -s localhost:8080`

2. RUNNING AN INTERACTIVE SHELL IN THE CONTAINER:
    - If you want to run several commands in the container, you can run a shell in the container as follows: `kubectl exec -it myapp -- bash`

> [Ephemeral Containers](https://kubernetes.io/docs/concepts/workloads/pods/ephemeral-containers/): A new Kubernetes feature called ephemeral containers allows you to debug running containers by attaching a debug container to them.

## Running multiple containers in a pod
Build the Envoy proxy container image:
- `cd ~/docker-k8s-public/18_Kubernetes_Pods/examples/02_ssl_proxy`
- `docker build -t leon11sj/myapp-ssl-proxy:1.2 .`
- `docker push leon11sj/myapp-ssl-proxy:1.2`
- `cd ..`

Run the new service:
- `cat 02_example-ssl.yaml`
- `kubectl apply -f 02_example-ssl.yaml`
- `kubectl get pod`
- `kubectl describe pod myapp-ssl`
- On the local machine: `kubectl port-forward myapp-ssl 8080 8443 9901`

Open the URLs:
- `http://localhost:8080`
- `https://localhost:8443`
- `http://localhost:9901/`

Logs:
- `kubectl logs myapp-ssl -c myapp`
- `kubectl logs myapp-ssl -c envoy`
- `kubectl logs myapp-ssl --all-containers`.
- `kubectl exec -it myapp-ssl -c envoy -- bash`

## Running init containers
- [Init Containers](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/)

Build the images:
- `cd ~/docker-k8s-public/18_Kubernetes_Pods/examples/03_init_demo`
- `docker build -t leon11sj/init-demo:1.0 .`
- `docker push leon11sj/init-demo:1.0`
- `cd ~/docker-k8s-public/18_Kubernetes_Pods/examples/03_network_checker/`
- `docker build -t leon11sj/network-checker:1.0 .`
- `docker push leon11sj/network-checker:1.0`
- `cd ..`

Before you create the pod from the manifest file, run the following command in a separate terminal:
- `kubectl get pods -w`
- `kubectl get events -w`
- `kubectl apply -f 03_example_init.yaml`

Run:
- `kubectl logs myapp-init -c network-check`
- `kubectl logs myapp-init -c init-demo`

## Deleting pods and other objects
- `kubectl delete pod myapp`

You can also delete multiple pods with a single command: 
- `kubectl delete pod <POD1> <POD2>`
- `kubectl delete -f 02_example-ssl.yaml`

Instead of deleting these pods by name, we can delete them all using the `--all` option:
- `kubectl delete pod --all`
- `kubectl get pod`

