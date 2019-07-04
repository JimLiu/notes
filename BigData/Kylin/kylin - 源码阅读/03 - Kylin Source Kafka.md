### MR 框架消费 KAFKA
1. 主类: KafkaFlatTableJob
2. 输入格式：KafkaInputFormat
创建一个 Consumer, 拿到相应 topic 的相关信息，比如 分区数，开始与结束的 offset.
有几个 partition 
3. RecordReader: KafkaInputRecordReader
创建一个 Consumer 消费对应的 topic 指定的 Partition 与 offset range.
4. 输入分片: KafkaInputSplit