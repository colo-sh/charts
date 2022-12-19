# Colo v0.1.0

## Configuration

Note that installing this chart directly means it will use the default template values for Colo.

You may have to set your specific configurations if it is deployed into a production cluster, or you want to configure feature-gates.

### Optional: chart parameters

The following table lists the configurable parameters of the chart and their default values.

| Parameter                                 | Description                                                      | Default                         |
| ----------------------------------------- | ---------------------------------------------------------------- | ------------------------------- |
| `featureGates`                            | Feature gates for Colo, empty string means all by default        | ` `                             |
| `installation.namespace`                  | namespace for Colo installation                                  | `colo-system`                   |
| `installation.createNamespace`            | Whether to create the installation.namespace                     | `true`                          |
| `imageRepositoryHost`                     | Image repository host                                            | ` `                             |
| `manager.log.level`                       | Log level that colo-manager printed                             | `4`                             |
| `manager.replicas`                        | Replicas of colo-manager deployment                             | `2`                             |
| `manager.image.repository`                | Repository for colo-manager image                               | `colo/colo-manager`   |
| `manager.image.tag`                       | Tag for colo-manager image                                      | `v0.1.0`                        |
| `manager.resources.limits.cpu`            | CPU resource limit of colo-manager container                    | `1000m`                         |
| `manager.resources.limits.memory`         | Memory resource limit of colo-manager container                 | `1Gi`                           |
| `manager.resources.requests.cpu`          | CPU resource request of colo-manager container                  | `500m`                          |
| `manager.resources.requests.memory`       | Memory resource request of colo-manager container               | `256Mi`                         |
| `manager.nodeSelector`                    | Node labels for colo-manager pod                                | `{}`                            |
| `manager.tolerations`                     | Tolerations for colo-manager pod                                | `[]`                            |
| `scheduler.log.level`                     | Log level that colo-scheduler printed                           | `4`                             |
| `scheduler.replicas`                      | Replicas of colo-scheduler deployment                           | `2`                             |
| `scheduler.image.repository`              | Repository for colo-scheduler image                             | `colo/colo-scheduler` |
| `scheduler.image.tag`                     | Tag for colo-scheduler image                                    | `v0.1.0`                        |
| `scheduler.resources.limits.cpu`          | CPU resource limit of colo-scheduler container                  | `1000m`                         |
| `scheduler.resources.limits.memory`       | Memory resource limit of colo-scheduler container               | `1Gi`                           |
| `scheduler.resources.requests.cpu`        | CPU resource request of colo-scheduler container                | `500m`                          |
| `scheduler.resources.requests.memory`     | Memory resource request of colo-scheduler container             | `256Mi`                         |
| `scheduler.port`                          | Port of metrics served                                           | `10251`                         |
| `scheduler.nodeSelector`                  | Node labels for colo-scheduler pod                              | `{}`                            |
| `scheduler.tolerations`                   | Tolerations for colo-scheduler pod                              | `[]`                            |
| `scheduler.hostNetwork`                   | Whether colo-scheduler pod should run with hostnetwork          | `false`                         |
| `cololet.log.level`                      | Log level that cololet printed                                  | `4`                             |
| `cololet.image.repository`               | Repository for cololet image                                    | `colo/cololet`        |
| `cololet.image.tag`                      | Tag for cololet image                                           | `v0.1.0`                        |
| `cololet.resources.limits.cpu`           | CPU resource limit of cololet container                         | `1`                          |
| `cololet.resources.limits.memory`        | Memory resource limit of cololet container                      | `1G`                         |
| `cololet.resources.requests.cpu`         | CPU resource request of cololet container                       | `1`                             |
| `cololet.resources.requests.memory`      | Memory resource request of cololet container                    | `1G`                             |
| `service.cololet.type`      | Service type of cololet                     | `ClusterIP`                             |
| `service.cololet.port`      | Service port of cololet                      | `9435`                             |
| `service.manager.port`      | Service port of manager                    | `8443`                             |
| `webhookConfiguration.failurePolicy.pods` | The failurePolicy for pods in mutating webhook configuration     | `Ignore`                        |
| `webhookConfiguration.timeoutSeconds`     | The timeoutSeconds for all webhook configuration                 | `30`                            |
| `crds.managed`                            | Colo will not install CRDs with chart if this is false    | `true`                          |
| `imagePullSecrets`                        | The list of image pull secrets for Colo image             | `false`                         |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install` or `helm upgrade`.

### Optional: feature-gate

Feature-gate controls some influential features in Colo:

| Name                      | Description                                                       | Default | Effect (if closed)                     |
| ------------------------- | ----------------------------------------------------------------  | ------- | -------------------------------------- |
| `PodMutatingWebhook`      | Whether to open a mutating webhook for Pod **create**             | `true`  | Don't inject colo.sh/qosClass, Colo.sh/priority and don't replace Colo extend resources ad so on |
| `PodValidatingWebhook`    | Whether to open a validating webhook for Pod **create/update**    | `true`  | It is possible to create some Pods that do not conform to the Colo specification, causing some unpredictable problems |


If you want to configure the feature-gate, just set the parameter when install or upgrade. Such as:

```bash
$ helm install colo https://... --set featureGates="PodMutatingWebhook=true\,PodValidatingWebhook=true"
```

If you want to enable all feature-gates, set the parameter as `featureGates=AllAlpha=true`.

### Optional: the local image for China

If you are in China and have problem to pull image from official DockerHub, you can use the registry hosted on Ksyun Cloud:

```bash
$ helm install Colo https://... --set imageRepositoryHost=hub.kce.ksyun.com
```