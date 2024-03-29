# sunflare-tools

![Version: 1.1.0](https://img.shields.io/badge/Version-1.1.0-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: latest](https://img.shields.io/badge/AppVersion-latest-informational?style=flat-square)

A Helm chart for deploying the sunflare tools

**Homepage:** <https://github.com/glenndehaan/charts>

## Maintainers

| Name | Email | Url |
| ---- | ------ | --- |
| glenndehaan | <glenn@dehaan.cloud> | <https://glenndehaan.com> |

## Source Code

* <https://github.com/glenndehaan/charts>

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| affinity | object | `{}` | Affinity for pod assignment |
| fullnameOverride | string | `""` | String to fully override names.fullname |
| image.pullPolicy | string | `"Always"` | sunflare-tools image pull policy |
| image.repository | string | `"glenndehaan/sunflare-tools"` | sunflare-tools image repository |
| image.tag | string | `""` | Overrides the image tag whose default is the chart appVersion. |
| imagePullSecrets | list | `[]` | Specify docker-registry secret names as an array |
| ingress.annotations | object | `{}` | Additional annotations for the Ingress resource. To enable certificate autogeneration, place here your cert-manager annotations. |
| ingress.className | string | `""` | Set the ingressClassName on the ingress record for k8s 1.18+ |
| ingress.enabled | bool | `false` | Set to true to enable ingress record generation |
| ingress.hosts[0] | object | `{"host":"sunflare-tools.local","paths":[{"path":"/","pathType":"ImplementationSpecific"}]}` | Default host |
| ingress.tls | list | `[]` | TLS secret configuration |
| nameOverride | string | `""` | String to partially override names.fullname |
| nodeSelector | object | `{}` | Node labels for pod assignment. Evaluated as a template. |
| podAnnotations | object | `{}` | Annotations for sunflare-tools pods |
| podSecurityContext | object | `{}` | Pod Security Context for sunflare-tools pods |
| replicaCount | int | `2` | Number of sunflare-tools replicas to deploy |
| resources | object | `{"limits":{"memory":"25Mi"},"requests":{"memory":"25Mi"}}` | Resources for pods. Evaluated as a template. |
| revisionHistoryLimit | int | `1` | Number of sunflare-tools revisions to keep |
| securityContext | object | `{"allowPrivilegeEscalation":false,"capabilities":{"drop":["ALL"]},"privileged":false,"readOnlyRootFilesystem":true}` | Security Context for sunflare-tools |
| service.port | int | `8080` | Service HTTP port |
| service.type | string | `"ClusterIP"` | Service type |
| serviceAccount.annotations | object | `{}` | Annotations to add to the service account |
| serviceAccount.create | bool | `true` | Specifies whether a service account should be created |
| serviceAccount.name | string | `""` | The name of the service account to use. If not set and create is true, a name is generated using the fullname template |
| sunflare.logLevel | string | `"info"` | sunflare-tools Logger Level |
| sunflare.tools | list | `["ip","whoami","password","uuid","dns","host"]` | Sunflare tools that will be installed |
| tolerations | list | `[]` | Tolerations for pod assignment. Evaluated as a template. |

----------------------------------------------
Autogenerated from chart metadata using [helm-docs v1.11.0](https://github.com/norwoodj/helm-docs/releases/v1.11.0)
