# sqoop 실습
1. From the accounts table, import only the primary key, along with the first name, last name to
HDFS directory /loudacre/accounts/user_info. Please save the file in text format with tab
delimiters.
Hint: You will have to figure out what the name of the table columns are in order to complete
this exercise.


### accounts table 존재하는지 확인
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

### 컬럼 명 확인 
```
sqoop eval \
--connect jdbc:mysql://localhost/loudacre \
--username training --password training \
--query "describe accounts"
```
19/03/10 21:39:37 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.7.0
19/03/10 21:39:37 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
19/03/10 21:39:38 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.

*| Field                | Type                 | Null | Key | Default              | Extra                |

*| acct_num             | int(11)              | NO  | PRI | (null)               |                      |
*| acct_create_dt       | datetime             | NO  |     | (null)               |                      |
*| acct_close_dt        | datetime             | YES |     | (null)               |                      |
*| first_name           | varchar(255)         | NO  |     | (null)               |                      |
*| last_name            | varchar(255)         | NO  |     | (null)               |                      |
*| address              | varchar(255)         | NO  |     | (null)               |                      |
*| city                 | varchar(255)         | NO  |     | (null)               |                      |
*| state                | varchar(255)         | NO  |     | (null)               |                      |
*| zipcode              | varchar(255)         | NO  |     | (null)               |                      |
*| phone_number         | varchar(255)         | NO  |     | (null)               |                      |
*| created              | datetime             | NO  |     | (null)               |                      |
*| modified             | datetime             | NO  |     | (null)               |                      |

### import acct_num(PK), first_name, last_name, 텍스트파일로 구분자는 tab
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

### import 1번과 동일, 단 parquet format, snappy compression
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


### 데이터 값 확인
```
sqoop eval \
--connect jdbc:mysql://localhost/loudacre \
--username training --password training \
--query "select state from accounts group by state"
```
19/03/10 22:09:58 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.7.0
19/03/10 22:09:59 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
19/03/10 22:09:59 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.

*| state                |

*| AZ                   |
*| CA                   |
*| NV                   |
*| OR                   |


