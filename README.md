## OVERVIEW

**Druid** is a distributed real-time analytics database that handles time series data and works well with **Apache Kafka** as an event streaming platform. The Grafana Druid plugin provides a Druid data source for Grafana so you can visualize Druid data.

**Fluentd** will collect your logs and stream them to Kafka topics. Druid will read from the Kafka topics and index the data. **Prometheus** will expose metrics. And **Grafana** will provide the dashboards and visualization of your metrics and logs.

## HOW TO START?

- start Docker on local machine
- execute and enter your password

  ```ruby
  cd helpers/docker-compose/<specific_service> & docker-compose up
  ```

- open browser and access one of these URLs:

  | Host                                           | Note                                                                                       |
  | ---------------------------------------------- | ------------------------------------------------------------------------------------------ |
  | [Druid Router Console](http://localhost:8888/) |                                                                                            |
  | [Prometheus](http://localhost:9090/)           | [Prometheus server URL](http://prometheus:9090)                                            |
  | [Grafana](http://localhost:3000/dashboards)    | find the credentials needed in `~/.helpers/docker-compose/grafana/config/credentials` file |
  | [cAdvisor](http://localhost:8080/containers/)  | [Metrics](http://localhost:8080/metrics)                                                   |

- to access a specific container, execute:

  ```ruby
  docker exec -it --user root <container_name> bash
  ```

## IMPROVEMENT

- [Import docker compose file in another compose file](https://stackoverflow.com/questions/55650342/import-docker-compose-file-in-another-compose-file)

## REFERENCES

| Name                                                                                                                                                         |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Setup                                                                                                                                                        |
| [Christian's Boilerplates](https://github.com/xcad2k/boilerplates)                                                                                           |
| [Giám sát ứng dụng Docker với cAdvisor, Prometheus và Grafana](https://viblo.asia/p/giam-sat-ung-dung-docker-voi-cadvisor-prometheus-va-grafana-RQqKLevMZ7z) |
| Tutorials                                                                                                                                                    |
| [Server Monitoring // Prometheus and Grafana Tutorial](https://www.youtube.com/watch?v=9TJx7QTrTyo)                                                          |
| UI                                                                                                                                                           |
| [Build a Markdown blog w/ React](https://www.youtube.com/watch?v=Od-Uj5RSsuM)                                                                                |
