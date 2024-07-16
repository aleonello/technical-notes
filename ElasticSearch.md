# ElasticSearch

## Links

* [Configure a lifecycle policy](https://www.elastic.co/guide/en/elasticsearch/reference/current/set-up-lifecycle-policy.html)
* [Index lifecycle management](https://www.elastic.co/guide/en/observability/current/apm-ilm-how-to.html)

## Get Indices

```sh
GET _cat/indices?v
```

## Get Indices in json format by GB size

```sh
GET _cat/indices?format=json&bytes=gb
```

## Get nodes memory usage

```sh
GET _cat/nodes?v=true&h=name,node*,heap*
```

## Get nodes stats

```sh
GET _nodes/stats?human&filter_path=nodes.*.name,nodes.*.indices.mappings.total_estimated_overhead*,nodes.*.jvm.mem.heap_max*
```

## Fluentd Error: object mapping for [kubernetes.labels.app] tried to parse field [app] as object, but found a concrete value, object mapping for [kubernetes.labels.app] tried to parse field [app] as object, but found a concrete value

1. Step 1: (Create an ingest pipeline with a simple Rename processor):

```sh
PUT _ingest/pipeline/rename_pipeline
{
  "processors": [
    {
      "rename": {
        "field": "kubernetes.labels.app",
        "target_field": "kubernetes.labels.app.value",
        "ignore_missing": true,
        "if": "ctx?.kubernetes?.labels?.app instanceof String",
        "ignore_failure": true
      }
    }
  ]
}
```

https://www.elastic.co/guide/en/elasticsearch/reference/8.14/rename-processor.html

2. Step 2: (Attach the ingest pipeline to an index or index template):

```sh
PUT _index_template/index1_template
{
  "index_patterns": ["index1-*"],
  "template": {
    "settings": {
      "index.default_pipeline": "rename_pipeline"
    }
  }
}
```

Source: https://stackoverflow.com/questions/75568513/k8s-fluent-bit-elastic-tried-to-parse-field-as-object-but-found-a-concrete
