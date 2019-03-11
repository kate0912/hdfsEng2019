# sqoop 실습
1. From the accounts table, import only the primary key, along with the first name, last name to
HDFS directory /loudacre/accounts/user_info. Please save the file in text format with tab
delimiters.
Hint: You will have to figure out what the name of the table columns are in order to complete
this exercise.


## accounts table 존재하는지 확인
```
sqoop list-tables \
--connect jdbc:mysql://localhost/loudacre \
--username training --password training
```
19/03/10 21:34:07 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.7.0
19/03/10 21:34:07 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
19/03/10 21:34:07 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
accountdevice
accounts
basestations
customerservicerep
device
knowledgebase
mostactivestations
webpage

## 컬럼 명 확인 
```
sqoop eval \
--connect jdbc:mysql://localhost/loudacre \
--username training --password training \
--query "describe accounts"
```
19/03/10 21:39:37 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.7.0
19/03/10 21:39:37 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
19/03/10 21:39:38 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
---------------------------------------------------------------------------------------------------------
| Field                | Type                 | Null | Key | Default              | Extra                | 
---------------------------------------------------------------------------------------------------------
| acct_num             | int(11)              | NO  | PRI | (null)               |                      | 
| acct_create_dt       | datetime             | NO  |     | (null)               |                      | 
| acct_close_dt        | datetime             | YES |     | (null)               |                      | 
| first_name           | varchar(255)         | NO  |     | (null)               |                      | 
| last_name            | varchar(255)         | NO  |     | (null)               |                      | 
| address              | varchar(255)         | NO  |     | (null)               |                      | 
| city                 | varchar(255)         | NO  |     | (null)               |                      | 
| state                | varchar(255)         | NO  |     | (null)               |                      | 
| zipcode              | varchar(255)         | NO  |     | (null)               |                      | 
| phone_number         | varchar(255)         | NO  |     | (null)               |                      | 
| created              | datetime             | NO  |     | (null)               |                      | 
| modified             | datetime             | NO  |     | (null)               |                      | 
---------------------------------------------------------------------------------------------------------

## import acct_num(PK), first_name, last_name, 텍스트파일로 구분자는 tab
```
sqoop import \
--connect jdbc:mysql://localhost/loudacre \
--username training --password training \
--table accounts \
--target-dir /loudacre/accounts/user_info \
--columns "acct_num, first_name, last_name" \
--fields-terminated-by "\t" \
--as-textfile
```
19/03/10 21:47:54 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.7.0
19/03/10 21:47:54 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
19/03/10 21:47:54 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
19/03/10 21:47:54 INFO tool.CodeGenTool: Beginning code generation
19/03/10 21:47:55 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/03/10 21:47:55 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/03/10 21:47:55 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-training/compile/8ca57e6ce6475763ad761b67f3e77480/accounts.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
19/03/10 21:47:57 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-training/compile/8ca57e6ce6475763ad761b67f3e77480/accounts.jar
19/03/10 21:47:57 WARN manager.MySQLManager: It looks like you are importing from mysql.
19/03/10 21:47:57 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
19/03/10 21:47:57 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
19/03/10 21:47:57 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
19/03/10 21:47:57 INFO mapreduce.ImportJobBase: Beginning import of accounts
19/03/10 21:47:57 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
19/03/10 21:47:57 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
19/03/10 21:47:58 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
19/03/10 21:47:58 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
19/03/10 21:48:01 INFO db.DBInputFormat: Using read commited transaction isolation
19/03/10 21:48:01 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`acct_num`), MAX(`acct_num`) FROM `accounts`
19/03/10 21:48:01 INFO db.IntegerSplitter: Split size: 32440; Num splits: 4 from: 1 to: 129761
19/03/10 21:48:01 INFO mapreduce.JobSubmitter: number of splits:4
19/03/10 21:48:01 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1552277057261_0004
19/03/10 21:48:02 INFO impl.YarnClientImpl: Submitted application application_1552277057261_0004
19/03/10 21:48:02 INFO mapreduce.Job: The url to track the job: http://localhost:8088/proxy/application_1552277057261_0004/
19/03/10 21:48:02 INFO mapreduce.Job: Running job: job_1552277057261_0004
19/03/10 21:48:11 INFO mapreduce.Job: Job job_1552277057261_0004 running in uber mode : false
19/03/10 21:48:11 INFO mapreduce.Job:  map 0% reduce 0%
19/03/10 21:48:19 INFO mapreduce.Job:  map 25% reduce 0%
19/03/10 21:48:25 INFO mapreduce.Job:  map 50% reduce 0%
19/03/10 21:48:31 INFO mapreduce.Job:  map 75% reduce 0%
19/03/10 21:48:38 INFO mapreduce.Job:  map 100% reduce 0%
19/03/10 21:48:38 INFO mapreduce.Job: Job job_1552277057261_0004 completed successfully
19/03/10 21:48:38 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=560464
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=470
		HDFS: Number of bytes written=2615920
		HDFS: Number of read operations=16
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=8
	Job Counters 
		Launched map tasks=4
		Other local map tasks=4
		Total time spent by all maps in occupied slots (ms)=0
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=18520
		Total vcore-seconds taken by all map tasks=18520
		Total megabyte-seconds taken by all map tasks=4741120
	Map-Reduce Framework
		Map input records=129761
		Map output records=129761
		Input split bytes=470
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=232
		CPU time spent (ms)=4000
		Physical memory (bytes) snapshot=492511232
		Virtual memory (bytes) snapshot=8262221824
		Total committed heap usage (bytes)=251920384
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=2615920
19/03/10 21:48:38 INFO mapreduce.ImportJobBase: Transferred 2.4947 MB in 40.1252 seconds (63.666 KB/sec)
19/03/10 21:48:38 INFO mapreduce.ImportJobBase: Retrieved 129761 records.


