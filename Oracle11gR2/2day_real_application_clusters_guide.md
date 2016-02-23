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
		- 验证对应用程序，以便能跟RAC数据库配合使用
		- 转换之前backup程序必须是可用哦的
		- 对于RAC归档环境，归档日志要求有thread number
		- RAC中所有实例的归档日志文件能从介质恢复
	- 使用Grid Control转换数据库
	- 使用rconfig转换数据库
	- 转换RAC数据库到RAC One Node数据库

####4 Administering Database Instances and Cluster Databases
####5 Administering Oracle Clusterware Components
####6 Administering Backup and Recovery
####7 Managing Database Workload Using Services
####8 Monitoring Performance and Troubleshooting 
####9 Adding and Deleting Nodes and Instances
####10 Managing Oracle Software and Applying Patches 

