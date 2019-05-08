# bit array

### 各种操作
setbit $bitArrayName $index $value
#### setbit 将对应索引的比特位置为 0， 或者 1
1. setbit jingqi_bitmap_1 0 1              // 最小索引
2. setbit jingqi_bitmap_1 4294967295 1     // 最大索引
#### getbit 将对应索引位置值获取出来
3. getbit jingqi_bitmap_1 0                // (integer) 1
4. getbit jingqi_bitmap_1 1                // (integer) 0
#### bitcount 获取 bitarray 1 的个数
5. bitcount jingqi_bitmap_1                // (integer) 4
#### bittop 命令, 可以对 多个位数组进行按位与（and），按位或()，按位异或
x = 0000 1011
6. setbit x 3 1; setbit x 1 1; setbit x 0 1
y = 0000 0110
7. setbit y 2 1; setbit y 1 1
z = 0000 0101
8. setbit z 2 1; setbit z 0 1

9. bitop and and-result x y z # 0000 0000
10. bitop or or-result x y z 
