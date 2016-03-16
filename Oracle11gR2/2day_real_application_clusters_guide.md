# [2 Day + Real Application Clusters Guide](http://docs.oracle.com/cd/E11882_01/rac.112/e17264/toc.htm)

###简介
Oracle RAC使Oracle数据库可以跨服务器集群上运行，在应用程序不需要改动下提供容错性、高可用性、高性能和可扩展性    
    
####1 Introduction to Oracle Database 2 Day + Real Application Clusters Guide
        
* 关于此文档
	- Oracle RAC 数据库管理指南
	- 介绍如何配置和管理Oracle Clusterware和Oracle RAC环境
	- 帮助你理解安装和维护Oracle RAC环境（基本故障处理、性能监控、备份和恢复）
* 关于GI for a Cluster and RAC
	- GI是Clusterware和ASM的一个组合产品
	- RAC使得多台服务器（主机/节点）可以像一台服务器来操作和提供服务，提供更大的高可用性、吞吐量和可扩展性
	- 3个概念: Free pool, policy-managed database, administrator-managed database
* 关于Oracle ASM
	- ASM是一个集成的、高性能的卷管理器和文件系统
	- 自11gR2，ASM增加了对Oracle OCR(集群注册表)和Voting disks(表决磁盘)，被称为Oracle ACFS
	- 好处：条带化、冗余、在线存储重配置和动态再平衡、管理文件创建和删除
* 关于Oracle RAC
	- RAC扩展Oracle数据库，以至于你可以在不同服务器上对数据同时进行存取、更新和有效查询
	- 构成数据库的数据文件必须存放在集群里所有的服务器都可以访问的共享存储上(多实例对应1个数据库)
	- Oracle RAC数据库每个数据库实例使用它自己的内存结构和后台进程
	- RAC使用Cache Fusion同步（传输）存放在不同实例的buffer cache中的数据
	- Oracle RAC one Node:
		- 单实例节点集群，使其免受计划内或计划外停机
		- 当One Node数据库超载，可以对应用用户无影响情况下在线升级到cluster
        - RAC不支持一个集群跑在不同平台上，但允许同一平台下有不同型号的机器      
* 安装、配置、管理RAC的工具        
	- OUI 
	- CVU: Cluster Verification Utility
		- 验证一系列集群和RAC组建（共享存储、网络配置、系统需求、Clusterware、操作系统的组和用户）的命令行工具
		- 可以在安装前和安装后检查RAC集群环境 
	- OEM
	- SQL*Plus
	- SRVCTL: Server Control
		- 管理在集群注册表(OCR)定义的资源(nodeapps, Oracle Clusterware)
		- Clusterware包括ONS(Oracle Notification Service), GSD(Global Service Daemon), VIP(Virutal IP)
		- 其他资源包括启动和停止数据库、实例、监听器、服务，删除和移动实例和服务，添加服务和管理配置信息
	- CRSCTL: Cluster Ready Services Control 
		- 管理Oracle集群守护进程(CSS, CRS, EVM)的命令行工具
		- Cluster Synchronization Services, Cluster-Ready Services, Event Manager
		- 可用来启动和停止Clusterware，并可以确定集群安装的状态
	- DBCA
	- ASMCA
	- ASMCMD    
	- LSNRCTL        
        
####2 Preparing Your Cluster

* 验证系统要求
	- OS认证
	- 硬件要求
	- 共享存储: 除数据、控制、重做日志、参数文件，Clusterware需要Voting Disks(3×300MB) OCR(3*300MB)来实现出色的扩展性和高可用性
	- 网络要求： private interconnect - 私网IP
	- IP地址要求： 
		- 可以用GNS(Grid Naming Service)和DHCP(Dynamic Host Configuration Protocol)来实现虚拟IP  
		- 典型安装： DNS中要求-每个节点需要一个公有IP和一个虚拟IP，集群需要三个SCAN(single client access name)地址
	- 操作系统和软件要求	
* 准备服务器
	- 关于OS用户和组
	- 配置OS用户和组
	- 配置SSH(Secure Shell)
	- 配置软件用户shell环境
