
**somepod.yaml**
```yaml
apiVersion: v1
kind: Pod
metadada:
    name: FOOBAR
spec:
    containers:
      - image: DOCKERIMAGE
        name: FOOBAR
        ports:
            - containerPort: 8080
              name: http
              protocol: TCP
        command: ['command', 'parm1', 'param2', '...']
```

Use [[kubectl apply]] to run the manifest.

## See also

- [Kubernetes YAML Templates](https://github.com/dennyzhang/kubernetes-yaml-templates)