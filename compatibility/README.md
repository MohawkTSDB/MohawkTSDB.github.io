/ [mohawk](/) / [compatibility](/compatibility)

## Compatibility

Mohawk is tested(1) with [Hawkular](http://www.hawkular.org/) plugins, like [Hawkular Grafana Plugin](https://grafana.com/plugins/hawkular-datasource) and clients like [Python](https://github.com/hawkular/hawkular-client-python) and [Ruby](https://github.com/hawkular/hawkular-client-ruby). Mohawk also work with [Heapster](https://github.com/kubernetes/heapster) to automagically scrape metrics from Kubernetes/OpenShift clusters.

(1) Mohawk implement only part of Hawkular's API, some functionality may be missing.

### Hawkular / Grafana

##### Setting up Hawkular Grafana Plugin:

![Mohawk](/images/mohawk-grafana.gif?raw=true "Mohawk help")

##### More data in chart:

![Mohawk](/images/mohawk-grafana-plugin.gif?raw=true "Mohawk help")

Using [Hawkular Grafana Plugin](https://grafana.com/plugins/hawkular-datasource) and [push_metrics_example.sh](https://github.com/MohawkTSDB/mohawk/blob/master/examples/push_metrics_example.sh) to push random data into Mohawk.

### Prometheus

##### Set as a Prometheus scraping point:

Example `prometheus.yml` file:
```yml
scrape_configs:
- job_name: 'mohawk'

  scrape_interval: 5s
  scrape_timeout: 5s

  metrics_path: '/hawkular/metrics/exports'
  scheme: 'http'
  tls_config:
    insecure_skip_verify: true

  static_configs:
  - targets: ['10.35.3.121:8080']
```

![Mohawk](/images/mohawk-prometheus.gif?raw=true "Mohawk help")

```
# run Prometheus
docker run -p 9090:9090 -v /home/yzamir/prometheus.yml:/etc/prometheus/prometheus.yml:Z prom/prometheus
```

```
# run mohawk and push some random data

# push the labels once
curl http://127.0.0.1:8080/hawkular/metrics/gauges/tags -d "[{ \
    \"id\":\"machine/example.com/test\", \
    \"tags\":{\"__name__\":\"test\",\"hostname\":\"example.com\"}}]" -X PUT

# push data every 1s
while true; do \
  curl http://127.0.0.1:8080/hawkular/metrics/gauges/raw -d "[{ \
       \"id\":\"machine/example.com/test\", \
       \"data\":[{\"timestamp\":$(date +%s)000,\"value\":$(echo "$(date +%s)%10"|bc)}]}]"; \
  sleep 1; done
```

![Mohawk](/images/mohawk-prometheus-g.gif?raw=true "Mohawk help")
