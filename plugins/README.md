/ [mohawk](/) / [plugins](/plugins)


## Storage Plugins

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

##### Chart: different Plugins vs. Run Time

![Time chart](https://github.com/MohawkTSDB/MohawkTSDB.github.io/raw/master/benchmark/time.png?raw=true "benchmark time vm")
