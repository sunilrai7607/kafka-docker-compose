# kafka-docker-compose
kafka docker compose

Zookeeper configuration
```yaml
 zookeeper-server:
    image: wurstmeister/zookeeper
    container_name: zookeeper
    ports:
    - "2181:2181"
```

Kafka configuration
```yaml
kafka-server:
    image: wurstmeister/kafka
    ports:
    - "9092:9092"
    environment:
      KAFKA_ADVERTISED_HOST_NAME: kafka-local
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
    depends_on:
      - zookeeper-server
```

Run below command to up zookeeper and kafka borker
```commandline
docker-compose up --build
```
```commandline
% docker ps   
CONTAINER ID   IMAGE                    COMMAND                  CREATED          STATUS          PORTS                                                NAMES
af28d7a0652f   wurstmeister/kafka       "start-kafka.sh"         31 minutes ago   Up 31 minutes   0.0.0.0:9092->9092/tcp                               kafka-docker-compose_kafka-server_1
8fc304dff9a2   wurstmeister/zookeeper   "/bin/sh -c '/usr/sb…"   31 minutes ago   Up 31 minutes   22/tcp, 2888/tcp, 3888/tcp, 0.0.0.0:2181->2181/tcp   zookeeper

```
### Below steps to manually create topic, produce and consume.
1. Run below command to run docker exec for kafka
```yaml
docker exec -it <container-image-name> /bin/bash

docker exec -it kafka-docker-compose_kafka-server_1 /bin/bash (Enter)
```
2. List of available topics
```commandline
bash-4.4# kafka-topics.sh --list --bootstrap-server localhost:9092
songs
```
3. Create Topic
```commandline
bash-4.4# kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic resveration-topic
Created topic resveration-topic
```
4. Produce message
```commandline
docker exec -it kafka-docker-compose_kafka-server_1 /bin/bash
bash-4.4# kafka-console-producer.sh --bootstrap-server localhost:9092 --topic songs
>Hello kafka producer
```
5. Consumer 
```commandline
docker exec -it kafka-docker-compose_kafka-server_1 /bin/bash
bash-4.4# kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic songs --group groupB
> Hello kafka producer
```
