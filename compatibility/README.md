/ [mohawk](/) / [compatibility](/compatibility)

##### Running Mohawk:
```

## Compatibility

Mohawk is tested(2) with [Hawkular](http://www.hawkular.org/) plugins, like [Hawkular Grafana Plugin](https://grafana.com/plugins/hawkular-datasource) and clients like [Python](https://github.com/hawkular/hawkular-client-python) and [Ruby](https://github.com/hawkular/hawkular-client-ruby). Mohawk also work with [Heapster](https://github.com/kubernetes/heapster) to automagically scrape metrics from Kubernetes/OpenShift clusters.

(2) Mohawk implement only part of Hawkular's API, some functionality may be missing.

##### Setting up Hawkular Grafana Plugin:

![Mohawk](/images/mohawk-grafana.gif?raw=true "Mohawk help")

##### More data in chart:

![Mohawk](/images/mohawk-grafana-plugin.gif?raw=true "Mohawk help")

Using [Hawkular Grafana Plugin](https://grafana.com/plugins/hawkular-datasource) and [push_metrics_example.sh](https://github.com/MohawkTSDB/mohawk/blob/master/examples/push_metrics_example.sh) to push random data into Mohawk.
