# Spark 실습
### 디바이스 정보 원본 파일 HDFS상에 업로드
```
hdfs dfs -mkdir /loudacre/device
hdfs dfs -put $DEVDATA/devicestatus.txt  /loudacre/device
```

### ETL 수행 및 해당 경로에 결과 데이터 저장
```
sc.textFile("/loudacre/device/devicestatus.txt") \
.filter(lambda line: len(line) > 20) \
.map(lambda line: line.split(line[19:20])) \
.filter(lambda values: len(values) == 14) \
.map(lambda values: (values[0], values[1].split(' ')[0], values[2], values[12], values[13])) \
.map(lambda values: ','.join(values)) \
.saveAsTextFile("/loudacre/devicestatus_etl")
```
- A. Upload the devicestatus.txt file to HDFS.
- B. Determine which delimiter to use (hint: the character at
position 19 is the first use of the delimiter).
- C. Filter out any records which do not parse correctly 
(hint: each record should have exactly 14 values).
- D. Extract the date (first field), model (second field), device ID 
(third field), and latitude and longitude (13th and 14th fields respectively).
- E. The second field contains the device manufacturer and model name (such as Ronin S2). 
Split this field by spaces to separate
the manufacturer from the model (for example, manufacturer Ronin, model S2). 
Keep just the manufacturer name.
- F. Save the extracted data to comma-delimited text files in the
/loudacre/devicestatus_etl directory on HDFS.
- G. Confirm that the data in the file(s) was saved correctly.

### 저장된 데이터 확인
```
hdfs dfs -cat /loudacre/devicestatus_etl/part-00000 | head -10
```
- 결과값
```
2014-03-15:10:10:20,Sorrento,8cc3b47e-bd01-4482-b500-28f2342679af,33.6894754264,-117.543308253
2014-03-15:10:10:20,MeeToo,ef8c7564-0a1a-4650-a655-c8bbd5f8f943,37.4321088904,-121.485029632
2014-03-15:10:10:20,MeeToo,23eba027-b95a-4729-9a4b-a3cca51c5548,39.4378908349,-120.938978486
2014-03-15:10:10:20,Sorrento,707daba1-5640-4d60-a6d9-1d6fa0645be0,39.3635186767,-119.400334708
2014-03-15:10:10:20,Ronin,db66fe81-aa55-43b4-9418-fc6e7a00f891,33.1913581092,-116.448242643
2014-03-15:10:10:20,Sorrento,ffa18088-69a0-433e-84b8-006b2b9cc1d0,33.8343543748,-117.330000857
2014-03-15:10:10:20,Sorrento,66d678e6-9c87-48d2-a415-8d5035e54a23,37.3803954321,-121.840756755
2014-03-15:10:10:20,MeeToo,673f7e4b-d52b-44fc-8826-aea460c3481a,34.1841062345,-117.9435329
2014-03-15:10:10:20,Ronin,a678ccc3-b0d2-452d-bf89-85bd095e28ee,32.2850556785,-111.819583734
2014-03-15:10:10:20,Sorrento,86bef6ae-2f1c-42ec-aa67-6acecd7b0675,45.2400522984,-122.377467861
```