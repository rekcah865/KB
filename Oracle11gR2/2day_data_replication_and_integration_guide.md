# [2 Day + Data Replication and Integration Guide](http://docs.oracle.com/cd/E11882_01/server.112/e17516/toc.htm)

###简介:


### 1 Introduction to Data Replication and Integration

本指南教你如何进行日常不同类型的数据复制和整合任务.
本指南还提供数据复制和整合环境下的配置、维护、监控和故障排除
    
* 通过分布式SQL、复制和消息队列来有效的利用你的技术资源
	- 数据库之间数据复制
	- 提供方便的分布式数据库访问
	- Oracle和非Oracle数据库之间数据交换
	- 启用应用之间的沟通(跟客户，合作、供应商之间的数据交换)
	- 事件通知和工作流程

### 2 Common Data Replication and Integration Tasks

* Setting the GLOBAL_NAMES Initialization Parameter to TRUE

若访问多个不同的数据库，首先必须确保每个数据库有唯一标识。
每个数据库的唯一标识符被称为global database name。
由初始化参数设置GLOBAL_NAMES为TRUE ，则保证在分布式数据库环境中的每个数据库可以被唯一标识。
数据库由db_domain和db_name构成全局唯一标识。

* Tutorial: Configuring an Oracle Streams Administrator

	- Oracle高级流复制包括
		- Queue
		- Queue tables
		- Capture process
		- Synchronous captures
		- Propagations
		- Apply processes
		- Rules and rule sets
	- Oracle流复制表空间(streams_tbs)
		- 需要DBMS_STREAMS_AUTH包中的GRANT_ADMIN_PRIVILEGE程序权限
		- DBA角色：需要创建和修改capture processes, synchronous captures, and apply processes
		- EXP_FULL_DATABASE角色和IMP_FULL_DATABASE角色
		- Database Control管理权限： EM
	- Oracle流复制管理员(strmadmin)
		- 不应该使用SYS或者SYSTEM用户作为Oracle流管理员和Oracle流管理员不应该使用SYSTEM表空间作为其默认表空间。
		- Oracle Streams管理员应使用单独区别于其他任何用户专用的专有表空间。 以满足Queue表及流复制组件需要的磁盘空间
		- "EM" "Data Movement" "Setup" in "Stream Section"
		![Images](http://docs.oracle.com/cd/E11882_01/server.112/e17516/img/tdpii_setup.gif)

* Creating an ANYDATA Queue

queue存储Oracle数据流环境中的消息。 
在Oracle Streams复制环境中，queue包含有关数据库更改的信息。 
在Oracle Streams messaging环境，队列存储应用程序和用户的produced和consumed的消息。 通常情况下，在一个Oracle数据流环境中的每个数据库具有一个或多个队列。

ANYDATA队列可以很容易地存储几乎任何类型的信息。

"EM" "Data Movement" "Manage Advanced Queues" in "Stream Section"
![](http://docs.oracle.com/cd/E11882_01/server.112/e17516/img/tdpii_create_queue_type.gif)

也可以使用[DBMS_STREAMS_ADM.SET_UP_QUEUE](http://docs.oracle.com/cd/E11882_01/appdev.112/e40758/d_streams_adm.htm#ARPLS507)来创建ANYDATA Queue

* Tutorial: Creating a Database Link

数据库链接是定义从一个数据库到另一个数据库的单向通信路径的指针。 
Oracle数据库使用的数据库链接，使一个数据库的用户访问远程数据库中的对象。

### 3 Accessing and Modifying Information in Multiple Databases

* About Accessing and Modifying Information in Multiple Databases

当连接到Oracle数据库，您可以访问和修改其他Oracle数据库和非Oracle数据库的信息。
当在两个或多个数据库的信息显示为在单个数据库中，它被称为federation(联合)。 
Federation leaves信息仍然在原来的位置，在那里它被保持和更新。
多个数据源的出现，使得不同类型的数据库在一个统一视图被呈现给被集成到一个单一的虚拟数据库。
联合配置可以让所有的数据库看起来像一个虚拟数据库，应用程序和最终用户，从而减少了一些在分布式系统的复杂性。

	- About Distributed SQL
		```
		可以使用分布式SQL应用程序和用户查询或修改多个数据库中的信息与单个的SQL语句。 
		分布式SQL包含以下内容： distributed queries（其访问数据）和distributed transactions（其修改的数据）。 
		在分布式事务中，two-phase commit mechanism可以确保数据通过确保事务中的所有语句提交或在参与分布式事务的每个数据库回滚作为一个单元的完整性。
		```
		```
		当应用程序或用户尝试提交分布式事务，到应用程序或用户所连接的数据库称为global coordinator。 
		全局协调完成两相通过启动以下阶段提交：
		- 准备阶段：全局协调请求所涉及的分布式事务的其他数据库，以确认它们可以提交或回滚事务，即使有一个发生故障。 如果有任何数据库无法完成准备阶段，那么事务回滚。
		- 提交阶段：如果所有其他数据库的通知，他们正在准备的全局协调人，那么全局协调人提交事务，并要求所有其他数据库提交事务。
		```
	- About Synonyms and Location Transparency
	
		```
		同义词是一个数据库对象，它充当另一个数据库对象的别名。 
		您可以创建公共和私有同义词。 每个数据库用户可以访问公共同义词。 
		私人同义词是在一个特定的用户的模式，只有被授予的用户才可以使用它。

		在分布式环境中，同义词数据库对象提供了位置透明性。 
		同义词隐藏应用程序和用户数据库对象的位置。 如果数据库对象必须被移动或重命名，则可以重新定义同义词，应用程序和用户可以继续使用同义词无需做任何修改。
		```
	- About Accessing and Modifying Information in Non-Oracle Databases
		```
		您可以在Oracle数据库中非Oracle数据库的federate data使用分布式SQL。
		Oracle数据库Gateway使Oracle数据库访问和修改一些非Oracle数据库的数据，包括Sybase，DB2，Informix和Microsoft SQL Server的，Ingres，和Teradata数据库。 
		这种访问对最终用户是完全透明的。 
		也就是说，你可以发出相同的SQL语句，无论您是在Oracle数据库或非Oracle数据库访问数据。
		```
	- About Stored Procedures
		```
		要在federated环境中进行复杂的操作时，为减少网络流量，您可以使用存储过程。 
		procedure 或function是运行解决特定问题或执行一组相关任务的schema对象。 
		通常，使用一个过程来执行操作，并且使用函数来计算值。 
		在本指南中，一般的stored procedure既包括过程和函数。

		Oracle数据库支持PL/SQL或Java程序的存储过程，但该指南只讨论PL/SQL存储过程。
		PL/SQL存储程序由一组被分组和存储在数据库中的SQL语句和其他的PL/SQL结构。 
		存储过程让你结合SQL的易用性和灵活性，实现结构化编程语言的程序功能。

		由于使用SQL语句，运行一个存储过程，你并不需要知道它的物理位置。
		同样，通过使用相应的Oracle数据库Gateway，你甚至可以调用存储过程，在非Oracle数据库。 
		在这种情况下，Gateway映射的PL/SQL调用非Oracle数据库存储的过程。
		```
		
* Preparing to Access and Modify Information in Multiple Oracle Databases

准备
- 在分布式环境中的每个Oracle数据库设置GLOBAL_NAMES初始化参数TRUE。
- 配置的网络连接，以使数据库可以互相通信。

* Tutorial: Querying Multiple Oracle Databases
* Tutorial: Modifying Data in Multiple Oracle Databases
* Tutorial: Running a Stored Procedure in a Remote Oracle Database
* Working with Data in Non-Oracle Databases
	- Configuring Oracle Databases to Work with Non-Oracle Databases
		- 安装并为每个非Oracle数据库的配置Oracle数据库网关软件
		- 配置Oracle网络服务
		- 创建数据库链接到非Oracle数据库
	- Best Practices for Working with Non-Oracle Databases
		Oracle数据库网关性能是由几个因素，包括网络速度，可用存储器，数据量从一个数据库传送到其他，和并发会话的数目的影响。
		- Reduce Post Processing
		- Tune the Non-Oracle Database
		- Set the Relevant Initialization Parameters
		- Check the Location of the Oracle Database Gateway Installation
		- Ensure Adequate Memory
		- Consider Case Differences
	- 可参考[Oracle Database Heterogeneous Connectivity User's Guide)(http://docs.oracle.com/cd/E11882_01/server.112/e11050/toc.htm)
	
### 4 Replicating Data Using Oracle Streams

* About Oracle Streams Replication
	
	```
	Replication是在多个数据库共享数据库对象和数据的过程。 为维持在多个数据库的数据库对象和数据，在数据库中的变化，以这些数据库对象中的一个与其它数据库共享。 以这种方式，数据库对象和数据被保持在所有在复制环境的数据库同步。
	
	Oracle Streams是Oracle数据库的连续复制的功能。
	```
	```
	如果更改一个共享的数据库对象所做的，Oracle流执行以下操作，以确保相同的变化是在其它各个数据库到相应的共享数据库对象进行的：
		- Oracle Streams automatically captures the change and stages it in a queue.
		- Oracle Streams automatically pushes the change to a queue in each of the other databases that contain the shared database object.
		- Oracle Streams automatically consumes the change at each of the other databases. 
		During consumption, Oracle Streams dequeues the change and applies the change to the shared database object.
	```
	![](http://docs.oracle.com/cd/E11882_01/server.112/e17516/img/tdpii022.gif)
	
	```
	当您使用Oracle streams捕获更改数据库对象，这些变化被格式化成 logical change records (LCRs)。 
	一个LCR是与描述的数据库改变的特定格式的消息。 
	如果变化是数据操纵语言（DML）操作，然后连续LCR封装由DML操作所造成的每一行的变化。
	一个DML操作可能导致多行更改，因此一个人DML操作可能导致多行的LCR。 
	如果变化是数据定义语言（DDL）操作，则单个DDL LCR封装DDL变化。
	```
	- About Change Capture
		- Oracle流提供了两种方式自动捕获数据库(source database)更改：
			- 使用capture process以捕获数据操纵语言（DML）的变化为相对大量的表，整个模式，或者一个数据库。 此外，捕获过程必须用于捕获数据定义语言（DDL）变为表和其它数据库对象。
			- 使用synchronous capture到捕获的DML变化相对小的数目的表。
		- Capture process
			- 一个可选的Oracle数据库后台进程异步捕获记录在重做日志的变化。
			- Enqueuing: 是将消息放入队列的过程。
			- 一个捕获过程中始终与单个队列相关联，并将排到的LCR放入队列。buffered queue
			![Capture Process](http://docs.oracle.com/cd/E11882_01/server.112/e17516/img/tdpii012.gif)
			- local capture process: 在源数据库上执行的捕捉过程
				- 容易配置和管理
				- 可适用于不同平台
			- downstream capture process: 在远程数据上执行的捕捉过程
				- 源数据库资源需求少
			```
			
			```
		- Synchronous Capture
			- 同步捕捉使用的内部机制(而不是redolog)来捕获数据操纵语言（DML）的变化
			- 同步捕获总是排排入到的LCR到persistent queue(存在磁盘上的queue表而非内存) 
			![Synchronous Capture](http://docs.oracle.com/cd/E11882_01/server.112/e17516/img/tdpii058.gif)
	- About Change Propagation Between Databases
	
	- About Change Apply
	
	- About Rules for Controlling the Behavior of Capture, Propagation, and Apply
	
	- About Rule-Based Transformations for Nonidentical Copies
	
	- About Supplemental Logging
	
	- About Conflicts and Conflict Resolution
	
	- About Tags for Avoiding Change Cycling
	
	- About the Common Types of Oracle Streams Replication Environments
	
	- About Key Oracle Streams Supplied PL/SQL Packages and Data Dictionary Views
	
* Preparing for Oracle Streams Replication

* Configuring Oracle Streams Replication: Examples

### 5 Administering an Oracle Streams Replication Environment
### 6 Extending an Oracle Streams Replication Environment
### 7 Replicating Data Using Materialized Views
### 8 Administering a Materialized View Replication Environment
### 9 Sending Messages Using Oracle Streams Advanced Queuing
### 10 Comparing and Converging Data
    