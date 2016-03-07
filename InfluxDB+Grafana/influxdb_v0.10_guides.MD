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


### Downsampleing and Data Retention
