## How to start?

- start Docker on local machine
- execute and enter your password

  ```ruby
  cd log-management & docker-compose up
  ```

- open browser and access one of these URLs:

| Host                                          | Note                                                                                            |
| --------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| [Druid Console](http://localhost:8888/)       |                                                                                                 |
| [Prometheus](http://localhost:9090/)          | [Prometheus server URL](http://prometheus:9090)                                                 |
| [Grafana](http://localhost:3000/dashboards)   | find the credentials needed in `~/.helpers/docker-compose/grafana/config/credentials.conf` file |
| [cAdvisor](http://localhost:8080/containers/) | [Metrics](http://localhost:8080/metrics)                                                        |
