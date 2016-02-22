# [2 Day + Real Application Clusters Guide](http://docs.oracle.com/cd/E11882_01/rac.112/e17264/toc.htm)

###简介:
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
        - 
* 关于Oracle RAC
        - RAC扩展Oracle数据库，以至于你可以在不同服务器上同时进行存储、更新和有效查询
        - 构成数据库的数据文件必须存放在集群里所有的服务器都可以访问的共享存储(多实例对应1个数据库)
        - Oracle RAC数据库每个数据库实例使用它自己的内存结构和后台进程
        - RAC使用Cache Fusion同步（传输）存放在不同实例的buffer cache中的数据
        - Oracle RAC one Node:
            - 单实例节点集群，使其免受计划内或计划外停机
            - 当One Node数据库超载，可以对应用用户无影响情况下在线升级到cluster
        - RAC不支持一个集群跑在不同平台上，但允许同一平台下有不同型号的机器      
* 安装、配置、管理RAC的工具        
        - OUI 
        - CVU: Cluster Verification Utility
            验证一系列集群和RAC组建（共享存储、网络配置、系统需求、Clusterware、操作系统的组和用户）的命令行工具
            可以在安装前和安装后检查RAC集群环境 
        - OEM
        - SQL*Plus
        - SRVCTL: Server Control
            管理在集群注册表(OCR)定义的资源(nodeapps, Oracle Clusterware)
            Clusterware包括ONS(Oracle Notification Service), GSD(Global Service Daemon), VIP(Virutal IP)
            其他资源包括启动和停止数据库、实例、监听器、服务，删除和移动实例和服务，添加服务和管理配置信息
        - CRSCTL: Cluster Ready Services Control 
            管理Oracle集群守护进程(CSS, CRS, EVM)的命令行工具
            Cluster Synchronization Services, Cluster-Ready Services, Event Manager
            可用来启动和停止Clusterware，并可以确定集群安装的状态
        - DBCA
        - ASMCA
        - ASMCMD    
        - LSNRCTL
        
        
####2 Preparing Your Cluster

####3 Installing Oracle Grid Infrastructure and Oracle Real Application Clusters
####4 Administering Database Instances and Cluster Databases
####5 Administering Oracle Clusterware Components
####6 Administering Backup and Recovery
####7 Managing Database Workload Using Services
####8 Monitoring Performance and Troubleshooting 
####9 Adding and Deleting Nodes and Instances
####10 Managing Oracle Software and Applying Patches 

