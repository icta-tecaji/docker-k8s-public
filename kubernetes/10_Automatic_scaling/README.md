# Avtomatsko skaliranje

## Horizontal pod autoscaling
- Creating a HorizontalPodAutoscaler based on CPU usage: `kubectl apply -f ex01-scaling-deployment.yaml`
- Check: `kubectl get deploy` & `kubectl get pods`
- Use the kubectl autoscale command: `kubectl autoscale deployment simple-server --cpu-percent=30 --min=1 --max=10`
- Let’s look at the definition of the HorizontalPodAutoscaler resource:
    - `kubectl get hpa`
    - `kubectl get hpa simple-server -o yaml`
- It soon scales the Deployment down to a single replica: `kubectl get deployment`
- You can use kubectl describe to see more information on the HorizontalPod-Autoscaler: `kubectl describe hpa`
- Triggering a scale-up:
    - `kubectl expose deployment simple-server --port=80 --target-port=8080 --name=simple-server-service --type=NodePort`
    - `kubectl get svc`
    - Run the following command in a separate terminal to keep an eye on what’s happening with the HorizontalPodAutoscaler and the Deployment: `watch -n 1 kubectl get hpa,deployment,pods`
    - `kubectl run -it --rm --restart=Never loadgenerator --image=busybox -- sh -c "while true; do wget -O - -q http://10.0.2.190:30657/; done"`
- Modifying the target metric value on an existing HPA object: `kubectl edit hpa simple-server`
- Delete: 
    - `kubectl delete svc simple-server-service`
    - `kubectl delete hpa simple-server`
    - `kubectl delete deploy simple-server`