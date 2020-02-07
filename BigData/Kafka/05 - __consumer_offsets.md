## consumer_offsets
### 背景
    有的程序会将 offset 信息提交到 __consumer_offsets 这个 topic 里面. 我们有时候需要从里面将相应的 offset 记录下来.
### 命令行
```bash
$KAFKA_HOME/bin/kafka-console-consumer.sh \
--bootstrap-server ip:port \
--topic __consumer_offsets \
--formatter 'kafka.coordinator.group.GroupMetadataManager$OffsetsMessageFormatter' \
--partition 2

# 其中, --partition 是可选命令
```

### 计算 group id 对应的 partition
    Math.abs("Fast-ETL-Example-jingqi-test".hashCode) % 50
    // 以上命令可以在 scala 命令行下执行.