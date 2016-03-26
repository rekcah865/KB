# [Database Backup and Recovery User's Guide](http://docs.oracle.com/cd/E11882_01/backup.112/e10642/toc.htm)

### Part I Overview of Backup and Recovery
#### 1 Introduction to Backup and Recovery

* Purpose of Backup and Recovery

* Oracle Backup and Recovery Solutions

* Oracle Flashback Technology

* Data Recovery Advisor

* Backup and Recovery Documentation Roadmap
	
#### 2 Getting Started with RMAN

* Overview of the RMAN Environment

	- A target database
	- The RMAN client
	- A fast recovery area
	- A media manager
	- A recovery catalog
	
* Starting RMAN and Connecting to a Database
	
	```
	RMAN 
	[ TARGET connectStringSpec 
	| { CATALOG connectStringSpec } 
	| LOG ['] filename ['] [ APPEND ]
	.
	.
	. 
	]...

	connectStringSpec::=
	['] [userid] [/ [password]] [@net_service_name] [']
	```
	
* Showing the Default RMAN Configuration
	
	```
	RMAN> SHOW ALL;
	```
	
* Backing Up a Database

	- Backing Up a Database in ARCHIVELOG Mode
	
	- Backing Up a Database in NOARCHIVELOG Mode
	
		To make a consistent database backup:
		```
		RMAN> SHUTDOWN IMMEDIATE;
		RMAN> STARTUP FORCE DBA;
		RMAN> SHUTDOWN IMMEDIATE;
		RMAN> STARTUP MOUNT;

		RMAN> BACKUP DATABASE;

		RMAN> BACKUP AS COPY DATABASE;
		
		RMAN> ALTER DATABASE OPEN;
		```
	- Typical Backup Options
	
		- FORMAT
		- TAG
		
	- Making Incremental Backups
	- Making Incrementally Updated Backups
	- Validating Database Files and Backups
	- Scripting RMAN Operations
	
* Reporting on RMAN Operations

	- Listing Backups
	- Reporting on Database Files and Backups
	
* Maintaining RMAN Backups

	- Cross-checking Backups
	- Deleting Obsolete Backups
	

* Diagnosing and Repairing Failures with Data Recovery Advisor

	- Listing Failures and Determining Repair Options
	- Repairing Failures
	
* Rewinding a Database with Flashback Database

* Restoring and Recovering Database Files
	
	- Preparing to Restore and Recover Database Files
	- Recovering the Whole Database
	- Recovering Tablespaces
	- Recovering Individual Data Blocks
	
### Part II Starting and Configuring RMAN and Flashback Database

#### 3 Recovery Manager Architecture

* About the RMAN Environment

* RMAN Command-Line Client

* RMAN Channels

* RMAN Repository

* Media Management

* Fast Recovery Area

* RMAN in a Data Guard Environment

#### 4 Starting and Interacting with the RMAN Client

* Starting and Exiting RMAN

* Specifying the Location of RMAN Output

* Setting Globalization Support Environment Variables for RMAN

* Entering RMAN Commands
	
	- Entering RMAN Commands at the RMAN Prompt
	
	- Using Command Files with RMAN
	
	- Entering Comments in RMAN Command Files
	
	- Using Substitution Variables in Command Files
	
	- Checking RMAN Syntax

* Making Database Connections with RMAN

	- About RMAN Database Connections

	- Connecting RMAN to an Auxiliary Database

	- Making RMAN Database Connections Within Command Files

	- Diagnosing RMAN Connection Problems

* Using the RMAN Pipe Interface

#### 5 Configuring the RMAN Environment

* Configuring the Environment for RMAN Backups


	- Showing and Clearing Persistent RMAN Configurations
	
	- Configuring the Default Device for Backups: Disk or SBT

	- Configuring the Default Type for Backups: Backup Sets or Copies

	- Configuring Channels

	- Configuring Control File and Server Parameter File Autobackups

* Configuring RMAN to Make Backups to a Media Manager


	- Prerequisites for Using a Media Manager with RMAN

	- Determining the Location of the Media Management Library

	- Configuring Media Management Software for RMAN Backups

	- Testing Whether the Media Manager Library Is Integrated Correctly

	- Configuring SBT Channels for Use with a Media Manager

* Configuring the Fast Recovery Area


	- Overview of the Fast Recovery Area

	- Enabling the Fast Recovery Area

	- Disabling the Fast Recovery Area

	- Configuring Locations for Control Files and Redo Logs

	- Configuring RMAN File Creation in the Fast Recovery Area

* Configuring the Backup Retention Policy

* Backup Optimization and the CONFIGURE command

* Configuring an Archived Redo Log Deletion Policy

* Configuring RMAN in a Data Guard Environment

#### 6 Configuring the RMAN Environment: Advanced Topics


* Configuring Advanced Channel Options

	- About Channel Control Options
	- Configuring Specific Channel Parameters

* Configuring Advanced Backup Options

	- Configuring the Maximum Size of Backup Sets

	- Configuring the Maximum Size of Backup Pieces

	- Configuring Backup Duplexing

	- Configuring Tablespaces for Exclusion from Whole Database Backups

	- Configuring Compression Options

	- Configuring Backup Encryption

* Configuring Auxiliary Instance Data File Names

* Configuring the Snapshot Control File Location

* Configuring RMAN for Use with a Shared Server

* Enabling Lost Write Detection

#### 7 Using Flashback Database and Restore Points

### Part III Backing Up and Archiving Data
#### 8 RMAN Backup Concepts
#### 9 Backing Up the Database
#### 10 Backing Up the Database: Advanced Topics

### Part IV Managing RMAN Backups

#### 11 Reporting on RMAN Operations
#### 12 Maintaining RMAN Backups and Repository Records
#### 13 Managing a Recovery Catalog

### Part V Diagnosing and Responding to Failures

#### 14 RMAN Data Repair Concepts
#### 15 Diagnosing and Repairing Failures with Data Recovery Advisor
#### 16 Validating Database Files and Backups
#### 17 Performing Complete Database Recovery
#### 18 Performing Flashback and Database Point-in-Time Recovery
#### 19 Performing Block Media Recovery
#### 20 Performing RMAN Recovery: Advanced Scenarios
#### 21 Performing RMAN Tablespace Point-in-Time Recovery (TSPITR)

### Part VI Tuning and Troubleshooting
#### 22 Tuning RMAN Performance
#### 23 Troubleshooting RMAN Operations

### Part VII Transferring Data with RMAN

#### 24 Duplicating a Database
#### 25 Duplicating a Database: Advanced Topics
#### 26 Creating Transportable Tablespace Sets
#### 27 Transporting Data Across Platforms

Part VIII Performing User-Managed Backup and Recovery
#### 28 Making User-Managed Database Backups
#### 29 Performing User-Managed Database Flashback and Recovery
#### 30 Performing User-Managed Recovery: Advanced Scenarios
