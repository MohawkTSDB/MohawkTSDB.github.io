
# Mohawk TSDB

Mohawk is a metric data storage engine, it's fun, fast, light and easy to use.

## Introduction

Mohawk is a metric data storage engine that uses a plugin architecture for data storage and a simple REST API as the primary interface.

Mohawk can use different storage plugins for different use cases. Different storage plugins may vary in speed, persistence and scale ability. Mohawk use a subset of Hawkular's REST API, inheriting Hawkular's ecosystem of clients and plugins.

Different use cases may have conflicting requirements for the metric engine, some use cases may require fast data transfer, while others may depend on long term, high availability data retention that inherently makes the system slower.

Mohowk exposes the same simple REST API for different storage options, consumer application can use the same REST API with a lean low footprint stroage and with a resource-intensive high availability storage. Mohowk makes hierarchical data storage using short, middle and long term data retention tiers easy to set up and consume.     

##### Installing and Running Mohawk:

[Installing and Running Mohawk](/install), from source, fedora/centos dnf repository or container:

```bash
sudo dnf copr enable yaacov/mohawk
sudo dnf install mohawk
```

```bash
mohawk --version
mohawk --help
mohawk --options help
```

```bash
# Run server on http://0.0.0.0:8080
mohawk
```

```bash
# post some data (timestamp is in ms)
curl http://127.0.0.1:8080/hawkular/metrics/gauges/raw \
     -d "[{\"id\":\"machine/example.com/test\", \"data\":[{\"timestamp\": $(date +%s)000, \"value\": 42}]}]"
     
# read multiple data points using an ids list, time can be in ms or relative [with s, mn, h or d postfix]
curl http://127.0.0.1:8080/hawkular/metrics/gauges/raw/query \
     -d "{\"ids\": [\"machine/example.com/test\"], \"start\": \"-5mn\"}"
```

## Compatibility

[Mohawk Compatibility](/compatibility) with Grafana/Kubernetes/OpenShift echosystems.

Mohawk is tested(1) with [Hawkular](http://www.hawkular.org/) plugins, like [Hawkular Grafana Plugin](https://grafana.com/plugins/hawkular-datasource) and clients like [Python](https://github.com/hawkular/hawkular-client-python) and [Ruby](https://github.com/hawkular/hawkular-client-ruby). Mohawk also work with [Heapster](https://github.com/kubernetes/heapster) to automagically scrape metrics from Kubernetes/OpenShift clusters.

(1) Mohawk implement only part of Hawkular's API, some functionality may be missing.

## Storage Plugins

Mohawk [Storage Plugins](/plugins).

Mohawk architecture makes it easy to implement and set up storage plugins for new data storage. The storage directory include documentation, examples and a template for plugin development.

##### Current storage plugin list include:

| Plugin name       |  Storage          | Advantages                                  | Use case                                 |
|-------------------|-------------------|---------------------------------------------|------------------------------------------|
| memory            | Memory            | No storage ware and tear from fast I/O      | Fast I/O, no need for persistence data   |
| sqlite            | Local File        | No data loss on network outages             | Persistence data, W/O external data base |
| mongo             | Mongo DB          | High availabilty, High volume storage       | Long term H.A. storage                   |


## Benchmarks

[Benchmark](/benchmark) (1) results depend on system resources, current work load and network.

##### Mohawk with different Plugins running on a desktop machine.

| Plugin   | Time       | %CPU      | RSS byte      |
|----------|------------|-----------|---------------|
|memory    |  0m2.011s  | 0.2 - 5.5 | 7456 - 11028  |
|mongo (2) |  0m4.885s  | 0.5 - 0.8 | 11892 - 11892 |
|sqlite3   |  0m14.471s | 0.2 - 7.4 | 8416 - 12560  |

(1) Description: 1000 writes + 1000 reads ( [benchmark.py](https://github.com/MohawkTSDB/MohawkTSDB.github.io/blob/master/benchmark/benchmark.py) ) less is better.

(2) the mongo usage metrics does not include usage of the mongodb server.
