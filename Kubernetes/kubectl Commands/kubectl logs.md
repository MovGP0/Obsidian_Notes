Prints the log from a given Kubernetes container.

## Example

Print the log from a pod
```bash
kubectl logs 'PODNAME'
```

Print the log from a container within a pod
```bash
kubectl logs 'PODNAME' --container='CONTAINERNAME'
```

Follow the log
```bash
kubectl logs 'PODNAME' -c 'CONTAINERNAME' -f
kubectl logs 'PODNAME' --container='CONTAINERNAME' --follow=true
```
