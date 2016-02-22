# [2 Day + Data Warehousing Guide](http://docs.oracle.com/cd/E11882_01/server.112/e25555/toc.htm)

###简介:


Part I Building Your Data Warehouse

    1 Introduction to Data Warehousing
    
        本指南教你如何实施和管理数据仓库的日常任务
        数据仓库是一个专为查询和分析的关系型或多维数据库
            - 不进行事务处理，通常整合多个来源的历史和分析数据
            - 其数据从多个数据源ETL(Extraction,transformation,Loading)加载
            - 一般分析跟时间相关的数据，负责的包括趋势分析和数据挖掘
        主要特点
            - 数据规格化是为了简单和提供性能
            - 使用大量的历史数据
            - 查询中常常检索大量数据
            - 常用为计划查询和即时(ad hoc)查询
            - 数据的负载是被控制
        常用数据库参考任务
            - 配置O​​racle数据库用作数据仓库
            - 设计数据仓库
            - 执行数据库和数据仓库软件新版本的升级
            - 管理模式对象，如表，索引和物化视图
            - 管理用户和安全性
            - 开发用于提取，转换例程，和加载（ETL）过程的规则
            - 创建基于数据仓库中的数据报告
            - 备份数据仓库和所需时执行恢复
            - 监测数据仓库的性能和基于要求采取预防或纠正措施
        管理数据仓库的个噢那句
            - OUI
            - OEM
            - Oracle Warehouse Builder
            - Oracle Tuning Pack
    
    2 Setting Up Your Data Warehouse System
    3 Identifying Data Sources and Importing Metadata
    4 Defining Warehouses in Oracle Warehouse Builder
    
Part II Loading Data into Your Data Warehouse

    5 Defining ETL Logic
    6 Deploying to Target Schemas and Executing ETL Logic
    
Part III Reporting on a Data Warehouse

    7 SQL for Reporting and Analysis
    
Part IV Managing a Data Warehouse

    8 Refreshing a Data Warehouse
    9 Optimizing Data Warehouse Operations
    10 Eliminating Performance Bottlenecks
    11 Backing up and Recovering a Data Warehouse
    12 Securing a Data Warehouse