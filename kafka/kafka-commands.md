## Kafka commands cheatsheet


### List all acls for a topic
`kafka-acls --bootstrap-server localhost:9092 --list --topic $TOPIC_NAME`
### List of all topics
`kafka-topics --bootstrap-server localhost:9092 --list`
### List all acls
`kafka-acls --bootstrap-server localhost:9092 --list`
### Describe group and topic subscriptions / offsets
`kafka-consumer-groups --bootstrap-server localhost:9092  --describe --group $GROUP_NAME`
### Reset the offset for a specific group and specific topic
`kafka-consumer-groups --bootstrap-server localhost:9092 --group $GROUP_NAME --reset-offsets --to-earliest --execute --topic $TOPIC_NAME`
### Consume messages from a topic and print them to the console
`kafka-console-consumer --bootstrap-server localhost:9092 --topic $TOPIC_NAME --from-beginning --property print.key=true --property print.value=false  --property key.separator=:`
### Print out number of messages in a topic.  Requires `bc` installed
`kafka-run-class kafka.tools.GetOffsetShell --broker-list localhost:9092  --topic $TOPIC_NAME --time -1 | while IFS=: read topic_name partition_id number; do echo "$number"; done | paste -sd+ - | bc`
### Connect to kafka broker using kafcakat using SASL_PLAINTEXT 
`kafkacat -b localhost:9092 -X security.protocol=SASL_PLAINTEXT -X 
sasl.mechanisms=PLAIN -X sasl.username=admin -X sasl.password=admin-secret -L`