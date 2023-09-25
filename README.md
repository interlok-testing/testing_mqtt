# MQTT Testing

[![license](https://img.shields.io/github/license/interlok-testing/testing_mqtt.svg)](https://github.com/interlok-testing/testing_mqtt/blob/develop/LICENSE)
[![Actions Status](https://github.com/interlok-testing/testing_mqtt/actions/workflows/gradle-build.yml/badge.svg)](https://github.com/interlok-testing/testing_mqtt/actions/workflows/gradle-build.yml)

Project tests interlok-mqtt features

## What it does

This project is very simple and contains two channels with one workflow each doing a simple MQTT to file system bridge.

The first workflow is listening on an MQTT topic and write the message on file system.

The second workflow is polling the file system for new files and send it's content to an MQTT topic.

```mermaid
graph LR
  subgraph FS to MQTT
    direction LR
    FS2[File System] --> FS_C(FS Consumer)
    FS_C --> SC2(Service Collection)
    SC2 --> MQTT_P(MQTT Producer)
    MQTT_P --> MQTT_B2[MQTT Broker]
  end
  subgraph MQTT to FS
    direction LR
    MQTT_B1[MQTT Broker] --> MQTT_C(MQTT Consumer)
    MQTT_C --> SC1(Service Collection)
    SC1 --> FS_P(FS Producer)
    FS_P --> FS1[File System]
  end

  style MQTT_C fill:#FF6C6C
  style MQTT_P fill:#6C79FF
  style FS_C fill:#FF6C6C
  style FS_P fill:#6C79FF
  style SC1 fill: #F89347
  style SC2 fill: #F89347
```


## Getting started

* `./gradlew clean build`
* `(cd ./build/distribution && java -jar lib/interlok-boot.jar)`

The config is using a variables.properties to configure the MQTT host, topics and the file system directory.

```
mqttScheme=ws
mqttHost=broker.emqx.io
mqttPort=8083
mqttUrl=${mqttScheme}://${mqttHost}:${mqttPort}
fsDir=file://localhost/./messages/in
topicSource=source
topicDest=destination
```

## Try it

For simplicity I used a test MQTT broker made available by EMQX (https://www.emqx.com/en/mqtt/public-mqtt5-broker) but any MQTT broken can be used.

Once the adapter is up and running you can send a message to the topicSource and check that you receive the same message on the topicDest.
If you are using EMQX you can use their toolkit http://tools.emqx.io/.

