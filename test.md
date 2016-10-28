# 项目经验 

### 2015/12-2016/6	生产中间件应用从VMWare迁移到Docker

* 项目描述：	将服务生产的中间件应用（JAVA程序 - 88台虚拟机 CentOS6.7）从VMWare平台迁移到Docker平台，以容器的方式实现中间件应用
* 责任描述：	前期的Docker技术研究，中间件应用测试环境搭建，及镜像制作等技术探索。协助系统管理员安装和部署生产环境

  * Project: 		Dockerizing production middleware service
  * Description:	Migrate product middleware service (Java program, total 88 virtual machine) from VMWare platform to Docker platform
  * Responsibility: Docker technical research and build midlleware application test environments and build the image. Assist system administrators in installing and deploying it in production 

### 2015/11-2016/4	核心生产Oracle数据库平台迁移（AIX-Linux）

* 项目描述：	将生产核心数据库（共12套Oracle，其中4套Dataguard环境）从AIX(IBM power+SAN)平台迁移到OEL7(X86+DAS)
* 责任描述：	数据库数据测试迁移，生产数据库和应用安装和迁移

  * Project:		Core production Oracle database platform migration(AIX to Linux)
  * Description:	Migrate production core database(Total 12 pairs, including 4 Dataguard) from AIX platform(IBM+SAN) to X86 platform(X86+DAS)
  * Responsibility: Do database and application testing before migration, install and deploy it on production environment.

### 2015/7-2016/1   监控Zabbix部署

* 项目描述：	部署Zabbix替换现有的监控工具(HP OpenView, Cacti, Nagios)以节省成本和高效监控
* 责任描述：	针对于数据库及后台应用开发通用接口，数据库端(Orabbix, pg_monz)及后端应用监控部署

  * Project:			Zabbix monitor deployment
  * Description:		Deploy Zabbix to replace existing monitoring tools(HP OpenView, Cacit, Nagios) for cost saving and efficient monitoring
  * Responsibility: 	Develop common interface for internal application monitoring, and deploy monitor for backend application and database(Orabbix, pg_monz, etc.)

### 2014/10-2016/7  通过业务连续性管理(ISO 22301)认证 

* 项目描述：	通过公司亚太地区工厂BCM ISO 22301认证
* 责任描述：	协调苏州工厂IT部门BCM工作（业务影响分析和风险评估），并负责数据库相关的文档编写

  * Project:			Get Business Continuity Management (ISO 22301) certification
  * Description:		Get BCM ISO 22301 certification for all Asia Pacific facilities
  * Responsibility: 	Coordinate IT BCM work(business impact analysis and risk assessment) of Suzhou Factory and prepare related documents for database service

### 2014/7-2015/3	Oralce生产实时报表分析数据库系统升级

* 项目描述：	基于业务需求将现有的一套生产实时报表分析提供存储容量从10TB扩容到40TB，同时要求不是备用数据库以达到灾备需求
* 责任描述：	结合开发人员和系统管理员决策升级方案和实施计划，负责数据库备份(RMAN+EXPDP)确保数据库存储扩容，及负责备用数据库的安装以及主从数据库切换

  * Project:			Production online reporting analysis database upgrade 
  * Description:		Expand the real-time report analysis database(Oracle) storage from 10TB to 40TB for business requirement, and build shadow database to meet DR(disaster recovery) target
  * Responsibility:	Achieve upgrade solution and implementation plan with developer and system administrator, and responsible for database backup(rman+expdp) for database storage expansion, and build shadow system and complete database service switchover


### 2013/11-2014/2	MySQL数据库整合

