# bit array

### 各种操作
setbit $bitArrayName $index $value
#### setbit 
1. setbit jingqi_bitmap_1 0 1              // 最小索引
2. setbit jingqi_bitmap_1 4294967295 1     // 最大索引
3. getbit jingqi_bitmap_1 0                // (integer) 1
4. getbit jingqi_bitmap_1 1                // (integer) 0
5. 