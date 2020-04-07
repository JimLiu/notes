### hive create table
```sql
CREATE EXTERNAL TABLE `mid.dwd_yike_action`(
    xxx string
)
COMMENT 'xxx'
PARTITIONED BY ( 
  `dt` string COMMENT 'xxx', 
  `tb` string COMMENT 'xxx')
ROW FORMAT SERDE
    'org.apache.hadoop.hive.serde2.columnar.ColumnarSerDe'
WITH SERDEPROPERTIES ( 'field.delim' = '\1' , 'collected.delim' = '\2', 'mapkey.delim' = '\3', 'line.delim' = '\n')
   STORED AS rcfile;
```

### hive parquet table
``` sql
create external table jingqi_test.jingqi_parquet_test(
    f1 string,
    f2 bigint
) PARTITIONED BY (
  `f3` string )
STORED AS parquet
location 'hdfs:///user/jingqi/temp/output'
TBLPROPERTIES ("orc.compress"="SNAPPY"); 
```

### eg
```SQL
create table if not exists test_one_model_two_cube(
    xname string,
    xage string,
    xgender string,
    xcity string,
    xno int
)
row format delimited
fields terminated by ','
collection items terminated by '\002'
map keys terminated by '\003'
lines terminated by '\n'
stored as textfile;

load data local inpath '/home/myhome/jingqi/temp/test_cube/table.data'
overwrite into table test_one_model_two_cube
partition (dt='20190821');
```

### 数据样例
xname, xage, xgender, xcity, xno
a,1,0,c1,1
b,1,0,c1,1
c,2,1,c1,2
d,2,1,c2,2
e,3,1,c2,3