* 项目描述：	鉴于IT现有30个MySQL数据库(非核心生产应用、非生产Web应用），分别跑在各自的应用服务器上，数据没有冗余和备份。部门决定将所有MySQL整合成一套MySQL cluster(master-slave)+日常备份来提高其数据库服务高可用性
* 责任描述：	根据需求安装和实施MySQL cluster(Binlog replication)&备份, 并创建其数据库和账号，确保应用无故障迁移

  * Project:			MySQL database consolidation
  * Description:		Base on 30 MySQL databases for non-critical production application and non-production web application running on their respective app servers, it plan to consolidate all MySQL into one pair MySQL cluster and daily backup to improve its high availability
  * Responsibility:	Install and implement MySQL cluster, migrate the database and deploy backup as required 

### 2013/4-2013/9	PostgreSQL数据库系统虚拟化

* 项目描述：	中间件PostgreSQL数据库系统从物理机迁移到虚拟机(VMWare+Netapp)
* 责任描述：	负责其应用迁移及PostgreSQL数据库升级从8.4到9.1(Pgdump)

  * Project:			Virtualize PostgreSQL database
  * Description:		Migrate middleware PostgreSQL database from physical machine to virtual machien(VMWare+Netapp)
  * Responsibility:	Upgrade PostgreSQL database from 8.4 to 9.1(pgdum) with reinstallation

### 2012/4-2012/8	数据备份改善

* 项目描述：	基于部门灾难备份策略，对所有ＩＴ服务器数据（包括操作系统，应用，数据库配置，数据库数据）进行改善
* 责任描述：	应用Shell(KSH) 和Expect(TCL)开发出自动远程备份工具，从而实现50台服务器数据周/月备份的自动化

	* Project:			Database backup improvement
	* Description:		Base departmental backup strategy achieve IT data(including OS, app, database configuration, database data) backup efficiently
	* Responsibility: 	Develop backup tool with ksh and expect to achieve automatic backup for 50 servers' data

### 2012/3-2012/10	核心Oralce数据库系统存储升级

* 项目描述：	升级生产2台核心数据库存储系统从EMC DMAX-3到EMC VMAX
* 责任描述：	参与存储升级方案制定，及负责Oracle数据库及应用方面的迁移

  * Project:			Production core database storage upgrade
  * Description:		Upgrade production database(Oracle) storage system from EMC DMX-3 to EMC VMAX
  * Responsibility:	Participate in to achieve storage upgrade plan and reponsible for database and application migration 

### 2011/10-2012/6	HP OpenView监控工具实施

* 项目描述：	核心生产数据库及应用监控由BMC Patrol迁移到HP OpenView。部署nagios监控非核心应用及中间件服务器
* 责任描述：	使用Perl开发HP Openview接口来监控公司内部应用。安装Nagios(Nagios Core,PNP4nagios, NDOUtils, Nconf)平台来监控中间件服务器及非核心应用

	* Project:			HP Openview monitor tools deployment
	* Description:		Migrate core database and application monitor from BMC Patrol to HP Openview
	* Responsibility:	Develop common interface with ksh and perl for internal application, and participate in to achieve monitor policy implementation

### 2011/8-2012/5	生产核心Oralce数据库系统从SUN迁移到IBM平台

* 项目描述：	所有生产核心Oracle数据库系统从SUN E20K(Solaris SPARC平台)迁移到IBM P770(AIX Power平台)
* 责任描述：	负责Oracle数据库及应用方面的迁移

	* Project:			Production core Oracle database system migration from SUN to IBM platform
	* Description:		Migrate all production core system from SUN E20k(Solaris SPARC platform) to IBM P770(AIB Power platform)
	* Responsibility:	migrate oracle database and application 

### 2010/9-2011/9	核心生产Oracle数据库系统从9i设计到10g

* 项目描述：	完成所有Oracle数据库平稳升级，并在升级后对应用及数据库进行优化以确保业务不受影响
* 责任描述：	oracle升级测试和oracle升级，对应用和数据库进行优化

	* Project:			Production core Oracle database upgrade(9i-10g)
	* Description:		Develop upgrade plan and solution for different hardware and application, and ensure 
	* Responsibility:	Achieve upgrade solution

### 2009/5-2010/9	Green IT实施    

* 项目描述：	金融危机背景下，公司提出节约成本。IT部门提倡绿色IT，鼓励员工下班后关掉电脑、显示器等用电设备；配合设施部测试机房温湿度并改善机房环境来达到节省开销；按CMMI标准对数据中心机房维护及改善(Airflow, Environmental, Power)
* 责任描述：	每天扫描下班后的活动主机以及报告给老板；每个季度测试机房Power的输入输出功率，分析PUE结果对UPS、空调或其他用电设备进行改善，提高机房设备运行的安全性

	* Project:
	* Description:
	* Responsibility:

### 2008/9-2008/11	PL/SQL Script标准脚本规范制定和模板编写

* 项目描述：	由于其他部门需求，每天需要编写脚本从生产报表数据库里检索信息提供给他们。根据不同的客户需求，制定，规范，编写和优化通用PL/SQL脚本给其他团队成员，使其能在ORACLE数据库系统里检索数据，生成报表给用户(PROCESS, QA, FA, PRODUCT)
* 责任描述：	通过与用户的交流，日常工作的积累和对业务及数据库中存储过程的熟悉，来制定、规范、编写通用脚本给整个支持团队，并对之进行优化来提高数据报表的高效性
