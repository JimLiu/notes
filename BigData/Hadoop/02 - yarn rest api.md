0. sql
测试 sql:
```sql
select dt, hour_id, fr0, count(1) 
from view_compass_indicator_regist 
where dt='20190311'
group by dt, hour_id, fr0;
```
1. 查看任务执行状态：
curl http://ip:8088/ws/v1/cluster/apps/application_1542708959653_144771/state
2. 杀死任务：
curl -v -XPUT \
-H "Content-type: application/json" \
-d '{"state":"KILLED"}' \
"ip:8088/ws/v1/cluster/apps/application_1542708959653_144774/state"