2. This time save the same in parquet format with snappy compression. Save it in
/loudacre/accounts/user_compressed. Provide.a screenshot of HUE with the new directory
created.

## import 1번과 동일, 단 parquet format, snappy compression
```
sqoop import \
--connect jdbc:mysql://localhost/loudacre \
--username training --password training \
--table accounts \
--target-dir /loudacre/accounts/user_compressed \
--columns "acct_num, first_name, last_name" \
--fields-terminated-by "\t" \
--as-parquetfile \
--compression-codec \
org.apache.hadoop.io.compress.SnappyCodec
```
19/03/10 21:58:31 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.7.0
19/03/10 21:58:31 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
19/03/10 21:58:32 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
19/03/10 21:58:32 INFO tool.CodeGenTool: Beginning code generation
19/03/10 21:58:32 INFO tool.CodeGenTool: Will generate java class as codegen_accounts
19/03/10 21:58:32 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/03/10 21:58:32 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/03/10 21:58:32 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-training/compile/46fa2838b986f92bda399526b05fb0bb/codegen_accounts.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
19/03/10 21:58:34 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-training/compile/46fa2838b986f92bda399526b05fb0bb/codegen_accounts.jar
19/03/10 21:58:34 WARN manager.MySQLManager: It looks like you are importing from mysql.
19/03/10 21:58:34 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
19/03/10 21:58:34 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
19/03/10 21:58:34 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
19/03/10 21:58:34 INFO mapreduce.ImportJobBase: Beginning import of accounts
19/03/10 21:58:34 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
19/03/10 21:58:35 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
19/03/10 21:58:35 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/03/10 21:58:35 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/03/10 21:58:37 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
19/03/10 21:58:37 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
19/03/10 21:58:39 INFO db.DBInputFormat: Using read commited transaction isolation
19/03/10 21:58:39 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`acct_num`), MAX(`acct_num`) FROM `accounts`
19/03/10 21:58:39 INFO db.IntegerSplitter: Split size: 32440; Num splits: 4 from: 1 to: 129761
19/03/10 21:58:39 INFO mapreduce.JobSubmitter: number of splits:4
19/03/10 21:58:39 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1552277057261_0006
19/03/10 21:58:39 INFO impl.YarnClientImpl: Submitted application application_1552277057261_0006
19/03/10 21:58:39 INFO mapreduce.Job: The url to track the job: http://localhost:8088/proxy/application_1552277057261_0006/
19/03/10 21:58:39 INFO mapreduce.Job: Running job: job_1552277057261_0006
19/03/10 21:58:48 INFO mapreduce.Job: Job job_1552277057261_0006 running in uber mode : false
19/03/10 21:58:48 INFO mapreduce.Job:  map 0% reduce 0%
19/03/10 21:58:57 INFO mapreduce.Job:  map 25% reduce 0%
19/03/10 21:59:06 INFO mapreduce.Job:  map 50% reduce 0%
19/03/10 21:59:15 INFO mapreduce.Job:  map 75% reduce 0%
19/03/10 21:59:24 INFO mapreduce.Job:  map 100% reduce 0%
19/03/10 21:59:24 INFO mapreduce.Job: Job job_1552277057261_0006 completed successfully
19/03/10 21:59:24 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=565908
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=25234
		HDFS: Number of bytes written=1305047
		HDFS: Number of read operations=272
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=40
	Job Counters 
		Launched map tasks=4
		Other local map tasks=4
		Total time spent by all maps in occupied slots (ms)=0
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=27863
		Total vcore-seconds taken by all map tasks=27863
		Total megabyte-seconds taken by all map tasks=7132928
	Map-Reduce Framework
		Map input records=129761
		Map output records=129761
		Input split bytes=470
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=550
		CPU time spent (ms)=10060
		Physical memory (bytes) snapshot=672690176
		Virtual memory (bytes) snapshot=8296374272
		Total committed heap usage (bytes)=251920384
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=0
19/03/10 21:59:24 INFO mapreduce.ImportJobBase: Transferred 1.2446 MB in 47.9265 seconds (26.5919 KB/sec)
19/03/10 21:59:24 INFO mapreduce.ImportJobBase: Retrieved 129761 records.


