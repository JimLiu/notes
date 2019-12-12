## Getting Start
### 提交  
```bash
~/envs/flink-1.9.1/bin/flink run \
-m yarn-cluster \
-yqu realtime \
-p 5 \
-C file:///home/homework/jingqi/apps/flink-stream/flink-connector-kafka-0.11_2.11-1.9.1.jar \
-C file:///home/homework/jingqi/apps/flink-stream/flink-connector-kafka-0.10_2.11-1.9.1.jar \
-C file:///home/homework/jingqi/apps/flink-stream/flink-connector-kafka-0.9_2.11-1.9.1.jar \
-C file:///home/homework/jingqi/apps/flink-stream/flink-connector-kafka-base_2.11-1.9.1.jar \
-yt . \
-ynm Flink-Streaming-by-jingqi-cmd \
-c com.homework.bigdata.MyStreamingJob \
flow-base-1.0-SNAPSHOT.jar
```