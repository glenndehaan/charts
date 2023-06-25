# traefik-helpers

![Version: 1.0.1](https://img.shields.io/badge/Version-1.0.1-informational?style=flat-square) ![Type: application](https://img.shields.io/badge/Type-application-informational?style=flat-square) ![AppVersion: 1.0.0](https://img.shields.io/badge/AppVersion-1.0.0-informational?style=flat-square)

A Helm chart with useful Traefik middlewares, helpers and default configuration

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
| fullnameOverride | string | `""` | String to fully override names.fullname |
| ingress.httpRedirects | object | `{"enabled":true}` | Redirects all http traffic to https without www |
| ingress.httpsRedirects | object | `{"enabled":true}` | Redirects all https traffic but strips the www if present |
| middlewares.compress.enabled | bool | `true` | Toggles the middleware |
| middlewares.compress.options | object | `{}` | Treafik compress middleware options, reference: https://doc.traefik.io/traefik/middlewares/http/compress/ |
| middlewares.ipWhitelist.enabled | bool | `true` | Toggles the middleware |
| middlewares.ipWhitelist.options | object | `{}` | Treafik compress middleware options, reference: https://doc.traefik.io/traefik/middlewares/http/ipwhitelist/ |
| middlewares.rateLimit.enabled | bool | `true` | Toggles the middleware |
| middlewares.rateLimit.options | object | `{"average":100,"burst":50}` | Treafik compress middleware options, reference: https://doc.traefik.io/traefik/middlewares/http/ratelimit/ |
| middlewares.secureHeaders.enabled | bool | `true` | Toggles the middleware |
| nameOverride | string | `""` | String to partially override names.fullname |
| serviceMonitor.enabled | bool | `true` | Toggles the service monitor |
| tlsOption.enabled | bool | `true` | Toggles the TLS option |

----------------------------------------------
Autogenerated from chart metadata using [helm-docs v1.11.0](https://github.com/norwoodj/helm-docs/releases/v1.11.0)