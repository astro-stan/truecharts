---
title: Grafana Dashboards
---

There are multiple options how to add dashboard to Grafana.
From chart version 21, this is extended with the option Dahsboards can be automaticly loaded from Grafana Marketplace or other URL. Scraping from configmaps will still be possible as well.

## Adding a dashboard via marketplace

Add the following part to your `.Values`:

```yaml
dashboards:
  volsync:
    enabled: true
    failOnError: false
    b64content: false
    datasource:
      - name: "${DS_PROMETHEUS}"
        value: Prometheus
      - name: "${VAR_REPLICATIONDESTNAME}"
        value: ".*-dst|.*-bootstrap"
    marketplace:
      id: 21356
      revision: 3
```

## Adding a dashboard via url

Add the following part to your `.Values`:

```yaml
dashboard:
  truenas:
    enabled: true
    failOnError: false
    b64content: false
    datasource:
      - name: "${DS_MIMIR}"
        value: Prometheus
    url: https://raw.githubusercontent.com/Supporterino/truenas-graphite-to-prometheus/refs/heads/main/dashboards/truenas_scale.json
```


### Some explanation of the values:
- `enabled` turn on/off the dashboard
- `failOnError` when enabled and dashboard fails to download, `init pod` will go into error and Grafana will not continue deploying.
- `b64content` automaticly decodes base64 encoded dashboards
- `datasource: { name: "", value: "" }` A list of maps where entry allows specifying a variable which needs to be replaced with a give value. You should lookup in the dashboard's JSON file which variables need to be set. Variable substitutions need to be as they appear in the JSON, including brackets (`${...}`), if present. Keep in mind that in deployments like flux you may need to escape some characters.
- `id` numbered id from https://grafana.com/grafana/dashboards/.
- `revision` numbered revision from https://grafana.com/grafana/dashboards/.
- `url` url where to download the dashboard from.

Note: It have to be `marketplace:` or `url:` both cannot be set at a time per dashboard.
