# Docker Compose + Kafka + Spring Boot
![background](./assets/background.jpg)

## Prerequisites

- Maven 3+
- Java 8+
- Docker 19.03+ 


## Preparing Environment

From the project's folder, execute:
- `docker-compose up` to initialize Kafka and Zookeeper
- `mvn clean package` to build the applications


## Booting Applications

- Initializing `producer`
````bash
$ cd producer
$ mvn spring-boot:run
````
**Note:** The Producer will be available at `http://localhost:8080/orders` accepting request.


- Initializing `consumer`
````bash
$ cd consumer
$ mvn spring-boot:run
````
**Note:** The Consumer has no endpoint, it just connects to the Kafka to listen to the stream events.


## Testing 

With both `consumer` and `producer` applications up and running, we can test their integration through Kafka:
````bash
$ curl -d "{'idorder': '1234', 'customer': 'Foo Bar', 'value': 4321}" \
-H "Content-Type: application/json" \
-X POST http://127.0.0.1:8080/orders
````

If the above request works, we should see a log in the `producer` application's console that looks like the following:
````
2021-07-07 06:23:26.433  INFO 2336 --- [ad | producer-1] b.c.d.producer.config.KafkaConfig        : ACK from ProducerListener message: {"idorder": "12345", "customer": "Foo Bar", "value": 54321} offset:  0
````
and something like in the `consumer` application's console:
````
2021-07-07 06:23:26.490  INFO 3996 --- [ntainer#0-0-C-1] b.c.d.consumer.kafka.KafkaConsumer       : Order: {"idorder": "12345", "customer": "Foo Bar", "value": 54321}
````