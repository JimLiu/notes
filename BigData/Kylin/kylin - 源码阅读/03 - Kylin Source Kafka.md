### MR 框架消费 KAFKA
1. 主类: KafkaFlatTableJob
2. 输入格式：KafkaInputFormat
创建一个 Consumer, 拿到相应 topic 的相关信息
3. RecordReader: KafkaInputRecordReader
4. 输入分片: KafkaInputSplit