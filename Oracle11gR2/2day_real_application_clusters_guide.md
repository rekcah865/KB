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
	- 自11gR2，ASM增加了对Oracle OCR(集群注册表)和Voting disks，被称为Oracle ACFS
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
			- CLUSTER_DATABASE 
			- COMPATIBLE 
			- CONTROL_FILES 
			- DB_BLOCK_SIZE 
			- DB_DOMAIN 
			- DB_FILES 
			- DB_NAME 
			- DB_RECOVERY_FILE_DEST 
			- DB_RECOVERY_FILE_DEST_SIZE 
			- DB_UNIQUE_NAME DML_LOCKS(0) 
			- INSTANCE_TYPE(RDBMS/ASM) 
			- PARALLEL_EXECUTION_MESSAGE_SIZE 
			- REMOTE_LOGIN_PASSWORDFILE 
			- RESULT_CACHE_MAX_SIZE 
			- UNDO_MANAGEMENT
		- 所有实例必须唯一的参数
			- CLUSTER_INTERCONNECTS 
			- INSTANCE_NUMBER
			- ROLLBACK_SEGMENTS(AUTO) 
			- UNDO_TABLESPACE(AUTO)			
		- 所有实例应该相同的参数
			- ARCHIVE_LAG_TARGET 
			- CONTROL_MANAGEMENT_PACK_ACCESS DIAGNOSTIC_DEST
			- LICENSE_MAX_USERS 
			- LOG_ARCHIVE_FORMAT 
			- REDO_TRANSPORT_USER
			- SPFILE 
			- TRACE_ENABLED UNDO_RETENTION
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

本章主要描述如何管理Voting Disks和Oracle集群注册表(OCR)

* 关于Oracle Clusterware

	Oracle RAC使用Oracle Clusterware作为基础设置结合多个节点那然后作为单个服务器进行操。 
	在Oracle RAC环境中， Oracle集群监控所有Oracle组件（如instance和listener）。
	如果发生故障，那么Oracle集群件自动尝试重新启动有故障的组件和重定向到正常的组件。

	Oracle集群包括用于管理群集上运行的任何应用程序的高可用性架构。 
	Oracle集群件还监视应用程序以确保它们始终可用。 
	- 关于voting disks
		
		voting disks记录节点成员的信息。
		一个节点必须能够在任何时候访问一半以上的voting disks。
		为避免多组voting disks的同时的损失，每个voting disks应该不要跟其它voting disks共享存储任何组件（控制器，互连等）。

		例如，如果你已经配置了五个voting disks，则节点必须能够在任何时候访问至少有三个voting disks。
		如果一个节点不能访问voting disks的所需的最低数目，那么它从集群中被evicted(逐出)或removed。
		该故障的原因已得到纠正，并获得了voting disks已恢复后，你可以指示Oracle集群恢复失败的节点，并将其恢复到集群。
	- 关于Oracle集群注册表

		Oracle集群注册表（OCR）是一个包含有关群集节点列表和实例到节点的映射信息的信息的文件。
		OCR还包含您已经自定义资源Oracle集群资源配置文件的信息。
		voting disks的数据也被OCR备份。

		集群中的每个节点也有OCR的本地副本，称为Oracle本地注册表（OLR），安装Oracle集群时创建。
		每个节点上的多个进程同时具有读取和写入它们所在的节点上的OLR，用来知道Oracle集群件是否功能齐全。
		默认情况下OLR - Grid_home/cdata/$HOSTNAME.olr
		
	- 关于Oracle集群文件的高可用性	
		
		高可用性配置通过避免单点故障以维持运营时的硬件和软件冗余。
		当一个组件发生故障，Oracle集群重定向其管理的资源到冗余组件。
		但是，如果灾难发生，或者大规模硬件发生故障时，则有冗余组件可能是不够的。
		为了充分保护您的系统拥有你的重要文件的备份是非常重要的。

		Oracle集群件安装过程中在共享存储创建voting disks和OCR。
		如果在安装过程中选择normal冗余副本的选项，然后Oracle集群自动维护这些文件的冗余副本，以防止成为单点故障的文件。
		normal冗余功能还省去了第三方存储冗余解决方案的需求。
		当您使用normal冗余，Oracle集群自动保持OCR文件的两个副本和voting disks文件的三个副本。
		
