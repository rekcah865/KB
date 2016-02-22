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
