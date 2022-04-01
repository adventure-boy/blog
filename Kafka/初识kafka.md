## kafka概念

Kafka是由[Apache软件基金会](https://baike.baidu.com/item/Apache软件基金会)开发的一个开源流处理平台，由[Scala](https://baike.baidu.com/item/Scala/2462287)和[Java](https://baike.baidu.com/item/Java/85979)编写。Kafka是一种高吞吐量的[分布式](https://baike.baidu.com/item/分布式/19276232)发布订阅消息系统，它可以处理消费者在网站中的所有动作流数据。 这种动作（网页浏览，搜索和其他用户的行动）是在现代网络上的许多社会功能的一个关键因素。 这些数据通常是由于吞吐量的要求而通过处理日志和日志聚合来解决。 对于像[Hadoop](https://baike.baidu.com/item/Hadoop)一样的[日志](https://baike.baidu.com/item/日志/2769135)数据和离线分析系统，但又要求实时处理的限制，这是一个可行的解决方案。Kafka的目的是通过[Hadoop](https://baike.baidu.com/item/Hadoop)的并行加载机制来统一线上和离线的消息处理，也是为了通过[集群](https://baike.baidu.com/item/集群/5486962)来提供实时的消息。（百度百科）

## 5个术语

- productor（生产者）：也就是发消息一方。生产者负责投递消息到kafka
- consumer（消费者）：也就是消费消息的一方。消费者负责从kafka消费消息
- broker（节点）：服务代理节点。可以理解为kafka服务节点或者说kafka实例。一个或者多个节点构成kafka集群
- topic（主题）：kafka消息以主题作为单位进行分类，生产者将消息发送到特定的主题，消费者负责订阅主题并且进行消费
- partition分区：主题是一个逻辑上的概念，一个主题可以包括多个分区，同一主题不同分区消息不同，消息在分区的末尾追加，并且会分配一个特定的偏移量（offset）。offset是消息在分区中的唯一表示，kafka偏移量不跨分区，只是分区有序而不是主题有序。