### import state=California인 경우, 단 parquet format, gzip compression
```
sqoop import \
--connect jdbc:mysql://localhost/loudacre \
--username training --password training \
--table accounts \
--target-dir /loudacre/accounts/CA \
--where "state='CA'" \
--as-parquetfile
```
- 참고사항: default compression은 gzip
19/03/10 22:10:31 INFO sqoop.Sqoop: Running Sqoop version: 1.4.6-cdh5.7.0
19/03/10 22:10:31 WARN tool.BaseSqoopTool: Setting your password on the command-line is insecure. Consider using -P instead.
19/03/10 22:10:32 INFO manager.MySQLManager: Preparing to use a MySQL streaming resultset.
19/03/10 22:10:32 INFO tool.CodeGenTool: Beginning code generation
19/03/10 22:10:32 INFO tool.CodeGenTool: Will generate java class as codegen_accounts
19/03/10 22:10:32 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/03/10 22:10:32 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/03/10 22:10:32 INFO orm.CompilationManager: HADOOP_MAPRED_HOME is /usr/lib/hadoop-mapreduce
Note: /tmp/sqoop-training/compile/72e7fa11914ceeba9f866f42429211d1/codegen_accounts.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
19/03/10 22:10:36 INFO orm.CompilationManager: Writing jar file: /tmp/sqoop-training/compile/72e7fa11914ceeba9f866f42429211d1/codegen_accounts.jar
19/03/10 22:10:36 WARN manager.MySQLManager: It looks like you are importing from mysql.
19/03/10 22:10:36 WARN manager.MySQLManager: This transfer can be faster! Use the --direct
19/03/10 22:10:36 WARN manager.MySQLManager: option to exercise a MySQL-specific fast path.
19/03/10 22:10:36 INFO manager.MySQLManager: Setting zero DATETIME behavior to convertToNull (mysql)
19/03/10 22:10:36 INFO mapreduce.ImportJobBase: Beginning import of accounts
19/03/10 22:10:36 INFO Configuration.deprecation: mapred.job.tracker is deprecated. Instead, use mapreduce.jobtracker.address
19/03/10 22:10:36 INFO Configuration.deprecation: mapred.jar is deprecated. Instead, use mapreduce.job.jar
19/03/10 22:10:37 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/03/10 22:10:37 INFO manager.SqlManager: Executing SQL statement: SELECT t.* FROM `accounts` AS t LIMIT 1
19/03/10 22:10:39 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
19/03/10 22:10:39 INFO client.RMProxy: Connecting to ResourceManager at /0.0.0.0:8032
19/03/10 22:10:41 INFO db.DBInputFormat: Using read commited transaction isolation
19/03/10 22:10:41 INFO db.DataDrivenDBInputFormat: BoundingValsQuery: SELECT MIN(`acct_num`), MAX(`acct_num`) FROM `accounts` WHERE ( state='CA' )
19/03/10 22:10:41 INFO db.IntegerSplitter: Split size: 32439; Num splits: 4 from: 1 to: 129760
19/03/10 22:10:41 INFO mapreduce.JobSubmitter: number of splits:4
19/03/10 22:10:41 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1552277057261_0009
19/03/10 22:10:42 INFO impl.YarnClientImpl: Submitted application application_1552277057261_0009
19/03/10 22:10:42 INFO mapreduce.Job: The url to track the job: http://localhost:8088/proxy/application_1552277057261_0009/
19/03/10 22:10:42 INFO mapreduce.Job: Running job: job_1552277057261_0009
19/03/10 22:10:52 INFO mapreduce.Job: Job job_1552277057261_0009 running in uber mode : false
19/03/10 22:10:52 INFO mapreduce.Job:  map 0% reduce 0%
19/03/10 22:11:03 INFO mapreduce.Job:  map 25% reduce 0%
19/03/10 22:11:12 INFO mapreduce.Job:  map 50% reduce 0%
19/03/10 22:11:21 INFO mapreduce.Job:  map 75% reduce 0%
19/03/10 22:11:30 INFO mapreduce.Job:  map 100% reduce 0%
19/03/10 22:11:30 INFO mapreduce.Job: Job job_1552277057261_0009 completed successfully
19/03/10 22:11:30 INFO mapreduce.Job: Counters: 30
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=569020
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=65386
		HDFS: Number of bytes written=4212382
		HDFS: Number of read operations=272
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=40
	Job Counters 
		Launched map tasks=4
		Other local map tasks=4
		Total time spent by all maps in occupied slots (ms)=0
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=31944
		Total vcore-seconds taken by all map tasks=31944
		Total megabyte-seconds taken by all map tasks=8177664
	Map-Reduce Framework
		Map input records=92416
		Map output records=92416
		Input split bytes=470
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=828
		CPU time spent (ms)=14440
		Physical memory (bytes) snapshot=768376832
		Virtual memory (bytes) snapshot=8296226816
		Total committed heap usage (bytes)=251920384
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=0
19/03/10 22:11:30 INFO mapreduce.ImportJobBase: Transferred 4.0172 MB in 51.365 seconds (80.0867 KB/sec)
19/03/10 22:11:30 INFO mapreduce.ImportJobBase: Retrieved 92416 records.


### 결과 확인
```
parquet-tools head \
hdfs://localhost/loudacre/accounts/CA
```
acct_num = 1
acct_create_dt = 1224803105000
first_name = Donald
last_name = Becton
address = 2275 Washburn Street
city = Oakland
state = CA
zipcode = 94660
phone_number = 5100032418
created = 1395174587000
modified = 1395174587000

acct_num = 2
acct_create_dt = 1226487601000
first_name = Donna
last_name = Jones
address = 3885 Elliott Street
city = San Francisco
state = CA
zipcode = 94171
phone_number = 4150835799
created = 1395174587000
modified = 1395174587000

acct_num = 3
acct_create_dt = 1229879990000
first_name = Dorthy
last_name = Chalmers
address = 4073 Whaley Lane
city = San Mateo
state = CA
zipcode = 94479
phone_number = 6506877757
created = 1395174587000
modified = 1395174587000

acct_num = 4
acct_create_dt = 1227859689000
first_name = Leila
last_name = Spencer
address = 1447 Ross Street
city = San Mateo
state = CA
zipcode = 94444
phone_number = 6503198619
created = 1395174587000
modified = 1395174587000

acct_num = 5
acct_create_dt = 1226819166000
first_name = Anita
last_name = Laughlin
address = 2767 Hill Street
city = Richmond
state = CA
zipcode = 94872
phone_number = 5107754354
created = 1395174587000
modified = 1395174587000