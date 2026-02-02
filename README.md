# kafka-demo

A simple Spring Boot project with Kafka consumer configuration using application.yaml.

## Project Structure

```
kafka-demo/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/example/kafkademo/
│   │   │       ├── KafkaDemoApplication.java
│   │   │       └── consumer/
│   │   │           └── KafkaConsumer.java
│   │   └── resources/
│   │       └── application.yaml
│   └── test/
│       └── java/
├── pom.xml
└── README.md
```

## Features

- Spring Boot 3.2.0
- Spring Kafka integration
- Kafka consumer with YAML configuration
- Simple message consumer implementation

## Prerequisites

- Java 17 or higher
- Apache Kafka running on localhost:9092 (or update the configuration)
- Maven 3.6 or higher

## Configuration

The Kafka consumer is configured in `src/main/resources/application.yaml`:

```yaml
spring:
  kafka:
    consumer:
      bootstrap-servers: localhost:9092
      group-id: kafka-demo-group
      auto-offset-reset: earliest
      key-deserializer: org.apache.kafka.common.serialization.StringDeserializer
      value-deserializer: org.apache.kafka.common.serialization.StringDeserializer

kafka:
  topic:
    name: demo-topic
```

### Configuration Properties

- **bootstrap-servers**: Kafka server address (default: localhost:9092)
- **group-id**: Consumer group identifier (default: kafka-demo-group)
- **auto-offset-reset**: Where to start consuming messages (earliest/latest)
- **key-deserializer**: Deserializer for message keys (StringDeserializer)
- **value-deserializer**: Deserializer for message values (StringDeserializer)
- **topic.name**: Topic name to consume from (default: demo-topic)

## Building the Project

```bash
# Compile the project
mvn clean compile

# Package the application
mvn clean package

# Skip tests during packaging
mvn package -DskipTests
```

## Running the Application

```bash
# Run with Maven
mvn spring-boot:run

# Or run the JAR directly
java -jar target/kafka-demo-1.0.0.jar
```

## How It Works

1. The `KafkaConsumer` class listens to the configured topic (`demo-topic` by default)
2. When messages are published to the topic, the consumer receives them
3. Each message is logged with the format: "Consumed message: {message}"

## Testing the Consumer

To test the consumer, you need to:

1. **Start Kafka** (if not already running):
   ```bash
   # Start Zookeeper
   bin/zookeeper-server-start.sh config/zookeeper.properties
   
   # Start Kafka
   bin/kafka-server-start.sh config/server.properties
   ```

2. **Create the topic** (if it doesn't exist):
   ```bash
   bin/kafka-topics.sh --create --topic demo-topic --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1
   ```

3. **Run the application**:
   ```bash
   mvn spring-boot:run
   ```

4. **Produce test messages**:
   ```bash
   bin/kafka-console-producer.sh --topic demo-topic --bootstrap-server localhost:9092
   > Hello Kafka!
   > This is a test message
   ```

5. **Check the application logs** to see the consumed messages

## Customization

### Change Kafka Server

Update the `bootstrap-servers` in `application.yaml`:
```yaml
spring:
  kafka:
    consumer:
      bootstrap-servers: your-kafka-server:9092
```

### Change Topic Name

Update the topic name in `application.yaml`:
```yaml
kafka:
  topic:
    name: your-topic-name
```

### Change Consumer Group

Update the group-id in `application.yaml`:
```yaml
spring:
  kafka:
    consumer:
      group-id: your-group-id
```

## Dependencies

- Spring Boot Starter
- Spring Kafka
- Spring Boot Starter Test (for testing)

## License

This project is a demonstration project for Kafka consumer configuration with Spring Boot.