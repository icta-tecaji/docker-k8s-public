# Del 1: Lokalna namestitev K3s

- [Offical install guide](https://rancher.com/docs/k3s/latest/en/quick-start/)

## Master node
K3s provides an installation script that is a convenient way to install it as a service on systemd or openrc based systems. This script is available at https://get.k3s.io. To install K3s using this method, just run:
- `curl -sfL https://get.k3s.io | K3S_KUBECONFIG_MODE=644 sh - `

After running this installation:
- The K3s service will be configured to automatically restart after node reboots or if the process crashes or is killed
- Additional utilities will be installed, including kubectl, crictl, ctr, k3s-killall.sh, and k3s-uninstall.sh
- A kubeconfig file will be written to /etc/rancher/k3s/k3s.yaml and the kubectl installed by K3s will automatically use it


## Worker nodes

Napišemo kako lahko dodamo dodatne node

curl -sfL https://get.k3s.io | K3S_URL=https://localhost:6443 K3S_TOKEN=K10ae0d29c5a0549181f1fdf8bf8763beeeaf5b7916ef793592eb36d3c296e62065::server:822ad0e0b793aad4cb1d75f20dbadff4 K3S_NODE_NAME=worker-node1 sh -