# Prometheus

## Links

* [Prometheus Official Documentation](https://prometheus.io/docs/introduction/overview/)
* [PromQL Query Language Reference](https://prometheus.io/docs/prometheus/latest/querying/basics/)
* [Alerting Rules](https://prometheus.io/docs/prometheus/latest/configuration/alerting_rules/)
* [Grafana Documentation](https://grafana.com/docs/grafana/latest/)
* [node_exporter GitHub](https://github.com/prometheus/node_exporter)
* [kube-state-metrics GitHub](https://github.com/kubernetes/kube-state-metrics)

## Prometheus Query to get container CPU usage

```promql
sum(rate(container_cpu_usage_seconds_total{image!=""}[5m])) by (pod)
```

## Prometheus Query to get container Memory usage

```promql
sum(container_memory_usage_bytes{image!=""}) by (pod)
```
