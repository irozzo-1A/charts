# Admission Controller

This chart deploys the Sysdig Admission Controller in your Kubernetes cluster.

## Installing the Chart

Add Sysdig Helm charts repository:

```
$ helm repo add sysdig https://charts.sysdig.com
```

Deploy the scanner adapter

```
$ helm install --create-namespace -n admission-controller admission-controller -f values.yaml sysdig/admission-controller
```

## Configuration

The following table lists the configurable parameters of the Sysdig Admission
Controller chart and their default values:

| Parameter                             | Description                                                  | Default                                                                                                                             |
| ---                                   | ---                                                          | ---                                                                                                                                 |
| `sysdig.url`                          | The Sysdig URL prefix                                        | `https://secure.sysdig.com`                                                                                                         |
| `sysdig.secureAPIToken`               | API Token to access Sysdig Secure                            | ``                                                                                                                                  |
| `clusterName`                         | Cluster Name which appear on Secure UI                       | ``                                                                                                                                  |
| `features.k8sAuditDetections`         | Enable K8s Audit detections with Falco rules                 | `false`                                                                                                                             |
| `verifySSL`                           | Verify SSL on HTTPS connections to Sysdig Secure             | `true`                                                                                                                              |
| `nameOverride`                        | Chart name override                                          | ` `                                                                                                                                 |
| `fullnameOverride`                    | Chart full name override                                     | ` `                                                                                                                                 |
| `serviceAccounts.webhook.create`      | Create the service account                                   | `true`                                                                                                                              |
| `serviceAccounts.webhook.annotations` | Extra annotations for serviceAccount                         | `{}`                                                                                                                                |
| `serviceAccounts.webhook.name`        | Use this value as serviceAccount Name                        | ` `                                                                                                                                 |
| `serviceAccounts.scanner.create`      | Create the service account                                   | `true`                                                                                                                              |
| `serviceAccounts.scanner.annotations` | Extra annotations for serviceAccount                         | `{}`                                                                                                                                |
| `serviceAccounts.scanner.name`        | Use this value as serviceAccount Name                        | ` `                                                                                                                                 |
| `webhook.name`                        | Service name for Webhook deployment                          | `webhook`                                                                                                                           |
| `webhook.replicaCount`                | Amount of replicas for webhook                               | `1`                                                                                                                                 |
| `webhook.image.repository`            | Webhook image repository                                     | `quay.io/sysdig/admission-controller`                                                                                               |
| `webhook.image.pullPolicy`            | PullPolicy for Webhook image                                 | `Always`                                                                                                                            |
| `webhook.image.tag`                   | Webhook image tag                                            | `3.4.0`                                                                                                                             |
| `webhook.service.type`                | Use this type as webhook service                             | `ClusterIP`                                                                                                                         |
| `webhook.service.port`                | Configure port for the webhook service                       | `5000`                                                                                                                              |
| `webhook.httpProxy`                   | HTTP Proxy settings for webhook                              | ``                                                                                                                                  |
| `webhook.httpsProxy`                  | HTTPS Proxy settings for webhook                             | ``                                                                                                                                  |
| `webhook.noProxy`                     | No proxy these URL's for webhook                             | `kubernetes,10.0.0.0/8`                                                                                                             |
| `webhook.podAnnotations`              | Webhook pod annotations                                      | `{"prometheus.io/scrape": "true", "prometheus.io/path": "/metrics", "prometheus.io/port": "5000", "prometheus.io/scheme": "https"}` |
| `webhook.podSecurityContext`          | PSP's for webhook                                            | `{"fsgroup": 1000}`                                                                                                                 |
| `webhook.securityContext`             | Configure securityContext for webhook                        | `{"capabilities": {"drop": ["ALL"]}, "readOnlyRootFilesystem": true, "runAsNonRoot": true, "runAsUser": 1000 }`                     |
| `webhook.imagePullSecrets`            | The image pull secrets for webhook                           | `[]`                                                                                                                                |
| `webhook.resources`                   | Resource request and limits for webhook                      | `{}`                                                                                                                                |
| `webhook.nodeSelector`                | Configure nodeSelector for scheduling for webhook            | `{}`                                                                                                                                |
| `webhook.nodeSelector`                | Configure nodeSelector for scheduling for webhook            | `{}`                                                                                                                                |
| `webhook.tolerations`                 | Tolerations for scheduling for webhook                       | `[]`                                                                                                                                |
| `webhook.affinity`                    | Configure affinity rules for webhook                         | `{}`                                                                                                                                |
| `webhook.denyOnError`                 | Deny request when an error happened evaluating request       | `false`                                                                                                                             |
| `webhook.timeoutSeconds`              | Number of seconds for the request to time out                | `10`                                                                                                                                |
| `scanner.enabled`                     | Deploy the Inline Scanner Service                            | `true`                                                                                                                              |
| `scanner.name`                        | Service name for Scanner deployment                          | `scanner`                                                                                                                           |
| `scanner.replicaCount`                | Amount of replicas for scanner                               | `1`                                                                                                                                 |
| `scanner.image.repository`            | Scanner image repository                                     | `quay.io/sysdig/inline-scan-service`                                                                                                |
| `scanner.image.pullPolicy`            | PullPolicy for Scanner image                                 | `Always`                                                                                                                            |
| `scanner.image.tag`                   | Scanner image tag                                            | `0.0.9`                                                                                                                             |
| `scanner.service.port`                | Configure port for the webhook service                       | `8443`                                                                                                                              |
| `scanner.authWithSecureToken`         | Authenticate with Secure token                               | `false`                                                                                                                             |
| `scanner.httpProxy`                   | HTTP Proxy settings for scanner                              | ``                                                                                                                                  |
| `scanner.httpsProxy`                  | HTTPS Proxy settings for scanner                             | ``                                                                                                                                  |
| `scanner.noProxy`                     | No proxy these URL's for scanner                             | `kubernetes,10.0.0.0/8`                                                                                                             |
| `scanner.podAnnotations`              | Scanner pod annotations                                      | `{"prometheus.io/scrape": "true", "prometheus.io/path": "/metrics", "prometheus.io/port": "5000", "prometheus.io/scheme": "https"}` |
| `scanner.psp.create`                  | Whether to create a psp policy and role / role-binding       | `false`                                                                                                                             |
| `scanner.resources`                   | Resource requests and limits for scanner                     | `{}`                                                                                                                                |
| `scanner.verifyRegistryTLS`           | Verify TLS on image pull from registries                     | `true`                                                                                                                              |
| `scanner.dockerCfgSecretName`         | Use a provided secret containing a .dockercfg for registry authentication (i.e. Openshift internal registry)   | ` `                                                                               |


Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```bash
$ helm install my-release \
    --set sysdig.secureApiToken=YOUR-KEY-HERE \
    sysdig/admission-controller
```

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```bash
$ helm install my-release -f values.yaml sysdig/admission-controller
```

### On Prem deployment

Use the following command to deploy in an on-prem:

```
$ helm install  --create-namespace -n sysdig-admission-controller sysdig-admission-controller \
                --set sysdig.url=SECURE_URL \
                --set sysdig.secureAPIToken=SECURE_API_TOKEN \
                --set clusterName=CLUSTER_NAME \
                --set verifySSL=false \
                sysdig/admission-controller
```

Use `verifySLL=false` if you are using self signed certificates.
