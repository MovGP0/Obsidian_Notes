Manages annotations on Kubernetes objects.

## Example

Add an annotation
```bash
kubectl annotate pots PODNAME color=red
```

Overwrite an existing annotation
```bash
kubectl annotate pots PODNAME color=green --overwrite
```

Remove an annotation
```bash
kubectl annotate pots PODNAME color-
```

## See also
- [[kubectl label]]