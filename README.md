# unms-helm

unms-helm is a helm chart to run Ubiquiti Network Management System (UNMS) in kubernetes.
The chart successfully runs on amd64 and on armv7/armhf (Raspberry Pi) architecture.

## Installation

```console
$ helm install --name unms ./unms-helm
```
You can also specify values in `values.yaml`.

## Configuration

The following table lists the configurable parameters of the unms-helm chart

| Parameter                      | Description                                                                                                          | Default                                           |
| -------------------------------| ---------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------- |
| `replicaCount`                 | The number of replicas to run |
| `image.repository`             | UNMS image name                                | None                        |
| `image.tag`                    | The version of the UNMS image to use      | None                                          |
| `imagePullSecrets`             | A list of image pull secrets (if needed)  | None                              |
| `fullnameOverride` | Override the full resource names | `{release-name}-unms` (or `unms` if release-name is unms) |
| `service.dashboard.type` | A valid Kubernetes service type| None                                    |
| `service.dashboard.http.enabled` | Set to `false` if you want to disable http |
| `service.dashboard.http.port` | Port used for http | 80 |
| `service.dashboard.https.enabled` | Set to `false` if you want to disable https |
| `service.dashboard.https.port` | Port used for https | 443 |
| `service.netflow.enabled` | Set to `false` if you want to disable netflow |
| `service.netflow.type` | A valid Kubernetes service type | `LoadBalancer` |
| `service.netflow.loadBalancerIP` | An available static IP you have reserved on your cloud platform |
| `service.netflow.port` | Port used for netflow | `2055` |
| `ingress.enabled` | Enable automatic creation of an ingress for the http/https |
| `ingress.hosts[0].host` | The ingress host | `unms.example.com` |
| `ingress.hosts[0].paths` | A valid path | `/` |
| `data.enabled` | Set to `true` to use a persistentVolume | `true` |
| `data.name` | Set the name of the persistentvolume | `unms` |
| `data.storageClassName` | Set the name of the storageClass used to provision a persistent volume |

If `data.enabled` is set to `false`, no persistentVolume is going to be created or mounted. Instead, all the data is going to be saved to `/config` within the container. If `data.enabled` is set to `true`, helm will create a persistentVolumeClaim. You can either create a matching persistentVolume before installing the helm chart, or you'll need a persistentVolume provisioner handling the claim. If a persistentVolume is used, the data persists `helm upgrade` and `helm delete`.
