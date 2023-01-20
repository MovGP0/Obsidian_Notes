Creates a new [[Kubernetes Pod]].

Use [[kubectl apply]] to create pods using a [[Kubernetes Manifest]].

## Example

Start a new pod with a container based on an docker image.
```bash
kubectl run 'PODNAME' \
    --image="DOCKERIMAGENAME" \
    --replicas 2 \
    --labels="app.version=1.0,app=foobar,env=prod"
```

List pods using [[kubectl get]]
```bash
kubectl get pods
```

Delete pods using [[kubectl delete]]
```bash
kubectl delete deployments/PODNAME
```
