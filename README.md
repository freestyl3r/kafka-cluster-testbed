# Kafka Cluster Testbed

A Kafka cluster testbed for testing & development.

Services
--------

- Zookeeper (3 nodes)
- Kafka (2 nodes)

Dependencies
------------

- `docker.io`
- `docker-compose`
- `libnss-docker` (optional)
- `kafkacat` (optional, for testing)

Usage
-----


```shell
# Prepare cluster
$ docker-compose build
$ docker-compose up (Ctrl-C to stop)

# Create a new topic
$ ./k.sh topics --topic top1 --partitions 2 --replication-factor 2 --create
$ ./k.sh topics --describe
Topic:top1	PartitionCount:2	ReplicationFactor:2	Configs:
	Topic: top1	Partition: 0	Leader: 1	Replicas: 1,2	Isr: 1,2
	Topic: top1	Partition: 1	Leader: 2	Replicas: 2,1	Isr: 2,1

# Pub/Sub
$ echo "Hello Kafka" | kafkacat -P -b kc1.docker -t top1
$ kafkacat -C -b kc1.docker -t top1 -c 1
Hello Kafka

# Rafka
$ redis-cli -h rafka.docker -p 6380 rpushx topics:top1 "hello there"

# Remove everything
$ docker-compose down
```

# Additional services

Additional kafka-related services are available

* [Rafka](https://github.com/skroutz/rafka)
* [Schema Registry](https://github.com/confluentinc/schema-registry)
* [Kafka REST](https://github.com/confluentinc/kafka-rest)
* [KSQL](https://github.com/confluentinc/ksql)
* [Confluent Control Center](https://docs.confluent.io/current/control-center/docs/index.html)

These services are not started by default and defined in `docker-compose.additional.yml`, according to the
[Multiple Compose files pattern](https://docs.docker.com/compose/extends/#multiple-compose-files)
and can be started independently through

```sh
docker-compose -f docker-compose.additional.yml up
```

All services, both kafka and all the additional ones, can be started through

```sh
docker-compose -f docker-compose.yml -f docker-compose.additional.yml up
```
