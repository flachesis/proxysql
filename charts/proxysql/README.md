# ProxySQL

[ProxySQL](/https://www.proxysql.com/) is a High-performance MySQL proxy with a GPL license.

## TL;DR;

```bash
$ helm repo add proxysql https://flachesis.github.io/proxysql
$ helm install my-release proxysql/proxysql
```

## Introduction

This chart bootstraps a [ProxySQL](https://hub.docker.com/r/proxysql/proxysql) proxy deployment on a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites

- Kubernetes 1.19+
- Helm 3.0-beta3+

## Installing the Chart

To install the chart with the release name `my-release`:

```bash
$ helm install my-release proxysql/proxysql
```

The command deploys ProxySQL on the Kubernetes cluster in the default configuration. The [Parameters](#parameters) section lists the parameters that can be configured during installation.

> **Tip**: List all releases using `helm list`

## Uninstalling the Chart

To uninstall/delete the `my-release` deployment:

```bash
$ helm delete my-release
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Parameters

The following table lists the configurable parameters of the ProxySQL chart and their default values.

| Parameter                                   | Description                                          | Default                                                          |
|---------------------------------------------|------------------------------------------------------|------------------------------------------------------------------|
| `replicaCount`                              | Replica count                                        | `1`                                                              |
| `image.repository`                          | ProxySQL Image name                                  | `proxysql/proxysql`                                              |
| `image.tag`                                 | ProxySQL Image tag                                   | `2.0.9`                                                          |
| `image.pullPolicy`                          | ProxySQL image pull policy                           | `IfNotPresent`                                                   |
| `imagePullSecrets`                          | Specify docker-registry secret names as an array     | `[]` (does not add image pull secrets to deployed pods)          |
| `service.type`                              | Kubernetes service type                              | `ClusterIP`                                                      |
| `service.port`                              | ProxySQL service port                                | default as `proxysql.mysql.port`                                 |
| `service.annotations`                       | Service annotations                                  | `{}`                                                             |
| `podAnnotations`                            | Pod annotations                                      | `{}`                                                             |
| `serviceAccount.create`                     | Specifies whether a ServiceAccount should be created | `true`                                                           |
| `serviceAccount.name`                       | The name of the ServiceAccount to create             | Generated using the proxysql.fullname template                   |
| `nameOverride`                              | String to partially override proxysql.fullname template with a string (will prepend the release name)                                               | `nil` |
| `fullnameOverride`                          | String to fully override proxysql.fullname template with a string                                                                                   | `nil` |
| `securityContext`                           | [a container security context](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/#set-the-security-context-for-a-container) | `{}`  |
| `podSecurityContext`                        | [pod security](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)                                                          | `{}`  |
| `proxysql.configmap`                        | ProxySQL confgmaps name                                  | `nil`                                                        |
| `proxysql.admin.user`                       | ProxySQL admin user for the management (127.0.0.1:6032)  | `admin`                                                      |
| `proxysql.admin.password`                   | ProxySQL admin password                                  | `admin`                                                      |
| `proxysql.port`                             | ProxySQL management port                                 | `6032`                                                       |
| `proxysql.monitor.enabled`                  | Enable Monitor for Backend                               | `false`                                                      |
| `proxysql.monitor.user`                     | Monitor user name                                        | `nil`                                                        |
| `proxysql.monitor.password`                 | Monitor user password                                    | `nil`                                                        |
| `proxysql.monitor.writerAsReader`           | Add writer to reader                                     | `true`                                                       |
| `proxysql.cluster.enabled`                  | Enable ProxySQL cluster                                  | `false`                                                      |
| `proxysql.cluster.user`                     | ProxySQL cluster user                                    | `cluster`                                                    |
| `proxysql.cluster.password`                 | ProxySQL cluster password                                | `cluster`                                                    |
| `proxysql.cluster.claim.enabled`            | Enable ProxySQL claim                                    | `true`                                                       |
| `proxysql.cluster.claim.accessModes`        | ProxySQL claim accessModes                               | `[ "ReadWriteOnce" ]`                                        |
| `proxysql.cluster.claim.storageClassName`   | Storage class name                                       | `default`                                                    |
| `proxysql.cluster.claim.size`               | Storage size                                             | `1Gi`                                                        |
| `proxysql.web.user`                         | ProxySQL web user for the state                          | `sadmin`                                                     |
| `proxysql.web.password`                     | ProxySQL web password                                    | `sadmin`                                                     |
| `proxysql.mysql.version`                    | MySQL version                                            | `5.7.34`                                                     |
| `proxysql.mysql.port`                       | MySQL port                                               | `3306`                                                       |
| `proxysql.mysql.maxConnections`             | The max num of client conn that the proxy can handle     | `2048`                                                       |
| `proxysql.mysql.queyCacheSizeMB`            | Query cache size in MB                                   | `256`                                                        |
| `proxysql.mysql.waitTimeout`                | mysql backend session timeout                            | `28800000`                                                   |
| `proxysql.mysql.queryRetriesOnFailure`      | query retries on failure                                 | `2`                                                          |
| `proxysql.mysql.readWriteSplit`             | MySQL Read/Write Splitting                               | `true`                                                       |
| `proxysql.mysql.servers.[].isWriter`        | MySQL backend is writable                                | `true`                                                       |
| `proxysql.mysql.servers.[].hostname`        | MySQL backend's hostname                                 | `nil`                                                        |
| `proxysql.mysql.servers.[].port`            | MySQL backend's port                                     | `3306`                                                       |
| `proxysql.mysql.servers.[].maxConnections`  | The max num of conn that the backend can handle          | `1000`                                                       |
| `proxysql.mysql.servers.[].compression`     | Compress data                                            | `false`                                                      |
| `proxysql.mysql.servers.[].weight`          | Server weight                                            | `1000`                                                       |
| `proxysql.mysql.users.[].username`          | The user for connecting backend                          | `nil`                                                        |
| `proxysql.mysql.users.[].password`          | The user's password                                      | `nil`                                                        |
| `proxysql.mysql.users.[].readOnly`          | Does the user is read only                               | `false`                                                      |
| `proxysql.mysql.users.[].maxConnections`    | The max connections that the proxysql can handle         | `10000`                                                      |
| `proxysql.mysql.slave.enabled`              | Enable the traditional master/slave replication          | `false`                                                      |
| `proxysql.mysql.slave.checkType`            | check `read_only`, use `innodb_read_only` for AWS Aurora | `read_only`                                                  |
| `proxysql.mysql.galera.enabled`             | Enable the Percona XtraDB Cluster                        | `false`                                                      |
| `proxysql.mysql.galera.maxWriters`          | The max writers that the proxySQL can use.               | `1`                                                          |
| `proxysql.mysql.galera.writerAsReader`      | Use the writer as reader                                 | `true`                                                       |
| `autoscaling.enabled`                           | Enable Autoscaling for pods (only supported when proxysql.cluster.enabled is false) | `false`                       |
| `autoscaling.minReplicas`                       |                                                                                     | `1`                           |
| `autoscaling.maxReplicas`                       |                                                                                     | `100`                         |
| `autoscaling.targetCPUUtilizationPercentage`    |                                                                                     | `80`                          |
| `autoscaling.targetMemoryUtilizationPercentage` |                                                                                     | `nil`                         |
| `resources`                                  | [Compute Resource Quota](https://kubernetes.io/docs/concepts/policy/resource-quotas/#compute-resource-quota)                  | `{}` |
| `nodeSelector`                               | [Pod node selector](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#nodeselector)                    | `{}` |
| `tolerations`                                | [Tolerations](https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/)                                  | `[]` |
| `affinity`                                   | [Compute Resource Quota](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#affinity-and-anti-affinity) | `{}` |

For more information please refer to the proxysql [config file](https://github.com/sysown/proxysql#configuring-proxysql-through-the-config-file) and [global variables](https://github.com/sysown/proxysql/wiki/Global-variables).

> **Tip**: You can use the default [values.yaml](values.yaml)

## Configuration and installation details

ProxySQL persists its configuration in SQLite, however this deployment is stateless i.e. no data is persisted. Since the configuration is managed via kubernetes and admin ProxySQL CLI is not meant for the configuration purposes all you need is to provide a `values.yaml` input file:

```bash
$ helm install my-release proxysql/proxysql -f values.yaml
```

### ProxySQL and MySQL 8.0

ProxySQL supports MySQL 8.0 , although there are some limitations for the [details refer to the documentation](https://github.com/sysown/proxysql/wiki/MySQL-8.0).
