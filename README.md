# KafkaNotes

Kafka is a distributed and scalable messaging system for real-time consumption of messages. It outperforms traditional messaging systems in many ways.

## Use cases
* messaging system - real time messages can be sent and received in millions
* activity tracking - aggregate user activing on a website for offline processing
* real time stream processing - process continuous stream of information in real time and pass it along
* log aggregation - collect multiple logs from different systems and store, like in HDFS
* commit log service - external commit log for distributed systems
* event sourcing - maintaining time ordered sequence of events

## Components and terms associated
* Broker - The relay system that accepts the produced message and communicates it to the consumer.
* Producer - the service that creates a message for particular service and sends it to the broker.
* Consumer - the service that listens, accepts, processes message from broker
* Topic - the title of the message, used to identified what message is sent from producer to consumer
* Message - the information passed from producer to consumer (in any data format - json, xml)
* Partition - messages that reach the broker are kept seperately into different boxes named partitions to which specific listeners listen from
* Zookeeper
* Amazon MSK
* Leader election
