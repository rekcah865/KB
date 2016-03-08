# [Guides](https://docs.influxdata.com/influxdb/v0.10/guides/)

### Writting Data

很多种方式写数据，比如CLI，Client库或者通用数据格式的插件(如Graphite)

* 通过HTTP API创建数据
	- 向终点/query发出GET请求且设置URL参数q值为CREATE DATABASE <new_database_name>
	```command
	curl -G http://localhost:8086/query --data-urlencode "q=CREATE DATABASE mydb"
	```
* 通过HTTP API写数据
	- 向终点/write发出POST请求且设置URL参数db为所操作的数据库
	- POST的内容称作为Line Protocol, 数据包括measurement,field key, field value,及可选tag keys, tag values, timestamp
	- 默认timestamp为服务器时间nanosecond(Unix epoch) UTC
	```
	curl -i -XPOST 'http://localhost:8086/write?db=mydb' --data-binary 'cpu_load_short,host=server01,region=us-west value=0.64 1434055562000000000'
	```
	- 写多个points数据 （新行隔开）
	```
	curl -i -XPOST 'http://localhost:8086/write?db=mydb' --data-binary 'cpu_load_short,host=server02 value=0.67
	cpu_load_short,host=server02,region=us-west value=0.55 1422568543702900257
	cpu_load_short,direction=in,host=server01,region=us-west value=2.0 1422568543702900257'
	```
	- 从文件写入points @filename （文件内数据遵循line protocol语法)
	```
	$ cat cpu_data.txt
	cpu_load_short,host=server02 value=0.67
	cpu_load_short,host=server02,region=us-west value=0.55 1422568543702900257
	cpu_load_short,direction=in,host=server01,region=us-west value=2.0 1422568543702900257
	...
	$ curl -i -XPOST 'http://localhost:8086/write?db=mydb' --data-binary @cpu_data.txt
	```
	- 默认HTTP请求timeout=5秒，如果超过5000数据点可以分为多个文件
	- Schemaless设计： 任何时候可以增加measurement, tags, fields
	- HTTP数据传输协议，很好的解决流行的WEB API REST
	
* HTTP响应总结
	- 2XX
		- HTTP 204 No Content: 成功
		- HTTP 200 OK: InfluxDB明白请求但是无法完成，反馈时将会包含额外的错误信息
	- 4XX: InfluxDB不明白请求
	```
	$ curl -i -XPOST 'http://localhost:8086/write?db=hamlet' --data-binary 'tobeornottobe booleanonly=true'  
	$ curl -i -XPOST 'http://localhost:8086/write?db=hamlet' --data-binary 'tobeornottobe booleanonly=5'
	HTTP/1.1 400 Bad Request
	[...]
	write failed: field type conflict: input field "booleanonly" on measurement "tobeornottobe" is type float64, already exists as type boolean
	```
	```
	$ curl -i -XPOST 'http://localhost:8086/write?db=atlantis' --data-binary 'liters value=10'
	HTTP/1.1 404 Not Found
	[...]
	database not found: "atlantis"
	```
	- 5XX: 系统超载或者明显受损
	
### Querying Data

* 通过HTTP API查询数据
	- 向终点/query发出GET请求，且设置URL的参数db为数据库及参数q为查询语句
	```
	$ curl -G 'http://localhost:8086/query?pretty=true' --data-urlencode "db=mydb" --data-urlencode "q=SELECT value FROM cpu_load_short WHERE region='us-west'"
	```
	- InfluxDB将返回JSON - results数组
	```json
	{
		"results": [
			{
				"series": [
					{
						"name": "cpu_load_short",
						"columns": [
							"time",
							"value"
						],
						"values": [
							[
								"2015-01-29T21:55:43.702900257Z",
								0.55
							],
							[
								"2015-01-29T21:55:43.702900257Z",
								23422
							],
							[
								"2015-06-11T20:46:02Z",
								0.64
							]
						]
					}
				]
			}
		]
	}
	```
* 多点查询
	- 多个查询的语句用分号(;)隔开
	```
	$ curl -G 'http://localhost:8086/query?pretty=true' --data-urlencode "db=mydb" --data-urlencode "q=SELECT value FROM cpu_load_short WHERE region='us-west';SELECT count(value) FROM cpu_load_short WHERE region='us-west'"
	```