* 配置网络  
	[参考Example](http://docs.oracle.com/cd/E11882_01/rac.112/e17264/preparing.htm#BCGJBBGE)
* 准备OS和软件
	- 在所有节点上设置时间
		- NTP(Network Time Protocol)
		- 或者CTSS(Cluster Time Synchronization Service)
	- 配置系统内核参数
	- 执行平台特定的配置任务
* 配置安装目录和共享存储
	- 定位Oracle Inventory目录
	- 创建Oracle Grid Infrastrcture for a Cluster主目录
	- 创建Oracle Base目录
	- 关于Oracle主目录
	- 配置共享存储
	- NAS设备上配置文件用于ASM
	- 用ASMLib标记共享磁盘为Candidate磁盘
		- [安装配置AMSLib步骤](http://docs.oracle.com/cd/E11882_01/rac.112/e17264/preparing.htm#BCGFGAIH)
		- 配置磁盘持久性 - udev跟ASMLib区别

####3 Installing Oracle Grid Infrastructure and Oracle Real Application Clusters

* 准备Oracle Media安装文件 
* 验证MOS凭证
* 安装Oracle Grid Infrastructure for a Cluster软件
	- 配置操作系统环境
	- 关于CVU Fixup脚本 （runfixup.sh）
	- 使用OUI安装Oracle Grid Infrastrcture for a Cluster
	- 完成Oracle Clusterware配置
* 安装Oracle Database软件和创建数据库
	- 配置操作系统环境
	- 创建额外的ASM磁盘组
	- OUI安装Oracle RAC软件
	- 验证RAC数据库安装
* 执行Postinstallation任务
	- 验证Oracle Clusterware安装
	- 使用DBCA创建数据库
	- 备份安装文件
	- 验证Enterprise Manager操作
	- 下载和安装RDBMS补丁
	- 配置用户帐户
* 关于转换Oracle数据库到RAC数据库
	- 数据库转换准备
		- 存在的数据库和目标数据库必须是同版本同平台
		- 硬件和OS必须是被认证的
		- 共享存储被配置给数据库
		- 检查应用程序，以便能跟RAC数据库配合使用
		- 转换之前保证backup程序必须是可用
		- 对于RAC归档环境，归档日志要求有thread number
		- RAC中所有实例的归档日志文件能从介质恢复
	- 使用Grid Control转换数据库
	- 使用rconfig转换数据库
	- 转换RAC数据库到RAC One Node数据库

####4 Administering Database Instances and Cluster Databases

	可用Enterprise Manager Database Control或者Grid Control管理RAC数据库
	本章主要是解释怎么开关数据库组件和管理RAC中的参数文件

* 关于Oracle RAC数据库管理
	- 可以增加更多节点到RAC环境来增加数据库集群的可用性(availability)和可靠性(reliability)
	- RAC数据库包括三部分： 结点、共享存储和Oracle Clusterware
	- RAC和单实例数据库在很多管理任务上是一样的
	
* 关于Oracle RAC One Node数据库管理
	- RAC One Node是个运行在RAC中一个节点上的单实例，可以在线重分配实例到另外一个节点上
	- 候选节点被监控着，确保对能被One Node数据库可以切换
	
* 关于使用Enterprise Manager管理Oracle RAC
	- Enterprise Management Grid Control可以管理整个RAC环境，而非仅仅RAC数据库

* 启动和停止Oracle RAC数据库和数据库实例
	- Enterprise Manager: http://hostname:portnumber/em
	- SQL*PLUS or SRVCTL

* 关于Oracle RAC Initialization参数
	跟单实例数据库参数基本上差不多，请注意以下的差异
		- RAC有特定于集群的参数
		- 每个实例一样的参数用 *. 标识
		- 每个实例上不一样的参数用实例名标识
	- 有关配置初始化参数的Oracle RAC数据库
		初始化参数文件(pfile)和服务器参数文件(spfile)
		- 所有实例必须相同的参数
			CLUSTER_DATABASE 
			COMPATIBLE 
			CONTROL_FILES 
			DB_BLOCK_SIZE 
			DB_DOMAIN 
			DB_FILES 
			DB_NAME 
			DB_RECOVERY_FILE_DEST 
			DB_RECOVERY_FILE_DEST_SIZE 
			DB_UNIQUE_NAME DML_LOCKS(0) 
			INSTANCE_TYPE(RDBMS/ASM) 
			PARALLEL_EXECUTION_MESSAGE_SIZE 
			REMOTE_LOGIN_PASSWORDFILE 
			RESULT_CACHE_MAX_SIZE 
			UNDO_MANAGEMENT
		- 所有实例必须唯一的参数
			CLUSTER_INTERCONNECTS 
			INSTANCE_NUMBER
			ROLLBACK_SEGMENTS(AUTO) 
			UNDO_TABLESPACE(AUTO)			
		- 所有实例应该相同的参数
			ARCHIVE_LAG_TARGET 
			CONTROL_MANAGEMENT_PACK_ACCESS DIAGNOSTIC_DEST
			LICENSE_MAX_USERS 
			LOG_ARCHIVE_FORMAT 
			REDO_TRANSPORT_USER
			SPFILE 
			TRACE_ENABLED UNDO_RETENTION
		- 关于SERVICE_NAMES参数
			- 指定客户端可以连接到该实例的一个或多个名称，实例对于监听器注册其服务
			- RAC数据库不应该直接修改此参数，可用EM或SRVCTL更改服务（自动生效)
	- 对于Oracle RAC数据库编辑初始化参数设置
		- EM "server" -> "Initialization Parameters"	
			- current Tab
			- spfile Tab
	- 有关Oracle真正应用集群服务器参数文件
		- 推荐使用spfile，存放在ASM DG或cluster文件系统
		- 可以通过RMAN备份spfile
	
* 关于Oracle RAC的存储管理
	- 关于Oracle RAC的UNDO管理
	...
	- Oracle RAC中的ASM
	...
	- 管理Oracle RAC的重做日志
	...

####5 Administering Oracle Clusterware Components

本章主要描述如何管理表决磁盘(Voting Disks)和Oracle集群注册表(OCR)

* 关于Oracle Clusterware

	Oracle RAC使用Oracle Clusterware作为基础设置结合多个节点那然后作为单个服务器进行操。 
	在Oracle RAC环境中， Oracle集群监控所有Oracle组件（如instance和listener）。
	如果发生故障，那么Oracle集群件自动尝试重新启动有故障的组件和重定向到正常的组件。

	Oracle集群包括用于管理群集上运行的任何应用程序的高可用性架构。 
	Oracle集群件还监视应用程序以确保它们始终可用。 
	- 关于表决磁盘
		
		表决磁盘记录节点成员的信息。
		一个节点必须能够在任何时候访问一半以上的表决磁盘。
		为避免多组表决磁盘的同时的损失，每个表决磁盘应该不要跟其它表决磁盘共享存储任何组件（控制器，互连等）。

		例如，如果你已经配置了五个表决磁盘，则节点必须能够在任何时候访问至少有三个表决磁盘。
		如果一个节点不能访问表决磁盘的所需的最低数目，那么它从集群中被evicted(逐出)或removed。
		该故障的原因已得到纠正，并获得了表决磁盘已恢复后，你可以指示Oracle集群恢复失败的节点，并将其恢复到集群。
	- 关于Oracle集群注册表

		Oracle集群注册表（OCR）是一个包含有关群集节点列表和实例到节点的映射信息的信息的文件。
		OCR还包含您已经自定义资源Oracle集群资源配置文件的信息。
		表决磁盘的数据也被OCR备份。

		集群中的每个节点也有OCR的本地副本，称为Oracle本地注册表（OLR），安装Oracle集群时创建。
		每个节点上的多个进程同时具有读取和写入它们所在的节点上的OLR，用来知道Oracle集群件是否功能齐全。
		默认情况下OLR - Grid_home/cdata/$HOSTNAME.olr
		
	- 关于Oracle集群文件的高可用性	
		
		高可用性配置通过避免单点故障以维持运营时的硬件和软件冗余。
		当一个组件发生故障，Oracle集群重定向其管理的资源到冗余组件。
		但是，如果灾难发生，或者大规模硬件发生故障时，则有冗余组件可能是不够的。
		为了充分保护您的系统拥有你的重要文件的备份是非常重要的。

		Oracle集群件安装过程中在共享存储创建表决磁盘和OCR。
		如果在安装过程中选择normal冗余副本的选项，然后Oracle集群自动维护这些文件的冗余副本，以防止成为单点故障的文件。
		normal冗余功能还省去了第三方存储冗余解决方案的需求。
		当您使用normal冗余，Oracle集群自动保持OCR文件的两个副本和表决磁盘文件的三个副本。
		
* 管理Oracle Clusterware Stack

* 管理Oracle集群的表决磁盘

* 备份和恢复的Oracle集群注册表

* 更改Oracle集群注册表配置
* Oracle集群注册表故障排除

####6 Administering Backup and Recovery
####7 Managing Database Workload Using Services
####8 Monitoring Performance and Troubleshooting 
####9 Adding and Deleting Nodes and Instances
####10 Managing Oracle Software and Applying Patches 

