Manages labels on Kubernetes objects. Used to identify a group of objects.

## Example

Add a label
```bash
kubectl label pods 'PODNAME' color=green
kubectl label deployments 'DEPLOYMENT' color=green
```

Overwrite an existing label
```bash
kubectl label pods 'PODNAME' color=green --overwrite
kubectl label deployments 'PODNAME' color=green --overwrite
```

Remove a label
```bash
kubectl label pods 'PODNAME' color-
kubectl label deployments 'DEPLOYMENT' color-
```

## Usage

List Labels
```bash
kubectl get pods --show-labels
kubectl get deployments --show-labels
```

Filter by label
```bash
kubectl get pods -L color=green
kubectl get deployments -L color=green
kubectl get pods --selector='app=appname,ver=2'
kubectl get pods --selector='app in (appname1, appname2)'
kubectl get pods --selector='isprod' # filter if label exists (or label=true)
```

| Operator                     | Description                 |
| ---------------------------- | --------------------------- |
| `key=value`                  | key set to value            |
| `key!=value`                 | key not set to value        |
| `key in (value1, value2)`    | key set to either value     |
| `key notin (value1, value2)` | key not set to either value |
| `key`                        | key is set/present          |
| `!key`                       | key is not set/present      |

## Hint

A label can also be applied using [[kubectl run]] or a [[Kubernetes Manifest]].

## See also
- [[kubectl annotate]]