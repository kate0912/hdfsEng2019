# Spark 실습
### 디바이스 정보 원본 파일 HDFS상에 업로드
```
hdfs dfs -mkdir /loudacre/device
hdfs dfs -put $DEVDATA/devicestatus.txt  /loudacre/device
```
- A. Upload the devicestatus.txt file to HDFS.
### 업로드한 데이터 확인
```
hdfs dfs -cat /loudacre/device/devicestatus.txt | head -10
```
- 결과값
```
2014-03-15:10:10:20,Sorrento F41L,8cc3b47e-bd01-4482-b500-28f2342679af,7,24,39,enabled,disabled,connected,55,67,12,33.6894754264,-117.543308253
2014-03-15:10:10:20|MeeToo 1.0|ef8c7564-0a1a-4650-a655-c8bbd5f8f943|0|31|63|70|39|27|enabled|enabled|enabled|37.4321088904|-121.485029632
2014-03-15:10:10:20|MeeToo 1.0|23eba027-b95a-4729-9a4b-a3cca51c5548|0|20|21|86|54|34|enabled|enabled|enabled|39.4378908349|-120.938978486
2014-03-15:10:10:20,Sorrento F41L,707daba1-5640-4d60-a6d9-1d6fa0645be0,8,22,60,enabled,enabled,disabled,68,91,17,39.3635186767,-119.400334708
2014-03-15:10:10:20,Ronin Novelty Note 1,db66fe81-aa55-43b4-9418-fc6e7a00f891,0,13,47,70,enabled,enabled,enabled,10,45,33.1913581092,-116.448242643
2014-03-15:10:10:20,Sorrento F41L,ffa18088-69a0-433e-84b8-006b2b9cc1d0,3,10,36,enabled,connected,enabled,53,58,42,33.8343543748,-117.330000857
2014-03-15:10:10:20,Sorrento F33L,66d678e6-9c87-48d2-a415-8d5035e54a23,1,34,74,enabled,disabled,enabled,57,42,15,37.3803954321,-121.840756755
2014-03-15:10:10:20|MeeToo 4.1|673f7e4b-d52b-44fc-8826-aea460c3481a|1|16|77|61|24|50|enabled|disabled|enabled|34.1841062345|-117.9435329
2014-03-15:10:10:20,Ronin Novelty Note 2,a678ccc3-b0d2-452d-bf89-85bd095e28ee,0,10,97,63,enabled,enabled,connected,48,4,32.2850556785,-111.819583734
2014-03-15:10:10:20,Sorrento F41L,86bef6ae-2f1c-42ec-aa67-6acecd7b0675,9,27,35,enabled,enabled,enabled,62,41,19,45.2400522984,-122.377467861
```

### ETL 수행 및 해당 경로에 결과 데이터 저장
```
sc.textFile("/loudacre/device/devicestatus.txt") \
.map(lambda line: line.split(line[19:20])) \
.filter(lambda values: len(values) == 14) \
.map(lambda values: (values[0], values[1].split(' ')[0], values[2], values[12], values[13])) \
.map(lambda values: ','.join(values)) \
.saveAsTextFile("/loudacre/devicestatus_etl")
```
- B. Determine which delimiter to use (hint: the character at
position 19 is the first use of the delimiter).
> map(lambda line: line.split(line[19:20])): 구분자 위치가 모두 동일하게 19-20번째 위치여서 해당 문자열을 잘라와 그 문자로 split 수행
- C. Filter out any records which do not parse correctly 
(hint: each record should have exactly 14 values).
> filter(lambda values: len(values) == 14): 각 레코드는 반드시 14개의 values로 구성되어 있어야해서 len()을 사용해 filter 수행
- D. Extract the date (first field), model (second field), device ID 
(third field), and latitude and longitude (13th and 14th fields respectively).
- E. The second field contains the device manufacturer and model name (such as Ronin S2). 
Split this field by spaces to separate
the manufacturer from the model (for example, manufacturer Ronin, model S2). 
Keep just the manufacturer name.
> map(lambda values: (values[0], values[1].split(' ')[0], values[2], values[12], values[13])): 각각의 레코드마다 해당 values의 index값으로
가져옴, 단 manufacturer name의 경우 name과 model이 한 value값에 들어있어 다시 스페이스로 split해 첫번째 요소를 가져옴
- F. Save the extracted data to comma-delimited text files in the
/loudacre/devicestatus_etl directory on HDFS.
> join을 통해서 , 로 구분되도록 설정하고 saveAsTextFile을 통해서 해당 HDFS 위치(/loudacre/devicestatus_etl)에 결과 값 저장함


### 저장된 데이터 확인
```
hdfs dfs -cat /loudacre/devicestatus_etl/part-00000 | head -10
```
- G. Confirm that the data in the file(s) was saved correctly.
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