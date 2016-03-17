# [2 Day + Performance Tunning Guide](http://docs.oracle.com/cd/E11882_01/server.112/e10822/toc.htm)

###简介:
    

###Part I Getting Started  

####1 Introduction  
      
      优化数据库工具：Oracle企业版、OEM、Oracle Diagnostics Pack(AWR,ADDM,ASH)、
      Oracle Database Tuning Pack(SQL Tuning Advisor,SQL Access Advisor)、
      Oracle Real Application Testing(Database Replay, SQL Performance Analyzer)
      
      
####2 Oracle Database Performance Method  
      
	  bottleneck, throughput, response time
      AWR收集统计信息
      - 2个参数：STATISTICS_LEVEL, CONTROL_MANAGEMENT_PACK_ACCESS
      - 5个统计信息：Time Model, Wait Event, Session and System, Active Session History, High-Load SQL
      
      性能优化方法
	  - Performing pre-tuning preparations
	  - Tuning the database proactively on a regular basis
	  - Tuning the database reactively when performance problems are reported by the users
	  - Identifying, tuning, and optimizing high-load SQL statements
      
      常见的性能问题
      - CPU bottlenecks
      - Undersized memory structures
      - I/O capacity issues
      - Suboptimal use of Oracle Database by the application
      - Concurrency issues
	  - Database configuration issues
	  - Short-lived performance problems
	  - Degradation of database performance over time
	  - Inefficient or high-load SQL statements
	  - Object contention
	  - Unexpected performance regression after tuning SQL statements
      
      
###Part II Proactive Database Tuning  
    
####3 Automatic Database Performance Monitoring  

自动数据库诊断监视器（ADDM）自动检测并报告数据库的性能问题。
结果显示在Oracle企业管理器（EM）数据库主页上的ADDM findings。
回顾ADDM发现结果使您能够快速确定需要您注意的性能问题。

每个ADDM finding提供的建议列表用于降低对数据库性能问题的影响。
您应该查看ADDM结果和执行每天的日常数据库维护的有关建议。
即使该数据库以最佳性能水平运行，您应该继续使用ADDM来持续不断地监视数据库的性能。

