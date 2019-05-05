### kylin 配置
1. 用来解决 kylin: kyro.regist() error , default: 1
   kylin.engine.mr.uhc-reducer-count=50
2. 用来解决: Container running beyond physical memory limits. Current usage: 527.1 MB of 512 MB physical memory used; 2.3 GB of 2.1 GB virtual memory used. Killing container
	<property>
  		<name>mapreduce.map.memory.mb</name>
  		<value>1024</value>
	</property>
3. 