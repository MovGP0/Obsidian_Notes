Executes a command in a running container

## Examples

Get date from the first container in a pod
```bash
kubectl exec 'PODNAME' -- 'date'
```

Start `bash` within a specific container; redirect input from console
```bash
kubectl exec 'PODNAME' --container='CONTAINERNAME' --stdin=false --tty=false -- 'bash'
```