* Overview of Automatic Database Diagnostic Monitor

	ADDM是内置于Oracle数据库的自诊断软件。 
	ADDM检查和分析自动负载信息库（AWR）采集的数据以确定可能的数据库性能问题。 
	ADDM定位性能问题的root causes，针对于纠正提供建议，并量化预期的收益。 
	ADDM还确定了在哪些领域是必要没有行动的。
	
	- ADDM分析

		每个AWR快照（默认一个小时）之后进行ADDM分析，并且结果被保存在数据库中。
		然后，您可以使用Enterprise Manager查看结果。
		使用本指南中规定的其他性能调整方法之前，首先查看ADDM分析的结果。

		ADDM分析是从上而下进行的，首先识别symptoms，然后精制分析以致得到性能问题的根本原因。 
		ADDM使用DB time statistic统计来确定性能问题。
		DB时间是由数据库在处理用户请求时的累计时间，包括等待时间和所有用户会话（非闲置）的CPU时间花费。

		数据库的性能调整的目标是对于给定的工作量减少系统的DB time。
		通过减少DB time，数据库可以通过使用相同或更少的资源支持更多的用户请求。		
		ADDM使用的DB time来报告系统资源，并通过相关数据库花费的时间量降序排序。
		有关DB时间统计的更多信息，请参阅["Time Model Statistics"](http://docs.oracle.com/cd/E11882_01/server.112/e10822/tdppt_method.htm#CIHCACJG)

	- ADDM建议

		除了诊断性能问题，ADDM建议可能的解决方案。
		在适当的时候，ADDM建议多个解决方案供您选择。 
		ADDM建议包括以下内容：
			- 硬件更改
			- 添加CPU或更改I/O子系统配置
			- 数据库配置
			- 更改初始化参数设置
			- 模式更改
			- Hash分区表或索引，或者使用自动段空间管理（ASSM）
			- 应用程序更改
			- 使用sequence缓存选项或使用绑定变量
			- 使用其他advisors
			
		对高负荷SQL语句运行SQL Tuning Advisor或hot object运行Segment Advisor。
		ADDM优点应用生产系统之外，即使在开发和测试系统，ADDM可以提供的潜在的性能问题的早期警告。
		
		性能调整是一个反复的过程。
		解决一个问题可能导致瓶颈转移到该系统的另一部分。
		基于ADDM分析的好处，可以采取多个tunning周期来达到所需的性能水平。

	- Oracle RAC的ADDM
	
		在Oracle真正应用集群（Oracle RAC的）环境中，可以使用ADDM来分析的数据库集群的吞吐量性能。 
		ADDM为Oracle RAC数据库考虑的时间为所有数据库实例的时间总和和报告findings的结果也是在集群级别。
		例如，单独考虑时每个集群节点的DB中的时间可能是微不足道的，但聚合的DB时间可以为集群作为一个整体显著问题。
		
* Configuring Automatic Database Diagnostic Monitor

	- 设置初始化参数启用ADDM
		
		自动数据库诊断监测默认是启用的，由CONTROL_MANAGEMENT_PACK_ACCESS和STATISTICS_LEVEL初始化参数控制。

		设置CONTROL_MANAGEMENT_PACK_ACCESS为DIAGNOSTIC+TUNING （默认）或DIAGNOSTIC 则启用自动数据库诊断监控。
		设置CONTROL_MANAGEMENT_PACK_ACCESS为NONE禁用许多Oracle数据库特性（包括ADDM）-不被推荐。

		设置STATISTICS_LEVEL为TYPICAL（默认）或ALL以启用自动数据库诊断监控。
		设置STATISTICS_LEVEL为BASIC禁用许多Oracle数据库特性（包括ADDM）-不被推荐。


	- 设置DBIO_EXPECTED参数

		I/O性能的ADDM分析部分依赖于DBIO_EXPECTED参数，它描述了I/O子系统的预期性能。
		DBIO_EXPECTED的值是数据库读取一个数据库块需要的平均时间（微秒计算）。
		Oracle数据库使用10 milliseconds的默认值，这对于大多数的硬盘驱动器的适当值。
		您可以选择基于硬件的特性不同的值。
		```
		EXECUTE DBMS_ADVISOR.SET_DEFAULT_TASK_PARAMETER('ADDM', 'DBIO_EXPECTED', 8000);
		```
		
	- 管理AWR快照

		默认情况下，自动负载信息库（AWR）每隔一小时生成性能数据的快照，并保留8天的工作量信息库的统计信息。
		您可以改变这两个快照间隔和保持周期的默认值。

		Oracle建议你调整AWR保留期限至少一个月。
		您也可以延长至一个业务周期，因此您可以比较不同时间框架数据，如财政季度结束。
		您还可以创建AWR baselines无限期地保留快照重要的时间段。

		在快照间隔中的数据被ADDM分析。 ADDM比较快照之间的差异，在捕捉到的SQL语句的基础上确定对系统负载的效果。 
		ADDM的分析也会显示在一段时间被捕获的SQL语句的数目。

		- Creating Snapshots
		- Modifying Snapshot Settings

* Reviewing the Automatic Database Diagnostic Monitor Analysis

	![Images](http://docs.oracle.com/cd/E11882_01/server.112/e10822/img/diagnostic_sum.gif)
	
* Interpretation of Automatic Database Diagnostic Monitor Findings

	ADDM的分析结果表示为一组结果。每个ADDM发现属于三种类型之一：
		- Problem 描述数据库性能问题的根本原因的Findings 
		- Symptom 包含往往导致的一个或多个问题的发现信息的Findings 
		- Information 被用来报告系统区域的发现，即不具有性能影响

	每一个问题的findings与导致性能问题的DB time估计量化。
	
	当一个特定的问题有多种原因时，可能ADDM报告多个结果。
	在这种情况下，这些多个结果的影响可以包含相同部分的DB Time。
	因为性能问题可以重叠，总结报告的结果的影响可以产生若干数据块时高于100％。
	例如，如果一个系统执行许多读I/O操作，ADDM可能报告50% DB time的SQL语句（IO活动作为一个finding，DB time 75%较小缓冲区作为另外一个finding）。
	
	一个问题的发现与性能问题影响的建议列表相关联。
	每个建议具有即如果该建议被实现可以保存在的数据库时间部分的估计值的好处。
	当多个建议与ADDM发现相关，这些建议可能包含解决同一问题的替代品。
	在这种情况下，所带来的好处的总和可大于发现的影响越高。
	您不必应用所有的建议，以解决同样的问题。

	建议是由行动和理由组成。
	您必须应用的建议获得其预计受益的一切行动。
	基本原理解释了为什么被推荐的一套动作，并实现它们提供更多的信息。
	一个ADDM行为可能存在多种解决方案。
	如果是这种情况，然后选择最容易实现的解决方案。

* Implementing Automatic Database Diagnostic Monitor Recommendations

	![Images](http://docs.oracle.com/cd/E11882_01/server.112/e10822/img/perf_analysis.gif)
	
* Viewing Snapshot Statistics

	![Images](http://docs.oracle.com/cd/E11882_01/server.112/e10822/img/snapshot_det.gif)
	
####4 Monitoring Real-Time Database Performance  
####5 Monitoring Performance Alerts  
      
###Part III Reactive Database Tuning  

####6 Manual Database Performance Monitoring  
####7 Resolving Transient Performance Problems  
####8 Resolving Performance Degradation Over Time  
      
###Part IV SQL Tuning  

####9 Identifying High-Load SQL Statements  
####10 Tuning SQL Statements  
####11 Optimizing Data Access Paths  
