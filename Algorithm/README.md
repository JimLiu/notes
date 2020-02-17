# Algorithm
## 思考问题的几种方法
    1. 反推法: 假设结论已经知道的情况下, 反推出结论. 
       例子: kmp 算法中推导的过程(不包含 next(j) 函数的思考过程)
    2. 迭代法: 已经初始的边界条件,通过不断的迭代求后面的解.
       例子: Fibonacci 数列. kmp 算法中求解 next(j) 的具体值的实现
    3. 可逆法: 如果已知条件 a, 得出 结论 b, 需要反向思考. 如果知道 b, 能不能唯一得出 a
       例子: 数据中 sin(a) + cos(a) 的那个公式.(我忘记叫啥了) 
    4. 动态规划法
    5. 贪心法
    6. 反推正向反向迭代法: 先假设结论已知, 然后迭代的向前向后迭代的推导出结论.向右(索引增加方向)推导不需要终止条件.
       向左(索引减小方向)是需要终止条件的.
       例子: kmp 算法中, 如何实现 next(j) 函数的思考过程.
    7. 问题元素与问题位置: 求解概率相关问题的时候用到的想法
    8. 对称法: 对称无处不在
## 在线编程
    1. 在线编程白板: http://collabedit.com
## 书籍推荐
    1. << Outliers >> 异类-不一样的成功启示录
       1) 切碎知识点
       2) 反复练习
       3) 反馈: 主动反馈(多了解别人, 看别人), 被动反馈(code review)
## 知识结构
    {
        Abstract data type {
            Stack = [ vector ]
            Queue = [Linked List, Priority Queue { heap }]
            Set = [Hash Set, Tree Set]
            Map = [Hash Map, Tree Map] 
        }     
    }
## 数据结构
    1. Array
    2. Stack / Queue
    3. PriorityQueue (heap)
    4. LinkedList (single, double)
    5. Tree / Binary Tree
    6. HashTable
    7. Disjoint Set
    8. Trie
    9. BloomFilter
    10. LRU Cache
## Algorithm
    1. General Coding
    2. In-order / Pre-order / Post-order traversal
    3. Greedy
    4. Recursion / Backtrace
    5. Breadth-first search
    6. Depth-first search
    7. Divide and Conquer
    8. Dynamic Programming
    9. Binary Search
    10. Gra 
    














