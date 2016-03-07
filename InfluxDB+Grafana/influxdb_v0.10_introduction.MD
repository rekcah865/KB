# [Introduction](https://docs.influxdata.com/influxdb/v0.10/introduction/)


### Download

[v0.10.2稳定版](https://influxdata.com/downloads/#influxdb)
	
### Installation

* 需求
	- root或管理员权限
	- 网络端口: 
		- 8083用于管理面板
		- 8086用于客户端HTTP API连接
		- 8088,8091用于集群数据库实例

* 安装
	- Ubuntu & Debian : apt-get
	- RedHat & CentOS : yum
	- SLES & openSUSE : zypper
	- FreeBSD/PC-BSD : pkg
	- Mac OS X : brew
	- Hosted : [Hosted InfluxDB + Granfa](https://customers.influxdb.com/)
* 配置

	产生新的配置文件: influxd config > influxdb.generated.conf
	可以编辑influxdb.generated.conf
	重新开启数据库: influxd -config influxdb.generated.conf
	如果没有-config指定， 默认的

* Hosting on AWS
	- 硬件
		influxdb/wal, influxdb/data
	- 配置实例
	- 配置文件
	```
	...
	[meta]
	  dir = "/mnt/db/meta"
	  ...
	...
	[data]
	  dir = "/mnt/db/data"
	  ...
	wal-dir = "/mnt/influx/wal"
	  ...
	...
	[hinted-handoff]
		...
	dir = "/mnt/db/hh"
		...
	```
	- 权限
	- 其他配置
	
* Nightly and Development Versions


### Getting Started

	influx CLI(command line interface) - 封装InfluxDB包的一个轻量及简单操作数据库的方法
	
* 创建数据库

The _internal database is created and used by InfluxDB to store internal runtime metrics.

```
$ influx
Connected to http://localhost:8086 version 0.10.0
InfluxDB shell 0.10.0
> CREATE DATABASE mydb
>
> SHOW DATABASES
name: databases
---------------
name
_internal
mydb

> USE mydb
Using database mydb
> 
> 
```

* 写和查询数据

InfluxDB的数据是以时序(time series)来组织的, schema不需要事先被定义，空值是不会被存储的
每个measurement至少有一个key-value, 零个或多个用来包含value的metadata的tags

measurement 相当于一个SQL表，主索引总是time。tags和fields相当于有效的列，其中tag是索引而fields不是

*Line Protocol*
```
<measurement>[,<tag-key>=<tag-value>...] <field-key>=<field-value>[,<field2-key>=<field2-value>...] [unix-nano-timestamp]
```

*数据点例子*
```
cpu,host=serverA,region=us_west value=0.64
payment,device=mobile,product=Notepad,method=credit billed=33,licenses=3i 1434067467100293230
stock,symbol=AAPL bid=127.46,ask=127.48
temperature,machine=unit42,type=assembly external=25,internal=37 1434067467000000000
```

*CLI - Insert*
```
> INSERT cpu,host=serverA,region=us_west value=0.64
> SELECT host, region, value FROM cpu
name: cpu
---------
time		    	                     host     	region   value
2015-10-21T19:28:07.580664347Z  	serverA	  us_west	0.64

> 
```

*查询 - 基于Go风格*

```
> SELECT * FROM /.*/ LIMIT 1
--
> SELECT * FROM cpu_load_short
--
> SELECT * FROM cpu_load_short WHERE value > 0.9
```