## 删除 cube 
### url
    http://data-bigdata-12-28.bjcq.zybang.com:7070/kylin/api/cubes/aaaaaaaaa
### method
    DELETE
### Exception
```
HBase failed: 'Failed to delete cube.  Caused by: org.apache.hadoop.hbase.client.RetriesExhaustedException: Failed after attempts=1, exceptions:
Mon Aug 05 14:13:07 GMT+08:00 2019, RpcRetryingCaller{globalStartTime=1564985587854, pause=100, retries=1}, org.apache.hadoop.hbase.NotServingRegionException: org.apache.hadoop.hbase.NotServingRegionException: Region kylin_metadata,/execute_output/4623a4ff-3bac-6ea4-19df-b0d2beca25ba-00,1555636520104.9ce9e62089bb4ebb00c65d73641ed73c. is not online on search-as-100-48.bjcq.zybang.com,16020,1557748203582
	at org.apache.hadoop.hbase.regionserver.HRegionServer.getRegionByEncodedName(HRegionServer.java:2928)
	at org.apache.hadoop.hbase.regionserver.RSRpcServices.getRegion(RSRpcServices.java:974)
	at org.apache.hadoop.hbase.regionserver.RSRpcServices.scan(RSRpcServices.java:2259)
	at org.apache.hadoop.hbase.protobuf.generated.ClientProtos$ClientService$2.callBlockingMethod(ClientProtos.java:32295)
	at org.apache.hadoop.hbase.ipc.RpcServer.call(RpcServer.java:2127)
	at org.apache.hadoop.hbase.ipc.CallRunner.run(CallRunner.java:107)
	at org.apache.hadoop.hbase.ipc.RpcExecutor.consumerLoop(RpcExecutor.java:133)
	at org.apache.hadoop.hbase.ipc.RpcExecutor$1.run(RpcExecutor.java:108)
	at java.lang.Thread.run(Thread.java:748)
```

    