[![Artifact Hub](https://img.shields.io/endpoint?url=https://artifacthub.io/badge/repository/hoppscotch)](https://artifacthub.io/packages/search?repo=hoppscotch)
# Hoppscotch Helm Chart

This repository contains the Helm chart for deploying the Hoppscotch application, an open-source API development ecosystem, on Kubernetes.

## Prerequisites

- Kubernetes cluster (v1.21+)
- Helm (v3.7.0+)

## Installation

1. Add the Helm repository:

   ```bash
   helm repo add hoppscotch https://karimali92.github.io/helmcharts/
   helm repo update
   ```

2. Install the chart with a release name (e.g., `my-hoppscotch`):

   ```bash
   helm install my-hoppscotch hoppscotch/hoppscotch
   ```

3. (Optional) Customize the chart by creating a `values.yaml` file and overriding the default values:

   ```bash
   helm install my-hoppscotch hoppscotch/hoppscotch -f values.yaml
   ```

4. To uninstall:

   ```bash
   helm uninstall my-hoppscotch
   ```

## Values

The following table lists the configurable parameters of the chart and their default values:

| Key                          | Description                                                   | Default                                 |
|------------------------------|---------------------------------------------------------------|-----------------------------------------|
| `replicaCount`               | Number of replicas for the application pod                   | `1`                                     |
| `image.repository`           | Container image repository                                   | `hoppscotch/hoppscotch`                |
| `image.pullPolicy`           | Image pull policy                                            | `IfNotPresent`                          |
| `image.tag`                  | Image tag                                                    | `2024.11.0`                             |
| `imagePullSecrets`           | Secrets for pulling images                                   | `[]`                                    |
| `nameOverride`               | Override the chart name                                      | `""`                                    |
| `fullnameOverride`           | Override the full resource names                            | `""`                                    |
| `serviceAccount.create`      | Create a service account                                     | `true`                                  |
| `serviceAccount.automount`   | Automount the service account                               | `true`                                  |
| `serviceAccount.annotations` | Annotations for the service account                         | `{}`                                    |
| `serviceAccount.name`        | Service account name                                         | `""`                                    |
| `podAnnotations`             | Additional annotations for pods                             | `{}`                                    |
| `podLabels`                  | Additional labels for pods                                  | `{}`                                    |
| `podSecurityContext`         | Pod-level security context                                  | `{}`                                    |
| `securityContext`            | Container-level security context                            | `{}`                                    |
| `service.type`               | Service type                                                | `ClusterIP`                             |
| `service.port`               | Service port                                                | `80`                                    |
| `ingress.enabled`            | Enable ingress                                              | `false`                                 |
| `ingress.className`          | Ingress class name                                          | `nginx`                                 |
| `ingress.annotations`        | Annotations for ingress                                     | `{}`                                    |
| `ingress.hosts`              | List of ingress hosts                                       | See `values.yaml`                       |
| `ingress.tls`                | TLS configuration for ingress                               | `[]`                                    |
| `resources`                  | Resource requests and limits                                | `{}`                                    |
| `livenessProbe`              | Liveness probe configuration                                | See `values.yaml`                       |
| `readinessProbe`             | Readiness probe configuration                               | See `values.yaml`                       |
| `autoscaling.enabled`        | Enable Horizontal Pod Autoscaler                           | `false`                                 |
| `autoscaling.minReplicas`    | Minimum replicas for autoscaling                            | `1`                                     |
| `autoscaling.maxReplicas`    | Maximum replicas for autoscaling                            | `5`                                     |
| `autoscaling.targetCPUUtilizationPercentage` | Target CPU utilization percentage for autoscaling | `80`                                    |
| `volumes`                    | Volumes configuration                                       | See `values.yaml`                       |
| `volumeMounts`               | Volume mounts configuration                                 | See `values.yaml`                       |
| `nodeSelector`               | Node selector for pod scheduling                           | `{}`                                    |
| `tolerations`                | Tolerations for pod scheduling                             | `[]`                                    |
| `affinity`                   | Affinity rules for pod scheduling                          | `{}`                                    |
| `ContainerEnv`               | Additional environment variables for the container          | `[]`                                    |
| `env`                        | Application-specific environment variables                  | `{}`                                    |

## Required Configuration

To ensure the application runs correctly, you must provide the following environment variables under `ContainerEnv`:

```yaml
ContainerEnv:
  - name: ENABLE_SUBPATH_BASED_ACCESS
    value: "true"
  - name: VITE_ALLOWED_AUTH_PROVIDERS
    value: GITHUB
  - name: VITE_BACKEND_GQL_URL
    value: your-domain/backend/graphql
  - name: VITE_BACKEND_WS_URL
    value: wss://your-domain/backend/graphql
  - name: VITE_BACKEND_API_URL
    value: your-domain/backend/v1
  - name: VITE_ADMIN_URL
    value: your-domain/admin
```

Additionally, you can provide other application-specific environment variables under `env`:

```yaml
env:
  DATABASE_URL=postgresql://postgres:testpass@hoppscotch-db:5432/hoppscotch
```

For the complete list of available environment variables, refer to the [`.env.example` file](https://github.com/hoppscotch/hoppscotch/blob/main/.env.example).

## Customization

You can override any value in the `values.yaml` file to customize the deployment. For example, to specify a custom image tag:

```yaml
image:
  tag: "custom-tag"
```

Apply the customization by specifying your custom `values.yaml` during installation:

```bash
helm install my-hoppscotch hoppscotch/hoppscotch -f custom-values.yaml
```

## Contributing

Feel free to open issues or submit pull requests for feature requests or bug fixes.

---

Happy developing with Hoppscotch!