* 查询时其他选项
	- 时间格式
		- 默认: RFC3339 UTC + 纳秒, 例如: 2015-08-04T19:05:14.318570484Z
		- 可使用epoch支持Unix epoch ；epoch=[h,m,s,ms,u,ns]
		```
		$ curl -G 'http://localhost:8086/query' --data-urlencode "db=mydb" --data-urlencode "epoch=s" --data-urlencode "q=SELECT value FROM cpu_load_short WHERE region='us-west'"
		```
	- 认证：默认被禁用，可参考[ Authentication安装和设置](https://docs.influxdata.com/influxdb/v0.10/administration/authentication_and_authorization/)
	- Chunk Size ：大型查询可以chunk_size设置批次大小
	```
	$ curl -G 'http://localhost:8086/query' --data-urlencode "db=deluge" --data-urlencode "chunk_size=20000" --data-urlencode "q=SELECT * FROM liters"
	```
* InfluxSQL - 参考[Data Exploration](https://docs.influxdata.com/influxdb/v0.10/query_language/data_exploration/)

### Downsampleing and Data Retention

	InfluxDB能够处理成千上万的每秒数据点，对于很长一段时间的大量数据会产生存储的担忧  
	一个自然的解决方案：往下采样数据;保持有限时间内高精度的原始数据，对长时间的数据存储使用精度较低的汇总数据  
	本指南说明了如何结合两个InfluxDB功能 - Retention Policies和Continuous Queries - to automatically downsample and expire data
	
* 保留策略
	-定义：RP用来描述InfluxDB数据的周期和copies数量，每个数据库可以有多个RPs，RP在每个数据库是唯一的
	- 目的：在没有删除流程的InfluxDB中实现老数据的清理
	- 工作
		- 创建数据库时，默认default(无限持续时间)RP并设置复制因子为集群中节点数量
		- 可以指定measurement的RP：<database_name>.<retention_policy>.<measurement_name>
		- 也可以基于数据进行创建、修改和删除自己的RPs

* 连续查询

	- 定义：CQ是一个在数据库中自动定期运行的InfluxSQL，CQs需要求一个SELECT函数且必须包括GROUP BY time()语句
	- 目的：最佳的定期向下取样数据，而不需要简单或普通的查询，InfluxDB将存储结果以备后面使用
	- 工作：[创建和管理CQs](https://docs.influxdata.com/influxdb/v0.10/query_language/continuous_queries/)
	
* 结果RPs和CQs - 学习案例

	```
	每10秒钟来自电话或网站关于餐厅食品追踪的实时数据  
	从长远来说，我们只关心每30分钟通过手机或网站的平均数量  
	接下来，使用InfluxDB的RPs和CQs
	- 自动删除2个小时前的每10秒的原始数据
	- 自动汇总10秒钟的原始数据到30分钟的数据
	- 保持30分钟的数据永不删除
	```
	- 准备数据数据
		- 创建新的default RP
		```
		> CREATE RETENTION POLICY two_hours ON food_data DURATION 2h REPLICATION 1 DEFAULT
			
		> SHOW RETENTION POLICIES ON food_data
		name		      duration	  replicaN	  default
		default		   0		        1		        false
		two_hours	  2h0m0s		   1		        true
		```
		- 创建CQ
			```
			> CREATE CONTINUOUS QUERY cq_30m ON food_data BEGIN SELECT mean(website) AS mean_website,mean(phone) AS mean_phone INTO food_data."default".downsampled_orders FROM orders GROUP BY time(30m) END
			```
	- 写/查看数据
		```
		> SELECT * FROM orders LIMIT 5
		name: orders
		-----------------
		time						            phone 	website
		2015-12-04T20:00:11Z	 1	     6
		2015-12-04T20:00:20Z		9	     10
		2015-12-04T20:00:30Z		2	     17
		2015-12-04T20:00:40Z		3	     10
		2015-12-04T20:00:50Z		1	     15
		```
		```
		> SELECT * FROM food_data."default".downsampled_orders LIMIT 5
		name: downsampled_orders
		------------------------
		time			               mean_phone		       mean_website
		2015-12-03T22:30:00Z	 4.318181818181818	 9.254545454545454
		2015-12-03T23:00:00Z	 4.266666666666667	 9.827777777777778
		2015-12-03T23:30:00Z	 4.766666666666667	 9.677777777777777
		2015-12-04T00:00:00Z	 4.405555555555556	 8.5
		2015-12-04T00:30:00Z	 4.788888888888889	 9.383333333333333
		```

### Hardware Sizing Guidelines

	本指南提供了InfluxDB一般硬件建议和解决有关硬件大小一些常见问题。
	这些建议只对tsm1存储引擎。实例仍在运行未转化b1或BZ1分片可以具有不同的性能特性。
	见InfluxDB0.9评估指南了解更多详情。

* 单节点通用硬件指导
* 什么时候需要更多内存
* 需要什么样的存储？
* 需要多少存储空间？

* 集群通用硬件指导
* 怎么来配置集群中的硬件