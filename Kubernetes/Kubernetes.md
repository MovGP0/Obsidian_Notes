| Name                   | Short  | Description                        |
| ---------------------- | ------ | ---------------------------------- |
| `Kubernetes`           | `k8s`  | 8 Buchstaben zwischen `k` und `s`  |
| `internationalization` | `i18n` | 18 Buchstaben zwischen `i` und `n` |
| `localization`         | `l10n` | 10 Buchstaben zwischen `l` und `n` |

## Kubernetes cluster components
| Name                   | Description                                                                                |
| ---------------------- | ------------------------------------------------------------------------------------------ |
| `controller-manager`   |                                                                                            |
| `scheduler`            |                                                                                            |
| `etcd-0`               | Key-value store for cluster data                                                           |
| `kube-proxy`           | Network-Routing-Proxy; must be active on all nodes                                         |
| `kube-dns`             | DNS Server for service naming and discovery; has load-balancing service with the same name |
| `kubernetes-dashboard` | user interface; start using `kubectl proxy`; has loadbalancing service with the same name  |

## See also

- [[kubectl]] - Kubernetes CLI
- [[Kubernetes Architecture]]