# MQTT to Apache Kafka Connect

![Build](https://github.com/drewsilcock/kafka-connect-mqtt/actions/workflows/build/badge.svg)

## Prerequisites

* [Apache Kafka](https://kafka.apache.org/) (2.8.x version) is publish-subscribe messaging rethought as a distributed commit log.

## Usage

**For development:**

* run check (checkstyle, findbugs, test):  
  `./gradlew clean check`

* run project:  
  `connect-standalone.sh /usr/local/etc/kafka/connect-standalone.properties config/mqtt.properties`
    * libs needs to be added to CLASSPATH:
        * `kafka-connect-mqtt-{project.version}.jar`
        * `org.eclipse.paho.client.mqttv3-1.0.2.jar`
        * if used with ssl there are more.. (`./gradlew copyRuntimeLibs` copies all runtime libs to `./build/output/lib`)

**For production:**

* build project: `./gradlew clean jar` - output `./build/libs`

* generate API documentation: `./gradlew javadoc` - output `./build/docs/javadoc`

## Configuration

The configuration can be added to a kafka connect kubernetes deployment descriptor like this:

```yaml
spec:
   class: com.evokly.kafka.connect.mqtt.MqttSourceConnector
   tasksMax: 2
   config:
     mqtt.server_uris: "tcp://broker_uri:1883"
     mqtt.user: "user"
     mqtt.password: "pass"
     mqtt.auto_reconnect: "true"
```

| Parameter  | Default | Description |
|:---|:---|:---|
| kafka.topic | `mqtt` | Kafka topic to put received data; depends on message processor |
| mqtt.client_id | `null` | mqtt client id to use don't set to use random |
| mqtt.clean_session | `true` | use clean session in connection? |
| mqtt.connection_timeout | `30` | connection timeout in seconds |
| mqtt.keep_alive_interval | `60` | keepalive interval in seconds |
| mqtt.auto_reconnect | `false` | flag if client should reconnect when connection is lost |
| mqtt.server_uris | `tcp://localhost:1883` | mqtt server to connect to |
| mqtt.topic | `#` | mqtt topic to subscribe to |
| mqtt.qos | `1` | mqtt qos to use |
| mqtt.ssl.ca_cert | `null` | CA cert file to use if using ssl |
| mqtt.ssl.cert | `null` | cert file to use if using ssl |
| mqtt.ssl.key | `null` | cert priv key to use if using ssl |
| mqtt.user | `null` | username to authenticate to mqtt broker |
| mqtt.password | `null` | password to authenticate to mqtt broker |
| message_processor_class | DumbProcessor.class | message processor to use |

## License

See [LICENSE](LICENSE) file for License
