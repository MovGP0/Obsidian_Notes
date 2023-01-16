Manages labels on Kubernetes objects.

## Example

Add a label
```bash
kubectl label pots PODNAME color=red
```

Overwrite an existing label
```bash
kubectl label pots PODNAME color=green --overwrite
```

Remove a label
```bash
kubectl label pots PODNAME color-
```

## See also
- [[kubectl annotate]]