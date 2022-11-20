# Introduction to Kubernetes

### K3s installation
- `curl -sfL https://get.k3s.io | K3S_KUBECONFIG_MODE="644" sh -`
- Check the version: `k3s --version`
- Run the following command to check that your cluster is up and running: `kubectl get nodes -o wide`
- To verify your cluster is working: `kubectl cluster-info`
- [Quick-Start Guide](https://docs.k3s.io/quick-start)

## Deploying your first application
- `cd ~/docker-k8s/17_Introduction_to_Kubernetes/examples/test-app`
- `kubectl create deployment test --image=leon11sj/test-app`
- `kubectl get deployments`
- `kubectl get pods`
- `kubectl expose deployment test --type=LoadBalancer --port 8080`
- `kubectl get svc`
- `kubectl scale deployment test --replicas=5`
- `kubectl get deploy`
- `kubectl get pods -o wide`
- Try: `curl <IP>:<PORT>`

Remove the resources:
- `kubectl delete svc test`
- `kubectl delete deploy test`
- `kubectl get pods`
