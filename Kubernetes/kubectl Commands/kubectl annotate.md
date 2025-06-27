Manages annotations on Kubernetes objects. Annotations povide information about:
- where an object comes from
- how to use it
- policies for the object

Annotations are used by automated tools via the [[Kubernetes API]]. Humans should use [[kubectl label]] instead.

Values can contain arbitrary data (ie. JSON/YAML).

## Use cases

- reason for update of an object
- information about tools used to create the object
- communicate scheduling or access policy
- build information (git hash, timestamp)
- track [[Kubernetes ReplicaSet]] for deployment objects
- link to an icon (or base64 encoded icon)
- track rollout status and rollback information

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