+++
title = "Compose"
# define chart data here
[data]
  fileLink = "content/projects.csv" # path to where csv is stored
  colors = ["#627c62", "#11819b", "#ef7f1a", "#4e1154"] # chart colors
  columnTitles = ["Section", "Status", "Author"] # optional if no table will be displayed from dataset
  baseChartOn = 3 # number of column the chart(s) and graph should be drawn from # can be overridden directly via shortcode parameter # it's therefore optional
  title = "Projects"
+++

{{< block "grid-2" >}}
{{< column >}}

{{< tip >}}
This project is in no way affiliated with [Velero](https://velero.io).
{{< /tip >}}

Velero Sentinel was born out of the necessity to get informed about failed and partially failed backups made with [Velero][velero].

There are more elaborate alternatives out there. If you have [Prometheus Operator][prometheus:operator] running, you can [achieve the same](https://github.com/vmware-tanzu/velero/issues/2725) with a simple rule:

```yaml
apiVersion: monitoring.coreos.com/v1
kind: PrometheusRule
metadata:
  name: velero
spec:
  groups:
  - name: velero-failures
    rules:
    - alert: VeleroBackupPartialFailures
      annotations:
        message: Velero backup {{ $labels.schedule }} has {{ $value | humanizePercentage }} partialy failed backups.
      expr: |-
        velero_backup_partial_failure_total{schedule!=""} / velero_backup_attempt_total{schedule!=""} > 0.25
      for: 15m
      labels:
        severity: warning
    - alert: VeleroBackupFailures
      annotations:
        message: Velero backup {{ $labels.schedule }} has {{ $value | humanizePercentage }} failed backups.
      expr: |-
        velero_backup_failure_total{schedule!=""} / velero_backup_attempt_total{schedule!=""} > 0.25
      for: 15m
      labels:
        severity: warning
```

However, this requires more moving parts. The goal of Velero Sentinel is to provide the most simple solution possible.

{{< tip "warning" >}}
Velero Sentinel is still work in progress. Although it only needs read access to your cluster, proceed with caution!
{{< /tip >}}

{{< button "./docs/install/" "Install Velero Sentinel" >}}

[velero]: https://velero.io "Velero project page"
[prometheus:operator]: https://github.com/prometheus-operator/prometheus-operator "Github page of Prometheus Operator"
{{< /column >}}

{{< column >}}

![diy](/images/pexels-alex-ca-1034040.jpg)
{{< /column >}}
{{< /block >}}
