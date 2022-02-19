# k8s-confluent-schema-registry-and-gui

This repo represents kubernetes deployment for Confluent Schema Registry and its UI.

The GUI Web application is based on project at https://github.com/lensesio/schema-registry-ui.

This deployment expects Apache Kafka cluster is already up and running.

Confluent schema registry uses Kafka to store underlying AVRO Schemas.

Various properties of deployment can be controlled by Environment variables to the PODS.

          ```
          env:
          - name: SCHEMA_REGISTRY_KAFKASTORE_BOOTSTRAP_SERVERS
            value: PLAINTEXT://kfknode01:9092,PLAINTEXT://kfknode02:9092,PLAINTEXT://kfknode03:9092
          - name: SCHEMA_REGISTRY_LISTENERS
            value: http://0.0.0.0:8081
          - name: SCHEMA_REGISTRY_HOST_NAME
            valueFrom:
              fieldRef:
                fieldPath: status.podIP
          - name: SCHEMA_REGISTRY_HEAP_OPTS
            value: -Xms512M -Xmx1g
          - name: SCHEMA_REGISTRY_ACCESS_CONTROL_ALLOW_METHOD
            value: GET,POST,PUT,OPTIONS
          - name: SCHEMA_REGISTRY_ACCESS_CONTROL_ORIGIN_ALLOW
            value:  "*"
          - name: SCHEMA_REGISTRY_KAFKASTORE_TOPIC
            value:  "data-avro-schemas"
          - name: SCHEMA_REGISTRY_SCHEMA_COMPATIBILITY_LEVEL
            value:  "full_transitive"
            ```
            
 The schema registry hostname will be dynamic with every new pod being spawned because of nature of Kubernetes.
 
 This is why, the variable `SCHEMA_REGISTRY_HOST_NAME` will get its hostname from podIP dynamically.
 
 Other important Environment variable being `SCHEMA_REGISTRY_KAFKASTORE_BOOTSTRAP_SERVERS` which needs to be provided with Kafka broker listeners addresse.
 `SCHEMA_REGISTRY_KAFKASTORE_TOPIC` will represent the Kafka topic where schemas are supposed to be stored.