* 管理Oracle Clusterware Stack

	默认情况下，Oracle集群服务器被配置restart。
	但在一定的维护操作是，您可能会被要求停止或手动启动Oracle Clusterware Stack(集群堆栈)。

	- 启动Oracle集群件
	
		您可以使用CRSCTL工具来管理Oracle集群。
		如果Oracle高可用性服务守护程序（OHASD）是所有群集节点上运行，那么你可以通过执行以下命令来启动整个Oracle集群堆栈（全部由Oracle集群件管理的流程和资源），在所有节点上的集群中任何节点上执行如下命令：
		```
		crsctl start cluster -all
		```
		您可以通过使用-n选项(节点名称的空格分隔列表)，例如在启动特定节点Oracle集群堆栈(确保OHASD进程是运行的)：
		```
		crsctl start cluster -n racnode1 racnode4
		```
		要在一个节点上启动整个Oracle集群堆叠，包括OHASD过程中，该节点上运行以下命令：
		```
		crsctl start crs
		```
	- 停止Oracle集群
	
		要停止集群中的所有节点上的Oracle集群，任何节点上执行以下命令：
		```
		crsctl stop cluster -all
		```
		上面的命令停止由Oracle集群件/Oracle ASM实例管理的资源及所有的Oracle集群进程（除了OHASD及其相关进程）。

		要停止Oracle Clusterware和Oracle ASM上选择的节点，包括-n选项(后面跟节点名称的空格分隔列表)，例如：
		```
		crsctl stop cluster -n racnode1 racnode3
		```
		如果不包括-all或-n选项，则Oracle集群件和其管理的资源只在您执行命令的节点上停下来。

		要完全关闭整个Oracle集群堆栈，包括OHASD进程，使用*crsctl stop crs*命令。 
		CRSCTL试图在Oracle Clusterware Stack的停机期间正常有序的停止Oracle集群管理的资源。
		如果Oracle集群件管理的任何资源执行*crsctl stop crs*命令后仍在运行，则该命令将失败。
		然后，您必须使用-f选项无条件地停止所有资源并停止Oracle Clusterware Stack，例如：
		```
		crsctl stop crs -f
		```

