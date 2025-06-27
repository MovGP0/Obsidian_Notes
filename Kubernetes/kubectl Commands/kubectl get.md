| Command                               | Description                                    |
| ------------------------------------- | ---------------------------------------------- |
| `kubectl get RESOURCENAME`            | information about a resource                   |
| `kubectl get RESOURCENAME OBJECTNAME` | Gets information about an object in a resource |

## Examples

List all [[Kubernetes Pod]] in the default namespace
```bash
kubectl get pods
```

Get information about a specific [[Kubernetes Pod]]
```bash
kubectl get pods PODNAME
```

List all [[Kubernetes Namespace]]
```bash
kubectl get namespace
```

Get status of the K8s cluster services
```bash
kubectl get componentstatuses
```

## See also
- [[kubectl describe]]
- [[kubectl set]]