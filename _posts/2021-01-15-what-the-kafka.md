---
title: What the Kafka!
layout: post
excerpt: This article explores the basic concepts of Apache Kafka with hand draw illustrations and explanations for some of the  
  commonly used terminologies for Kafka.
---

A typical messaging system sends a message point-to-point from the sender to the receiver.

![](https://paper-attachments.dropbox.com/s_C7CD0664535555038C18DBF202656ABC76127B377A7C8572729058D1F53C5EE0_1610712851413_image.png)


If the sender wants to send data to multiple receivers, it has to duplicate information and send it separately to each receiver.

![](https://paper-attachments.dropbox.com/s_C7CD0664535555038C18DBF202656ABC76127B377A7C8572729058D1F53C5EE0_1610713112466_image.png)


Clearly this system doesn’t scale well and is grossly inefficient. This is where a publish-subscribe messaging system comes into the picture. In this system, the publisher sends message to a node to which one or more subscribers are listening to.

![](https://paper-attachments.dropbox.com/s_C7CD0664535555038C18DBF202656ABC76127B377A7C8572729058D1F53C5EE0_1610713424245_image.png)


In the Kafka world, a publisher is called a Producer, and a subscriber is called a Consumer. In the real world, there can be one or more Producers (called a Producer group) sending messages to a node, and one or more Consumers (called a Consumer group) subscribing for messages from a node. A message in Kafka is called a record. Each record is a byte-array that can store objects of any format.

![](https://paper-attachments.dropbox.com/s_C7CD0664535555038C18DBF202656ABC76127B377A7C8572729058D1F53C5EE0_1610713819285_image.png)


A node in the case of Kafka is called a Broker, which has one or more Topics. Topics are categories to which Producer sends a message and consumer subscribes to. Each topic is then divided into partitions so that multiple consumers can read from the same topic in parallel.

![](https://paper-attachments.dropbox.com/s_C7CD0664535555038C18DBF202656ABC76127B377A7C8572729058D1F53C5EE0_1610716193159_image.png)


Each topic in the diagram has 2 partitions. A producer will send a record to a given Topic-Partition and a consumer will read a record from a Topic-Partition. Partitions are log of records and each new record is added at the end of the log. The consumer can decide from which offset in the logs, it wants to read the message.

![](https://paper-attachments.dropbox.com/s_C7CD0664535555038C18DBF202656ABC76127B377A7C8572729058D1F53C5EE0_1610716138452_image.png)


Now having a single broker is not good for fault tolerance, if that Broker goes down. Hence, Kafka allows setting up multiple brokers. A group of brokers is called a cluster. Management of brokers within a cluster, is performed by Zookeeper. There can be one or more clusters in a single Zookeeper instance.

![](https://paper-attachments.dropbox.com/s_C7CD0664535555038C18DBF202656ABC76127B377A7C8572729058D1F53C5EE0_1610716843589_image.png)


In case of a multi broker setup, Each broker will have a Topic → Partitions. These brokers are replicas of each other, with one being the leader, who has the responsibility of replicating records between partitions. If the leader goes down, the Replica will start operating as the leader. This allows us to build a scalable, distributed, and a fault-tolerant architecture.

For more information, read the official [quick start guide](https://kafka.apache.org/quickstart) of Apache Kafka.