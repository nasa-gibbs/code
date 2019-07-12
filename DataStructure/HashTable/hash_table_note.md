<!--
 * @Author: five-5
 * @Date: 2019-07-12 00:55:32
 * @Description: 哈希表笔记
 * @LastEditTime: 2019-07-12 17:47:38
 -->

# 哈希表 

充分体现了算法设计领域的经典思想：空间换时间
</br>哈希表是时间和空间之间的平衡

## 基本问题
- 设计哈希函数
1. 有些数据很难与索引相对应
2. “键”通过哈希函数得到的“索引”分布越均匀越好
</br>(对于一些特殊领域，有特殊领域的哈希函数设计方法甚至有专门的论文)
- 解决哈希冲突

## 哈希函数的设计
**原则**
1. 一致性：如果a == b， 则hash(a) == hash(b)
2. 高效性：计算高效简便
3. 均匀性：哈希值均匀分布 

**常见方法**
1. 直接寻址法
> H(key) = key
2. 平方取中法
</br>平方之后的中间几位和数字的每一位都相关
> H(key) = mid(key^2, n)
3. 随机数法
> H(key) = random(key)
4. 除留余数法
</br> 模数的选取很重要， 一般取素数，具体数值为数论讨论内容。可参照网站查看一些已经研究出来的素数：[good hash table primes](https://planetmath.org/goodhashtableprimes)
> H(key) = key % M


**整型**
- 小范围正整数直接使用
- 小范围负整数进行偏移：100-100 -> 0-200
- 大整数
eg. 身份证号 取模，分布不均匀（取模值的选取比较重要）；没有利用所有信息
</br>一个简单的解决方法：模一个素数（数论）

``` c++
/*
10 % 4 = 2            10 % 7 = 3
20 % 4 = 0            20 % 7 = 6
30 % 4 = 2            30 % 7 = 2
40 % 4 = 0            40 % 7 = 4
50 % 4 = 2            50 % 7 = 1
*/
```
**浮点型**
- 在计算机中都是32位或64位的二进制表示，把二进制当作整型处理
    
**字符串**  转成整型处理
- eg. 'code' = c * B^3 + o * B^2 + d * B^1 + e * B^0
((((C*B) + o) * B + d) * B + e) % M = 
</br> ((((C % M) * B + o) % M * B + d) % M * B + e)
hash(code) = () % M
``` c++
int hash = 0;
for (int i = 0; i < s.size(); ++i) {
    hash = (hash * B + s[i]) % M;
}
```

**复合类型** 转成整型处理
> Date: year month day

hash(Date) = (((year*B) + month) * B + day) % M 


## 哈希冲突的处理
链地址法 Sperate chaining
</br>0x07fffffff，31个1 抹去最高位
``` C++
0
1 -> K1
2 -> K2 -> K3
...
M-1
```
- 总共M个地址
- 如果放入哈希表的元素为N
- 每个地址是链表：O(N/M)
- 每个地址是平衡树：O(log(N/M))
</br>哈希碰撞攻击，使都冲突

- 需要resize
</br> 平均每个地址承载的元素多过一定程序，即扩容
N/M >= upperTol
</br> 平均每个地址承载的元素少过一定程序，即缩容
N / M < lowerTol

**哈希表的复杂度分析**
均摊分析，平均复杂度O(1)
每个操作在O(lowerTol)-O(upperTol)
缩容同理

**更多复杂的动态空间处理方法**
- 扩容后M还是素数
- 当哈希冲突达到一定程度，每一个位置从链表转成红黑树

**开放地址法**
</br> 每个地址对所有元素都是开放的。
- 线性探测法
- 平方探测法：步长为平方序列
- 伪随机序列
- 二次哈希法

也需要扩容， 负载率 N / M选择何时的情况下复杂度也是O(1)

**再哈希法**
</br> 使用其他哈希函数再映射

**Coalesced Hashing**
综合了Seperate Chaining 和 Open Addressing

**建立公共溢出区**

## 更多
哈希表牺牲了顺序性
1. 集合，映射： 有序[平衡树]； 无序[哈希表]