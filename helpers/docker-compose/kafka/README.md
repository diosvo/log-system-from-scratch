## Overview

**Kafka** is all about events. Events are organized in topics and categorized into partitions. These streams of events can then be used to build Publish/Subscribe systems or a Message Queues, which processes messages in parallel

![architecture](./images/architecture.png)

## Installation

```ruby
cd helpers/docker-compose/kafka # skip this command if you're already in kafka folder
docker-compose up
```

## Actions

After direct to `kafka` folder, follow these actions to interact with the data

| Action                           | Command                                                                                                                                                     |
| -------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Access to the container          | `docker exec -it --user root kafka bash`                                                                                                                    |
| Install and list all the sockets | `yum install net-tools` <br/> `netstat -an`                                                                                                                 |
| Kafka Producer                   | `docker run -it --rm --network kafka_docker_example_net confluentinc/cp-kafka /bin/kafka-console-producer --bootstrap-server kafka:9092 --topic test_topic` |
| Kafka Consumer                   | `docker run -it --rm --network kafka_docker_example_net confluentinc/cp-kafka /bin/kafka-console-consumer --bootstrap-server kafka:9092 --topic test_topic` |

## Related Images

- Diagram

  ![private-network](./images/private-network.png)

- Open two terminals in parallel to see the producer and consumer

  ![producer-consumer](./images/kafka-producer-consumer.png)

## References

| Name                                                                                            |
| ----------------------------------------------------------------------------------------------- |
| [How to Connect to Apache Kafka running in Docker](https://www.youtube.com/watch?v=L--VuzFiYrM) |
