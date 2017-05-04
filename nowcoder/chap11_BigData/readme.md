map-reduce 和 hadoop 逐步成为面试热门

哈希函数 hashmap	
    哈希函数也叫散列表，哈希函数的输入域可以是非常大得范围。但是输出域是固定范围	
    不同输入值得到的哈希值月均匀分布在输出域上，哈希函数越优秀。	
    (MD5算法，SHA1算法)	
    
介绍 Map-Reduce	
1. Map          把大任务分成子任务	
2. Reduce阶段   子问题并法处理，然后合并结果。	

注意点：	
1. 备份的考虑，分布式的存储设计细节，以及容灾策略	
2. 任务分配策略与任务进度跟踪的细节设计，节点状态的呈现。	
3. 多用户权限的控制	


海量处理题目解题关键  
1. 分而治之。通过哈希函数将大人物分流到机器
2. 常用到 hashMap 和 bitmap
难点在 通讯，时间，空间的计算。

【1】请对10亿个IPV4的IP地址进行排序，每个IP地址只出现一次。
申请一个bitmap 长度为2^32, 512 M   
其中 2^10 为 1k    
    1 byte = 8 bit = 2^3    
其中 10亿 为 2^30

【2】请对10亿人的年龄进行排序

【3】有一个包含20亿全是32位整数的大文件，在其中找到出现次数最多的数。内存限制2G
使用hash函数进行分流。   
同一种数不会被分流道不同文件，这是哈希函数的性质决定的。
不同的数，每个文件中含有整数的种数几乎一样，这也是哈希函数性质决定的。

【4】32位无符号整数的范围是0 ~ 42 9496 7295. 现在有一个正好包含40亿个无符号整数的文件。
所以在整个范围中必然有没出现过的数，可以使用最多10M的内存，只用找到一个没出现的数即可，该如何找。
Hash 16 G 和 bitmap 500 M都不可
1. 根据内存限制决定区间大小，根据区间大小得到有多少变量来记录每个区间数出现次数。 
2. 统计区间上的数出现次数，找到不足的区间。
3. 利用bitmap对不满的区间，进行这个区间上的数的词频统计。

【5】某搜索公司一天的用户搜索词汇是海量的，假设有百亿的数据量，请设计一种求出每天最热次的办法。    
使用哈希函数进行分流。

【6】工程师常使用服务器集群来设计和实现数据缓存，一下是常见策略：   
1. 无论是否添加，查询还是删除数据，都先将数据的id通过哈希函数转换成一个哈希值，记为k
2. 如果目前机器有N台，则计算key%N 的值， 这个值就是该数据所属的机器编号，
无论是添加，删除还是查询造作，都只是在这台机器上进行，请分析这种缓存策略可能带来的问题，并提出改进的方案。

潜在问题：如果增加或者删除机器，数据迁移的代价很大。  
一致性哈希算法：    
    