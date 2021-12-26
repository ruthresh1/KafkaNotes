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
* Zookeeper - the service that keeps track of brokers and handles availability and leader election
* Amazon MSK - aws service that maintains brokers and zookeeper
* Leader election - if the lead broker fails, then zookeeper elects a leader among the remaining nodes

### Topic
A Topic is a category/feed name to which records are stored and published. All Kafka records are organized into topics. Producer applications write data to topics and consumer applications read from topics.

### Broker
A Kafka broker is modelled as KafkaServer that hosts topics. It receives messages from producers and stores them on disk keyed by unique offset.
It allows consumers to fetch messages by topic, partition and offset. They can create a Kafka cluster by sharing information between each other directly or indirectly using Zookeeper. It has exactly one broker that acts as the Controller.

### Producer
Producers write data to topics (which is made of partitions), they automatically know to which broker and partition to write to, so the developer doesn’t need to know that.
In case of Broker failures, it will automatically recover. If producer sends data without a key, then data is sent in Round Robin Fashion a little bit of data to each one of the brokers in the cluster. It can choose to receive acknowledgement of data writes.
