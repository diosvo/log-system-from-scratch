## About this part

**Druid** is a distributed real-time analytics database that handles time series data and works well with **Apache Kafka** as an event streaming platform. The Grafana Druid plugin provides a Druid data source for Grafana so you can visualize Druid data.

You will need the following Docker images:

- Druid
- Apache Kafka
- Zookeeper
- Grafana
- Prometheus
- Fluentd

Your Docker Compose file will run containers for:

- Druid coordinator and overlord
- Kafka broker
- Zookeeper
- Grafana
- Prometheus
- Fluentd

**Fluentd** will collect your logs and stream them to Kafka topics. Druid will read from the Kafka topics and index the data. **Prometheus** will expose metrics. And **Grafana** will provide the dashboards and visualization of your metrics and logs.

## How to start?

- start Docker on local machine
- execute and enter your password

  ```ruby
  cd log-management & docker-compose up
  ```

- open browser and access one of these URLs:

  | Host                                          | Note                                                                                       |
  | --------------------------------------------- | ------------------------------------------------------------------------------------------ |
  | [Druid Console](http://localhost:8888/)       |                                                                                            |
  | [Prometheus](http://localhost:9090/)          | [Prometheus server URL](http://prometheus:9090)                                            |
  | [Grafana](http://localhost:3000/dashboards)   | find the credentials needed in `~/.helpers/docker-compose/grafana/config/credentials` file |
  | [cAdvisor](http://localhost:8080/containers/) | [Metrics](http://localhost:8080/metrics)                                                   |

- to access a specific container, execute:

  ```ruby
  docker exec -it --user root <container_name> bash
  ```