3. Finally save in /loudacre/accounts/CA only clients whose state is from California. Save the file
in parquet format and compressed using gzip. From the terminal, display some of the records
that you just imported. Take a screenshot and save it as CA_only.

## import state=California인 경우, 단 parquet format, gzip compression
```
sqoop import \
--connect jdbc:mysql://localhost/loudacre \
--username training --password training \
--table accounts \
--target-dir /loudacre/accounts/CA \
--where "state='California'" \
--as-parquetfile
```
- 참고사항: default compression은 gzip
19/03/10 22:02:42 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.7.0
19/03/10 22:02:42 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
19/03/10 22:02:43 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
19/03/10 22:02:43 INFO tool.CodeGenTool: Beginning code generation
19/03/10 22:02:43 INFO tool.CodeGenTool: Will generate java class as codegen_accounts
19/03/10 22:02:43 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/03/10 22:02:43 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/03/10 22:02:43 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-training/compile/55bf205c50259715369df6dc7ccb942b/codegen_accounts.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
19/03/10 22:02:46 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-training/compile/55bf205c50259715369df6dc7ccb942b/codegen_accounts.jar
19/03/10 22:02:46 WARN manager.MySQLManager: It looks like you are importing from mysql.
19/03/10 22:02:46 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
19/03/10 22:02:46 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
19/03/10 22:02:46 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
19/03/10 22:02:46 INFO mapreduce.ImportJobBase: Beginning import of accounts
19/03/10 22:02:46 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
19/03/10 22:02:46 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
19/03/10 22:02:47 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/03/10 22:02:47 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/03/10 22:02:49 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
19/03/10 22:02:49 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
19/03/10 22:02:51 INFO db.DBInputFormat: Using read commited transaction isolation
19/03/10 22:02:51 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`acct_num`), MAX(`acct_num`) FROM `accounts` WHERE ( state='California' )
19/03/10 22:02:51 INFO mapreduce.JobSubmitter: number of splits:1
19/03/10 22:02:51 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1552277057261_0008
19/03/10 22:02:52 INFO impl.YarnClientImpl: Submitted application application_1552277057261_0008
19/03/10 22:02:52 INFO mapreduce.Job: The url to track the job: http://localhost:8088/proxy/application_1552277057261_0008/
19/03/10 22:02:52 INFO mapreduce.Job: Running job: job_1552277057261_0008
19/03/10 22:03:00 INFO mapreduce.Job: Job job_1552277057261_0008 running in uber mode : false
19/03/10 22:03:00 INFO mapreduce.Job:  map 0% reduce 0%
19/03/10 22:03:09 INFO mapreduce.Job:  map 100% reduce 0%
19/03/10 22:03:09 INFO mapreduce.Job: Job job_1552277057261_0008 completed successfully
19/03/10 22:03:10 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=142271
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=16346
		HDFS: Number of bytes written=5563
		HDFS: Number of read operations=68
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=8
	Job Counters 
		Launched map tasks=1
		Other local map tasks=1
		Total time spent by all maps in occupied slots (ms)=0
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=6225
		Total vcore-seconds taken by all map tasks=6225
		Total megabyte-seconds taken by all map tasks=1593600
	Map-Reduce Framework
		Map input records=0
		Map output records=0
		Input split bytes=117
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=145
		CPU time spent (ms)=1650
		Physical memory (bytes) snapshot=173477888
		Virtual memory (bytes) snapshot=2070806528
		Total committed heap usage (bytes)=62980096
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=0
19/03/10 22:03:10 INFO mapreduce.ImportJobBase: Transferred 5.4326 KB in 20.9492 seconds (265.5473 bytes/sec)
19/03/10 22:03:10 INFO mapreduce.ImportJobBase: Retrieved 0 records.


## 결과 확인
sqoop eval \
--connect jdbc:mysql://localhost/loudacre \
--username training --password training \
--query "select "