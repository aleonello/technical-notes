# ElasticSearch

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