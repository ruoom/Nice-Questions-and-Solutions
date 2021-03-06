稀疏表

#### 主要作用

解决**RMQ**问题，查询区间最大最小值。应用**倍增**的思想。

预处理$O(nlogn)$ 查询$O(1)$



其实ST表不仅能处理最大值/最小值，凡是符合**结合律**且**可重复贡献**的信息查询都可以使用ST表高效进行。什么叫可重复贡献呢？设有一个二元运算  $f(x,y)$，满足  $f(a,a)=a$，则$f$是可重复贡献的。显然最大值、最小值、最大公因数、最小公倍数、按位或、按位与都符合这个条件。可重复贡献的意义在于，可以对两个交集不为空的区间进行信息合并。



### 预处理

循环$\log maxn$轮, 每次处理从$i$到$i+2^k$区间的结果. 这个结果怎么处理呢? 首先把这个区间分成两半, 也就是从$i$到$i+2^{k-1}$和$i+2^{k-1}$到$i+2^k$. 由于$2^{k-1}$这些都是上一轮处理过的了, 所以现在可以直接用.

![img](https://pic4.zhimg.com/80/v2-22d8a24faea894fb8ddceae627093bbf_720w.jpg)

预处理区间最大值:

```c++
int f[MAXN][21]; // 第二维的大小根据数据范围决定，不小于log(MAXN)
for (int i = 1; i <= n; ++i)
    f[i][0] = read(); // 读入数据
for (int i = 1; i <= 20; ++i)
    for (int j = 1; j + (1 << i) - 1 <= n; ++j)
        f[j][i] = max(f[j][i - 1], f[j + (1 << (i - 1))][i - 1]);
```



### 查询

查询区间$[l,r]$. 我们先找到一个2的k次方使其小于这个区间的长度. 其实$k$就等于$\lfloor\log2(r-l+1)\rfloor$. 然后我们直接查询区间$l$到$l+2^k$的值和$r-2^k+1$到$r$的值, 然后两个处理一下就行了.

![img](https://pic4.zhimg.com/80/v2-9d09b3492f0c0cbaa7555a56b22c1693_720w.jpg)

算$log$耗时, 所以可以预处理所有$log$的结果.

查询最大值:

```c++
for (int i = 0; i < m; ++i)
{
    int l = read(), r = read();
    int s = Log2[r - l + 1];
    printf("%d\n", max(f[l][s], f[r - (1 << s) + 1][s]));
}
```

