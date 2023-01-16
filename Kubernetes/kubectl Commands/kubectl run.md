Creates a new [[Kubernetes Pod]].

Use [[kubectl apply]] to create pods using a [[Kubernetes Pod Manifest]].

## Example

Start a new pod with a container based on an docker image.
```bash
kubectl run PODNAME --image='DOCKERIMAGENAME'
```

List pods using [[kubectl get]]
```bash
kubectl get pods
```

Delete pods using [[kubectl delete]]
```bash
kubectl delete deployments/PODNAME
```
