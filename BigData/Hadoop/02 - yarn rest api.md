1. 查看任务执行状态：
curl http://ip:port/ws/v1/cluster/apps/application_1542708959653_144771/state
2. 杀死任务：
curl -v -XPUT \
-H "Content-type: application/json" \
-d '{"state":"KILLED"}' \
"ip:port/ws/v1/cluster/apps/application_1542708959653_144774/state"
