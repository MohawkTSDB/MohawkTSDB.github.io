
# Mohawk TSDB

![Mohawk](https://github.com/MohawkTSDB/MohawkTSDB.github.io/raw/master/images/logo-128.png?raw=true "Mohawk Logo")

Mohawk is a metric data storage engine, it's fun, fast, light and easy to use.

## Introduction

Mohawk is a metric data storage engine that uses a plugin architecture for data storage and a simple REST API as the primary interface.

Mohawk can use different storage plugins for different use cases. Different storage plugins may vary in speed, persistence and scale ability. Mohawk use a subset of Hawkular's REST API, inheriting Hawkular's ecosystem of clients and plugins.

Different use cases may have conflicting requirements for the metric engine, some use cases may require fast data transfer, while others may depend on long term, high availability data retention that inherently makes the system slower.

Mohowk exposes the same simple REST API for different storage options, consumer application can use the same REST API with a lean low footprint stroage and with a resource-intensive high availability storage. Mohowk makes hierarchical data storage using short, middle and long term data retention tiers easy to set up and consume.     

```bash
# run the server
mohawk 
2017/12/07 19:26:00 Start server, listen on http://0.0.0.0:8080

# store some data
curl http://localhost:8080/hawkular/metrics/gauges/raw -d \
"[{\"id\":\"x\",\"data\":[{\"timestamp\":$(( $(date +%s) ))000,\"value\":42}]}]"

curl http://localhost:8080/hawkular/metrics/gauges/raw -d \
"[{\"id\":\"x\",\"data\":[{\"timestamp\":$(( $(date +%s) - 60 ))000,\"value\":42}]}]"

# read data
curl http://localhost:8080/hawkular/metrics/gauges/raw/query -d "{\"ids\":[\"x\"]}"
[{"id": "x", "data": [
    {"timestamp":1512667581000,"value":42},
    {"timestamp":1512667510000,"value":42}
]}]
```

## Compatibility

Mohawk is tested(2) with [Hawkular](http://www.hawkular.org/) plugins, like [Hawkular Grafana Plugin](https://grafana.com/plugins/hawkular-datasource) and clients like [Python](https://github.com/hawkular/hawkular-client-python) and [Ruby](https://github.com/hawkular/hawkular-client-ruby). Mohawk also work with [Heapster](https://github.com/kubernetes/heapster) to automagically scrape metrics from Kubernetes/OpenShift clusters.

(2) Mohawk implement only part of Hawkular's API, some functionality may be missing.

![Grafana plugin](https://github.com/yaacov/fosdem-2017/raw/master/images/grafana-screenshot2.png)

## Storage Plugins

Mohawk architecture makes it easy to implement and set up storage plugins for new data storage. The storage directory include documentation, examples and a template for plugin development.

##### Current storage plugin list include:

| Plugin name       |  Storage          | Advantages                                  | Use case                                 |
|-------------------|-------------------|---------------------------------------------|------------------------------------------|
| memory            | Memory            | No storage ware and tear from fast I/O      | Fast I/O, no need for persistence data   |
| sqlite            | Local File        | No data loss on network outages             | Persistence data, W/O external data base |
| mongo             | Mongo DB          | High availabilty, High volume storage       | Long term H.A. storage                   |


## Benchmarks

[Benchmark](/benchmark) results depend on system resources, current work load and network.

##### Mohawk with different Plugins running on a desktop machine.

| Plugin   | Time       | %CPU      | RSS byte      |
|----------|------------|-----------|---------------|
|memory    |  0m2.011s  | 0.2 - 5.5 | 7456 - 11028  |
|mongo (1) |  0m4.885s  | 0.5 - 0.8 | 11892 - 11892 |
|sqlite3   |  0m14.471s | 0.2 - 7.4 | 8416 - 12560  |

(1) the mongo usage metrics does not include usage of the mongodb server.

###### Description: 1000 writes + 1000 reads ( [benchmark.py](/benchmark/benchmark.py) ) less is better.

##### Chart: different Plugins vs. Run Time

![Time chart](https://github.com/MohawkTSDB/MohawkTSDB.github.io/raw/master/benchmark/time.png?raw=true "benchmark time vm")

##### Mohawk vs. Hawkular running on a vm under same load.

| DB/Plugin          | Time        |
|---------------------|-------------|
|Hawkular/Casandra    |  2m8.783s   |
|Mohawk/Memory        |  0m22.833s  |

##### Chart: DB/Plugin vs. Run Time

![Time chart](https://github.com/MohawkTSDB/MohawkTSDB.github.io/raw/master/benchmark/time-vm.png?raw=true "benchmark time vm")

### Performance

Moahawk cpu and memory usage is lower than Hawkular and comparable to Prometheus, for more details see [Performance](/benchmark/PERF.html) doc.

##### Chart: Mohawk vs. Prometheus CPU (Pod name is hawkular-metrics, but actually running mohawk)

![CPU chart](https://github.com/MohawkTSDB/MohawkTSDB.github.io/raw/master/benchmark/mohawk-cpu.png?raw=true "benchmark cpu vm")
![CPU chart](https://github.com/MohawkTSDB/MohawkTSDB.github.io/raw/master/benchmark/prometheus-cpu.png?raw=true "benchmark cpu vm")

##### Chart: Mohawk vs. Prometheus Memory (Pod name is hawkular-metrics, but actually running mohawk)

![CPU chart](https://github.com/MohawkTSDB/MohawkTSDB.github.io/raw/master/benchmark/mohawk-mem.png?raw=true "benchmark cpu vm")
![CPU chart](https://github.com/MohawkTSDB/MohawkTSDB.github.io/raw/master/benchmark/prometheus-mem.png?raw=true "benchmark cpu vm")

