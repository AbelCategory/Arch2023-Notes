## 降低 Cache 的 miss rate

#### 编译优化降低miss rate

1. 合并数组。原先是

   ```cpp
   int val[233];
   int key[233];
   ```

   现在用个 struct

   ```cpp
   struct merge{
       int val;
       int key;
   };
   ```

   可以增加空间局部性，使得 val 和 key 在 cache 更连续。

2. 交换循环。遍历二维数组是先枚举第一维，再枚举第二维会降低 cache 的 miss rate，优于先枚举第二维，再枚举第一维。

3. 矩阵乘法 $A=B\cdot C$。朴素的写法是枚举 $i,j,k$ 分别是 $A$ 的行列和做内积的向量下标。优化后变成五层循环，将 $B$ 和 $C$ 分成小矩阵，提升了数据的局域性。（存在更好的并行性）。

#### 其他

把 cache 做的太大虽然能降低 miss rate，但是会提升 hit 时的时间，反而得不偿失。

## 降低 miss 的惩罚

写回策略有 write through 和 write back。write through （写穿）会同时更新缓存和内存中的结果，write back（写回）只会更新缓存中的值，引入 dirty 位记录是否被修改。只有在被逐出cache line的时候才会写回内存。

如果 write back 策略在 read 过程中 miss 了，且面临对应的 index 中的 valid 和 dirty 均为1，一种是先将对应的值写入内存，再把内存读出的值写道 cache line，另一种是把两个过程并行进行，会降低所需的时钟周期数。