# Kafka , Zookeeper, Kafka Rest Proxy and Landoop UI for Kafka Topics

A kafka stack with topics ui embedded for viewing of topics


## Pre requisites 

* Docker CE/Enterprise
* Docker Compose

**NOTE** : ensure that the following ports are open, 80,3030,9091-9092,8081-8083,2181

## Running it

First clone this repo in you suitable folder 
```
  git clone https://github.com/apollo90/confluent-kafka-landoop-ui.git
```
Navigate to /confluent-kafka-landoop-ui/docker-compose.yml  and change PUBLIC_IP_ADDRESS to you actual address on the advertised listener environment variable

```
  KAFKA_ADVERTISED_LISTENERS: INTERNAL://kafka:9091,EXTERNAL://PUBLIC_IP_ADDRESS:9092
```

 Execute the docker compose command to bring the stack up

```
  docker-compose up --build -d
```

### Setup Kafka Rest clusters(optional)

Use multiple Kafka Rest clusters in `env.js` :
```
var clusters = [
    {
      NAME: "prod",
      KAFKA_REST: "http://kafka-rest-ip:8082",
      MAX_BYTES: "50000",
      RECORD_POLL_TIMEOUT: "5000",
      DEBUG_LOGS_ENABLED: true,
      LAZY_LOAD_TOPIC_META: false
    },
    {
      NAME: "dev",
      KAFKA_REST: "localhost",
      MAX_BYTES: "50000",
      COLOR: "#141414", // Optional
      RECORD_POLL_TIMEOUT: "5000",
      DEBUG_LOGS_ENABLED: true,
      LAZY_LOAD_TOPIC_META: false
    }
  ];
```

**Config**

* Use `MAX_BYTES` to set the default maximum amount of bytes to fetch from each topic.
* Use `RECORD_POLL_TIMEOUT` to set the timeout in ms.
* Use `COLOR` to set different header colors for each set up cluster.
* Set `DEBUG_LOGS_ENABLED` to true to enable the debug logs.
* Set `LAZY_LOAD_TOPIC_META` to true to lazy load topic meta information.


## Common Issues

If having `"CONNECTIVITY ERROR" problems` make sure the file `kafka-rest.properties` has CORS enabled.
To enable CORS add the following configuration to that file, and restart the backend Kafka-Rest

```
access.control.allow.methods=GET,POST,PUT,DELETE,OPTIONS
access.control.allow.origin=*
```

If using a recent version of the Kafka-Topics-UI and Kafka-REST, make sure that you have properly configured
Kafka-REST with the new consumer API. That requires setting up in the configuration of Kafka REST

```
bootstrap.servers=PLAINTEXT://ip-address-of-kafka-broker:9092
```

Make sure you restart Kafka REST after changing it's configuration files

## Maintainers

* Charles Mutale(apollo90)
