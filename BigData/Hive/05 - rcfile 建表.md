hive create table
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
### eg
```sql
create table if not exists test_one_model_two_cube(
    xname string,
    xage string,
    xgender string,
    xcity string,
    xno int
)
```

### 数据样例

