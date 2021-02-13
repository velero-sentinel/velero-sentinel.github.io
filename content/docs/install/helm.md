+++
title = "Using Helm"
weight = 2
+++

The installation using [helm][helm] is the most convenient way to get the Sentinel running.

## Add helm repository

```shell
helm add repo
```

## Configure Sentinel

| Key                   | Descriptption                                                  | Default Value |
| --------------------- | -------------------------------------------------------------- | ------------- |
| `auth.basic.user`     | Basic auth username for acessing prometheus                    | `sentinel`    |
| `auth.basic.password` | Basic auth password for acessing prometheus                    | &ndash;       |

### Example configuration

```yaml
auth:
  basic:
    user: sentinel
    password:
notifications:
  webhooks:
    - name: foo
      url: bar
  amqp:
    - name: baz
      host: hostname
      port: 12345
      username: bar
      password: 
```

## Deploy Sentinel

```shell
helm install -n velero \
--set auth.basic.password=$HTTP_BASIC_AUTH_PASSWORD \
--set notifications.amqp.[0].password=$SECRET_PASSWORD \
velero-sentinel/sentinel
```

[helm]: https://helm.sh "Helm Project page"
