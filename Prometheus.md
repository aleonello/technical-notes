# Prometheus

## Prometheus Query to get container CPU usage


```sql
sum(rate(container_cpu_usage_seconds_total{image!=""}[5m])) by (pod)
```

## Prometheus Query to get container Memory usage

```sql
sum(container_memory_usage_bytes{image!=""}) by (pod)
```
