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
A Topic is a category/feed name to which records are stored and published. All Kafka records are organized into topics. Producer applications write data to topics and consumer applications read from topics. Similar to a table in a database (without all the constraints), one can have as many topics as you want. It is identified by its name. They are split into Partitions.

### Broker
A Kafka broker is modelled as KafkaServer that hosts topics. It receives messages from producers and stores them on disk keyed by unique offset. It allows consumers to fetch messages by topic, partition and offset. They can create a Kafka cluster by sharing information between each other directly or indirectly using Zookeeper. It has exactly one broker that acts as the Controller.
A Kafka cluster is composed of multiple brokers (servers). Each broker is identified with its ID (integer). It cannot be named like “My Broker” or something. Each broker contains certain topic partitions. Each broker contains some kind of data but not all data, because Kafka is distributed. After connecting to any broker (called a bootstrap broker), you will be connected to entire cluster. A good number to get started is 3 brokers, but some big clusters have over 100 brokers.

### Producer
Producers write data to topics (which is made of partitions), they automatically know to which broker and partition to write to, so the developer doesn’t need to know that. In case of Broker failures, it will automatically recover. If producer sends data without a key, then data is sent in Round Robin Fashion a little bit of data to each one of the brokers in the cluster. It can choose to receive acknowledgement of data writes as 
* 0 - no wait for acknowledgement.
* 1 - wait for leader acknowledgement.
* all - wait for acknowledgement from leader + replicas.

### Consumer
Consumers read data from a topic (identified by name). They know which broker to read from. In case of broker failures, consumers know how to recover. Data is read in order within each partitions but there is no Reading in Order between Partitions. They usually operate in a group where partitions are assigned among them.

### Partition
Partitions are similar like columns in a table where each partition is ordered. Each message within a partition gets an incremental id, called offset. You as a user have to specify the number of Partitions for a Topic. The first message to Partition 0 starts with offset 0 then increments thereafter, where offsets can go to infinite, since they are unbounded. They can have different number of messages (basically offsets), since they are independent

### Zookeeper
Zookeeper manages brokers. It holds the brokers together (keeps a list of them). It helps in performing leader election for partitions, when a broker goes down a new replicated partition of another broker becomes the leader. It sends notifications to kafka in case of changes (e.g new topic, broker dies, broker comes up, delete topics, etc…). Kafka cannot work without a Zookeeper. So we first need to start Zookeeper. Zookeeper by design operates with an odd number of servers (3,5,7) in production. It also follows the concept of Leaders and Followers. Zookeeper that has a leader (handles writes) the rest of the zookeeper servers are followers (handles reads)

### Offset
Kafka stores the offsets at which a consumer group has been reading. The offsets committed live in a Kafka topic named “__consumer_offsets” (double underscore followed by consumer then followed by a single underscore then finally followed by offsets). When a consumer in a group has processed data received from Kafka, the consumer then should be committing the offsets to the topic name “__consumer_offsets”. This is done automatically in Kafka. If a consumer dies, it will be able to read back from where it left off, thanks to the committed consumer offsets!

### Kafka Guarantees
Messages are appended to a topic partition in the order they are sent.
Consumers read messages in the order stored in a topic partition
With a replication factor of N, producers and consumers can tolerate up to N-I brokers being down.
This is why a replication factor of 3 is a good idea:
Allows for one broker to be taken down for maintenance
Allows for another broker to be take down unexpectedly
As long as the number of partitions remains constant for a topic (no new partitions), the same key will always go to the same partition
