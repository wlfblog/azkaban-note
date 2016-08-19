## azkaban plugin 编译
- version  
3.0.0  

### 需要安装以下js  
- 安装dust.js  
sudo npm install dustjs-linkedin  
- 安装less.js  
sudo npm install -g less  

## azkaban 编译



## 安装  
- 安装mysql  

- 创建元数据库  

source create-all-sql-3.0.0.sql  

azkaban3.0.0 还需要执行  

update.active_executing_flows.3.0.sql  
update.execution_flows.3.0.sql  


```
Exception in thread "main" azkaban.executor.ExecutorManagerException: Error fetching active flows
	at azkaban.executor.JdbcExecutorLoader.fetchActiveFlows(JdbcExecutorLoader.java:216)
	at azkaban.executor.ExecutorManager.loadRunningFlows(ExecutorManager.java:431)
	at azkaban.executor.ExecutorManager.<init>(ExecutorManager.java:126)
	at azkaban.webapp.AzkabanWebServer.loadExecutorManager(AzkabanWebServer.java:261)
	at azkaban.webapp.AzkabanWebServer.<init>(AzkabanWebServer.java:197)
	at azkaban.webapp.AzkabanWebServer.main(AzkabanWebServer.java:739)
Caused by: java.sql.SQLException: Unknown column 'ex.executor_id' in 'on clause' Query: SELECT ex.exec_id exec_id, ex.enc_type enc_type, ex.flow_data flow_data, et.host host, .update_time axUpdateTime, et.id executorId, et.active executorStatus FROM execution_flows ex INNER JOIN  active_executing_flows ax ON ex.exec_id = ax.exec_id INNER JOIN  execxecutor_id = et.id Parameters: []
	at org.apache.commons.dbutils.AbstractQueryRunner.rethrow(AbstractQueryRunner.java:363)
	at org.apache.commons.dbutils.QueryRunner.query(QueryRunner.java:350)
	at org.apache.commons.dbutils.QueryRunner.query(QueryRunner.java:306)
	at azkaban.executor.JdbcExecutorLoader.fetchActiveFlows(JdbcExecutorLoader.java:212)
	... 5 more

```