* 管理Oracle集群的voting disks

	- 添加和删除voting disks
		
		如果您选择在Oracle ASM(并使用冗余磁盘组)存储Oracle集群文件，
		则Oracle ASM基于磁盘组的冗余数目自动维护voting disks的数目。

		如果您使用不同形式的共享存储来存储voting disks，那么你就可以在安装Oracle RAC后动态地添加和删除voting disks。
		为此可以使用下面的命令，其中path是附加的voting disks的完全qualified路径。
		```
		crsctl add css votedisk path
		
		crsctl delete css votedisk path
		```
	
	- 备份和恢复voting disks
	
		- Backing Up Voting Disks
		
			在Oracle Clusterware 11.2，你不再需要备份voting disks。
			voting disks数据自动备份作为OCR任何配置更改的一部分。
			如果文件的内容在以下几个方面发生了变化是Oracle Clusterware降自动备份voting disks文件：
				- Configuration parameters， 例如miscount被增加或修改
				- 在执行voting disks add或者delete操作
				
		- Replacing Voting Disks
		
			如果表决磁盘损坏，不需要再使用Oracle集群件，那么你就可以更换或重新创建表决磁盘。
			您可以通过删除无用表决磁盘，然后添加新的表决磁盘配置更换表决磁盘。
			不管表决磁盘文件是存储在ASM，表决磁盘内容从备份恢复作为新增加的表决文件。
			```
			crsctl delete css votedisk /dev/sda3
			
			crsctl add css votedisk /dev/sda3
			```
		- Restoring Voting Disks
		
			如果所有表决磁盘损坏，你可以通过参考[Oracle Clusterware Administration and Deployment Guide](http://docs.oracle.com/cd/E11882_01/rac.112/e41959/votocr.htm#CWADD90961)进行回复
			
	- 迁移voting disks到Oracle ASM存储
		...
	
* 备份和恢复的Oracle集群注册表

	Oracle集群每四个小时自动创建备份OCR。
	在任何时候，Oracle集群始终保留最近的三个OCR备份副本(四小时前，一天前，一周前）。
	
	您不能自定义备份的频率或Oracle集群保留的文件数量。
	您可以使用任何备份软件，至少每天一次从其中主OCR文件复制并自动生成备份文件到不同的设备。
	
	- 查看可用的备份OCR
		```
		ocrconfig -showbackup
		```
	- 手动备份OCR
		使用ocrconfig实用程序来强制Oracle集群随时执行OCR的备份，而不是等到发生在四小时的时间间隔自动备份。
		您可以基于需求更改OCR前的二进制备份，此选项是非常有用的。
		```
		# ocrconfig -manualbackup
		# ocrconfig -backuploc <directory_name?
		```
		默认Linux系统备份的目录 Grid_home/cdata/cluster_name。
		因为默认的备份是在本地文件系统上，Oracle建议您包括与ocrconfig命令使用标准操作系统或第三方工具操作系统备份的一部分创建的备份文件。
				
	- 恢复OCR
		有两种方法用于恢复OCR。第一种方法使用自动生成的OCR文件副本和第二种方法使用手动创建OCR导出文件。
		- 检查OCR的状态
		```
		ocrcheck
		```
		- 利用自动生成的OCR备份恢复OCR
		```
		# ocrconfig -showbackup
		# ocrdump ocr_dump_output_file -backupfile file_name
		# crsctl stop cluster -all
		# ocrconfig -restore file_name
		# crsctl start cluster -all
		
		# cluvfy comp ocr -n all [-verbose]
		```
		
* 更改Oracle集群注册表配置
	
	本节将介绍如何管理OCR。
	OCR包含有关已修改为Oracle集群管理的群集节点列表，其中实例哪个节点上运行，以及有关Oracle集群资源配置文件应用程序的信息
	
	- 添加一个OCR位置
	- 迁移OCR到Oracle ASM存储
	- 更换OCR
	- 移除OCR
	- 在本地节点上修复OCR配置
	...
	
* Oracle集群注册表故障排除

	- 关于OCRCHECK工具
	...
	- 常见的Oracle集群注册表的问题与对策
	...
	
####6 Administering Backup and Recovery

为了防止数据库的数据丢失和数据丢失后重建数据库，则必须设计，实施和管理备份和恢复策略。
本章介绍了如何备份和恢复的Oracle Real Application Clusters（Oracle RAC的）数据库。

* Oracle RAC数据库备份和恢复概述

	为了保护您的OracleRAC数据库（来自硬件故障或灾难），必须有数据库文件的物理副本。
	通过Oracle EM中的备份和恢复设施保护的文件包括数据文件，控制文件，服务器参数文件（SPFILE）和归档重做日志文件。
	使用这些文件，你的数据库可以被重建。
	这物理层工作的备份机制可以防止文件级别损坏，比如数据文件意外删除或磁盘驱动器的故障。
	Database recovery包括对损坏的文件从备份中还原，或复制，和从恢复的文件执行介质恢复。
	Media recovery是重做日志或增量备份的一个应用程序，用来恢复数据文件到当前时间或其他指定的时间。
	
	Oracle数据库flashback功能，如Oracle Flashback Drop和Oracle Flashback Table，
	提供一系列高效和易于使用的物理和逻辑数据恢复工具，从而作为一个可选物理和逻辑备份操作。
	闪回功能，使您能够回归到不想要的数据库变化，而不需要从备份恢复数据文件或执行介质恢复。
	
	EM的物理备份和恢复特性都是基于恢复管理器（RMAN）命令行客户端。
	企业管理器来使可用的RMAN功能，并提供了向导和自动的策略，以简化和进一步自动化基于RMAN的备份和恢复。
	
	EM引导式恢复能力提供了一个封装了广泛的文件还原和恢复方案，包括以下所需的逻辑恢复向导：
		- Complete restoration and recovery of the database
		- Point-in-time recovery of the database or selected tablespaces
		- Flashback Database
		- Other flashback features of Oracle Database for logical-level repair of unwanted changes to database objects
		- Media recovery at the block level for data files with corrupt blocks
	
* 关于Oracle RAC的Fast Recovery Area

	使用快速恢复区最大限度地减少了手动管理磁盘空间（需要为您的备份相关的文件和平衡不同类型的文件的空间管理）。
	Oracle建议您启用快速恢复区以简化您的备份管理。
	
	快速恢复面积越大，用处就越大。
	理想情况下，快速恢复面积应足够大，以包含所有以下文件：
	
		- A copy of all data files
		- Incremental backups
		- Online redo logs
		- Archived redo log files that have not yet been backed up
		- Control files and control file copies
		- Autobackups of the control file and database initialization parameter file

	对于Oracle RAC数据库快速恢复区必须放在一个Oracle ASM磁盘组，群集文件系统，或者是通过每个Oracle RAC实例的网络文件系统文件中配置的共享目录。
	换句话说，快速恢复区必须在所有的Oracle RAC数据库的实例的共享。
	为Oracle RAC的首选配置是使用Oracle ASM，用于存储快速恢复区，使用不同的磁盘组的恢复集比数据文件。

	在所有实例中它的位置和磁盘配额必须是相同的。 
	Oracle建议您将快速恢复区的共享的Oracle ASM磁盘。
	此外，还必须设置DB_RECOVERY_FILE_DEST和DB_RECOVERY_FILE_DEST_SIZE参数上的所有实例相同的值。

	要使用快速恢复功能，您必须先配置快速恢复区在Oracle RAC数据库每个实例。

* 归档Oracle RAC重做日志

* 关于备份和恢复操作准备

* 执行Oracle RAC数据库的备份

* 关于准备还原和恢复Oracle RAC数据库

* 恢复您的Oracle RAC数据库

* 有关管理数据库备份文件

* 显示Oracle RAC数据库备份报告

####7 Managing Database Workload Using Services

使用workload management你可以跨数据库实例分配工作量，以实现用户和应用的最佳数据库和集群的性能。

* 关于工作负载管理

	使用集群数据库应用程序通常要在集群平衡他们的工作量。 
	Oracle RAC包括高可用性（HA）的应用程序框架，它提供的Oracle RAC和定制的企业应用程序之间的必要的服务和集成点。

	你可以使用许多不同的方法的工作负荷管理功能在Oracle RAC和单实例Oracle数据库环境中部署。
	根据节点的数量和环境的复杂性及你的目标，最佳工作负载管理和高可用性配置选择取决于多种因素，如本章所述。

	要实现一个Oracle RAC数据库工作负载管理，可以使用许多不同的特点。
	- About Oracle Services  
		Oracle数据库10g引入了自动工作负载管理工具，名为数据库服务。
		数据库服务（services）是Oracle数据库工作负载管理逻辑抽象。
		服务工作负载划分成互不相交的分组。
		每个服务代表了公共属性，服务水平阈值和优先级工作负载。

		单个服务可以代表应用程序，多个应用程序或单个应用程序的一个子集。
		例如，Oracle E-Business suite为每个responsibility定义了一个服务，如总帐，应收账款，订单输入，等等。
		单个服务可以与Oracle RAC数据库的一个或多个实例相关联，而且一个单一的实例可以支持多个服务。
		
		服务提供以下好处：
		
			- 管理对于相同的资源竞争的应用程序可作为一个单一实体
			- 允许每个工作负荷被管理作为一个单元
			- 从客户端来看可以隐藏集群的复杂性
		要管理工作负载，您可以定义分配给特定的应用程序或应用程序的操作的子集服务。
		您还可以使用服务来管理不同类型的工作负载。
		例如，在线用户可以使用一个服务，而批处理可以使用不同的服务和报告可以使用另一种服务类型。

		传统上Oracle数据库提供一个单一的服务并连接到相同的服务的所有用户。
		数据库总是有这种默认的数据库服务，它是数据库名。
		此服务不能被修改，总是可以连接到数据库。
		
		当用户或应用程序连接到数据库，Oracle建议您使用服务进行连接。
		在创建数据库时，Oracle数据库会自动创建一个数据库服务。
		对于许多安装，这可能是你所需要的。
		对于使用数据库工作负载的管理更灵活的Oracle数据库，您可以创建多项服务，并指定数据库实例提供的服务。
		
		您可以为policy-managed和 administrator-managed数据库中定义的服务。
		
			- Policy-managed数据库：当您定义服务时，在正在运行的数据库中您可以指定一个服务器池的服务。您可以定义为既统一（在服务器池中的所有实例上运行）或单个（在服务器池中只运行一个实例）的服务。
			- Administrator-managed数据库：当您定义服务时，您可以定义哪些实例通常支持哪些service.这些被称为PREFERRED实例。如果preferred(首选)实例无法支持服务您也可以定义其他实例。这些被称为AVAILABLE实例。

		服务是集成了数据库资源管理器，它使您能够限制在一个实例中使用一个服务的资源。
		此外，Oracle Scheduler jobs可以作为服务运行，而不是使用特定的实例。
		
		- Administrator-Managed数据库的Service Failover
			当您为服务指定首选(preferred)实例，服务在正常操作过程中的实例运行。 
			Oracle集群试图确保服务始终运行在所有已配置的服务的首选实例。
			如果实例失败，则该服务被重新定位到一个可用的实例。您也可以手动重新定位服务可用的实例。

			如果服务因故障切换到可用实例，则该服务不会自动移回其首选实例。
			但是，您可以通过使用callout自动将服务搬迁到其首选实例。
			有关callouts的详细信息，请参阅["About FAN Callouts"](http://docs.oracle.com/cd/E11882_01/rac.112/e17264/configwlm.htm#CACCBDFA)。
			[首选实例搬迁服务的一个例子 callout脚本](http://www.oracle.com/technetwork/database/enterprise-edition/twpracwkldmgmt-132994.pdf)
		
			你不必为一个服务指定可用实例。
			如果您在创建服务，则默认情况下不指定首选或可用实例，在Oracle RAC数据库每个实例是该服务的首选实例。
			但是，如果您配置首选实例服务，但不为服务指定至少一个可用实例，如果首选实例失则服务不败移居到其他实例。

			您也可以指定一个实例作为Not Used。此设置意味着，即使该服务的首选实例失败,服务也不会在该实例上运行，。

		- 关于Policy-Managed数据库的Service Failover

			当您指定一个UNIFORM服务，Oracle集群试图确保该服务始终运行在指定服务器池中的所有可用实例。
			如果实例失败，则该服务是在该实例上不再可用。
			如果服务器池中的基数增加和一个实例的被添加到数据库中，则该服务开始启动新实例。
			您不能手动重新定位服务 指定特定的实例。

			当您指定一个SINGLETON服务，Oracle集群试图确保该服务始终运行在仅指定服务器池中的可用实例之一。
			如果实例失败，则该服务切换到服务器池中的不同实例。
			您不能指定服务应运行在服务器池中的哪个实例。
			
			对于SINGLETON服务，如果服务故障转移到一个新的实例，即使原实例可用了该服务是不会切回到原来的实例。
		
		- 关于服务的自动启动
			
			当你定义一个服务，你还可以定义该服务的管理政策。
			- 自动：在数据库启动时总是启动该服务。
			- 手动：需要你在数据库启动后，手动启动该服务。
			在Oracle数据库11gR2之前的版本，所有服务的被定义为手工管理策略。

	- About the Database Resource Manager
	
		​​你可以用数据库资源管理器来控制分配给用户，应用程序和服务的数据库资源。
		这种方法可以确保用户，应用程序和服务接收者共享现有的数据库资源。
		数据库资源管理器，在一个或多个节点上运行，使Oracle RAC数据库最佳效率的支持多种应用程序和混合工作负载。
		
		数据库资源管理器为Oracle数据库或RAC环境中提供了工作优先级的能力。
		资源基于由数据库管理员所指定的资源计划分配给用户。
		下列术语在指定资源计划用于：
		
			- resource plan
			- Resource consumer groups
			- Resource allocation methods
			- Resource plan directives
			- Subplans
			- Levels
			
		数据库资源管理器让您可以将资源使用者组映射到一个服务，让连接使用该服务的用户是指定的资源消费群的成员，因而可以限制resource consumer group的可用资源。

		
	- About Oracle RAC High Availability Framework
	
		Oracle RAC的高可用性架构使Oracle RAC维护数据库，组件和应用程序始终处于运行状态。
		如果一个实例，组件或应用程序出现故障，那么它可以自动重新启动，以保持Oracle数据库满能力运行。

		Oracle数据库侧重于保持服务的可用性。
		在Oracle RAC，Oracle服务设计为跨越一个或多个实例的共享工作负载持续可用。
		在Oracle RAC的高可用性架构通过存储在Oracle集群注册表（OCR）每个服务的配置信息保持服务的可用性。 Oracle集群根据服务定义跨实例恢复和balance服务。

	- About Fast Application Notification (FAN)
	
		
	- About FAN Callouts
	
	- About the Load Balancing Advisory
	
	- About Connection Load Balancing
	
	- About Run-time Connection Load Balancing
	
* 创建服务

* 管理服务

* 配置客户端高可用性

####8 Monitoring Performance and Troubleshooting 
####9 Adding and Deleting Nodes and Instances
####10 Managing Oracle Software and Applying Patches 

