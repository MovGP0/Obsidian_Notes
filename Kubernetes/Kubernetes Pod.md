| Command                         | Description                                                                      |
| ------------------------------- | -------------------------------------------------------------------------------- |
| `kubectl get pods`              | List all pods                                                                    |
| `kubectl describe pods PODNAME` | events |

## Examples

Get IP-Address of specific pod using a [[JSON Path]]
```bash
kubectl get pods PODNAME --output=jsonpath --template={.status.podIP}
```
