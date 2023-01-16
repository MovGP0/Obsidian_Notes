Namespaces are used to group related objects.

List namespaces using `kubectl get namespace`

## Default namespaces

| Namespace         | Description                                          |
| ----------------- | ---------------------------------------------------- |
| `default`         | User services which are not in a custom namespaces   |
| `kube-node-lease` | Manages leases for service heartbeat detection       |
| `kube-public`     | Data that can be read by all clients; ie. usage data |
| `kube-system`     | Objects created by the K82 System                    |

## Examples

Start an docker image in a namespace
```bash
kubectl run nginx --image=nginx --namespace=NAMESPACE
```
The service will get a DNS name of `SERVICE.NAMESPACE.svc.cluster.local`

Get all containers in a given namespace
```bash
kubectl get pods --namespace=NAMESPACE
```

## See also
- [Namespaces](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/)