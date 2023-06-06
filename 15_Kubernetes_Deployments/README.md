# Del 6: Deployments
- Deployment resource sits on top of ReplicaSets and enables declarative application updates.

## Creating a Deployment
- Build and push the image:
    - `cd simple-server-v1/`
    - `docker build -t leon11sj/simple-server:v1 .`
    - `docker push leon11sj/simple-server:v1`
    - `cd ..`
- `watch -n 1 'kubectl get pods; kubectl get rs; kubectl get deployment'`
- Creating the Deployment resource: `kubectl apply -f ex01-simple-server-deployment-v1.yaml`
- Displaying the status of the deployment rollout: `kubectl rollout status deployment simple-server`
- List pods: `kubectl get pods`
- Look at the ReplicaSet created by your Deployment: `kubectl get replicasets`

## Updating a Deployment
- Slowing down the rolling update for demo purposes: `kubectl patch deployment simple-server -p '{"spec": {"minReadySeconds": 10}}'`
- Run the service: `kubectl apply -f ex01-service.yaml`
- First run the curl loop again in another terminal to see what’s happening with the requests: `while true; do curl http://<IP>:<PORT>/; sleep 0.5; done`
- Build and push the image:
    - `cd simple-server-v2/`
    - `docker build -t leon11sj/simple-server:v2 .`
    - `docker push leon11sj/simple-server:v2`
- Triggering the rolling update:
    - `kubectl set image deployment simple-server nodejs=leon11sj/simple-server:v2`
- List ReplicaaSets: `kubectl get rs`

## Rolling back a deployment
- Build and push the image:
    - `cd simple-server-v3/`
    - `docker build -t leon11sj/simple-server:v3 .`
    - `docker push leon11sj/simple-server:v3`
- Deploying version 3: `kubectl apply -f ex03-simple-server-deployment-v3.yaml`
- You can follow the progress of the rollout with kubectl rollout status: `kubectl rollout status deployment simple-server`
- As the following listing shows, after a few requests, your web clients start receiving errors.
- Undoing a rollout: `kubectl rollout undo deployment simple-server`

## Blocking rollouts of bad versions
- Change to version 2: `kubectl apply -f ex02-simple-server-deployment-v2.yaml`
- Updating a Deployment with kubectl apply: `kubectl apply -f ex03-simple-server-deployment-v3-with-readinesscheck.yaml`
- Show rollout status: `kubectl rollout status deployment simple-server` 
- List the pods: `kubectl get po`
- Aborting a bad rollout: `kubectl rollout undo deployment simple-server`
- Delete: 
    - `kubectl delete service simple-server`
    - `kubectl delete deployments simple-server`

