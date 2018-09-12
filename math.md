# 数论杂谈（by $KesdiaelKen$）


------------


## 目录：
逆元求法

整除分块

卡特兰数

------------

## 逆元

**逆元定义：已知正整数$a,b$，$(a,b)=1,a<b$，求使得$ax\space mod \space b=1$**的$x(1<=x<=b)$

### 问题

求1到n模p(p为质数且p>n)意义下的逆元

### 求法1 扩展欧几里得求

直接对于每一个都求一次

$$O(n \log{n})$$

### 求法2 费马小定理

$\because a^{p-1}\equiv 1(mod\space p)$

$\therefore $设$ax\equiv 1(mod\space p)\space \therefore ax\equiv a^{p-1}(mod\space p)$

$\therefore x\equiv a^{p-2}(mod\space p)$

单个求：
$$O(n \log{n})$$

优化：

有a的逆元为$a^{p-2}$，b的逆元为$b^{p-2}$，$ab$的逆元为$(ab)^{p-2}$

$\therefore x_a x_b=x_{ab}$

线性筛求解

$$O(n)$$

### 求法3 递推公式

设$k^{-1}$表示k的逆元

设$p\div i=k...r \space\therefore p=ki+r$

$\therefore ki+r\equiv 0(mod\space p)\space$
$\therefore (ki+r)i^{-1}r^{-1}\equiv 0(mod\space p)$

$\therefore kr^{-1}+i^{-1}\equiv 0(mod\space p)\space \therefore i^{-1}\equiv (p-k)r^{-1}(mod\space p)$

$\therefore i^{-1}\equiv (p-p/i)(p\space mod\space i)^{-1}(mod\space p)$

$$O(n)$$

### 求法4 阶乘求法

首先$O(\log n)$求$n!$逆元

有$((k-1)!)^{-1}= (k!)^{-1}*k\space mod\space p$

之后$O(n)$倒着递推求出$1!$到$n!$的逆元

又有$k^{-1}=(k!)^{-1}*(k-1)!$

再次递推求即可

$$O(n+\log n)$$

## 整除分块

问题：求$\sum^n_{i=1}\lfloor\frac{n}{i}\rfloor$，
（当然不是只能在这个式子上应用，但它的确是最经常应用到整除分块的式子，没有之一）并将时间复杂度从$O(n)$降到$O(\sqrt n)$。

整除分块能实现基于这样一个事实：$\lfloor\frac{n}{i}\rfloor$的取值总共不会超过$2\sqrt n$种。为什么呢，我们来**证明**一下：

**当$i<=\sqrt n$时：**

我们需要证明：$\lfloor\frac{n}{i}\rfloor\not=\lfloor\frac{n}{i+1}\rfloor$

我们设$n\div i=k...s$，那么就有$n=i*k+s$。根据除法的性质，$s<i$，而又因为$i<=\sqrt n$，所以$k>=\sqrt n>=i$，所以$s<k$。

如果$\lfloor\frac{n}{i}\rfloor=\lfloor\frac{n}{i+1}\rfloor$，那么$n$就可以表示为$(i+1)*k+p$（$p>=0$），即$n=i*k+s+(k+p-s)=n+(k+p-s)$。而我们又知道$k-s>0,p>=0$。所以$k+p-s>0$，即等式两边矛盾。所以由反证法我们就得到了$\lfloor\frac{n}{i}\rfloor\not=\lfloor\frac{n}{i+1}\rfloor$。因此，在此情况下$\lfloor\frac{n}{i}\rfloor$的取值个数就等于$i$的取值数，即$\sqrt n$种。

**当$i>\sqrt n$时：**

这个不用怎么说了吧……$\lfloor\frac{n}{i}\rfloor_{max}$都已经是小于$\sqrt n$的了，总取值数当然不超过$\sqrt n$啊。

所以，两类加起来就是$2\sqrt n$种了。$$\mathcal{Q.E.D}$$

既然证玩了这个结论，我们就可以考虑这样一个思路：因为$\lfloor\frac{n}{i}\rfloor$的取值数为$\sqrt n$（那个$2$是常数，可以忽略），那么只要求出它取每一种值时$i$的最大值和最小值是多少，最终求和的时候用前缀和预处理算一下每一段的值是多少，加起来就可以了。

**那么接下来，我们又需要证明一个结论了：**

如果已知正整数$p,n$（$n>=p$），则使$\lfloor\frac{n}{i}\rfloor=\lfloor\frac{n}{p}\rfloor$的最大整数$i$的值就是$i_{max}=\lfloor\frac{n}{\frac{n}{p}}\rfloor$。

**证明：**

我们同样分类讨论一下。

**当$p<=\sqrt n$时：**

我们设$n\div p=k...s$，那么就有$n=p*k+s$。根据除法的性质，$s<p$，而又因为$p<=\sqrt n$，所以$k>=\sqrt n>=p$，所以$s<k$。而$\lfloor\frac{n}{p}\rfloor$的值就是$k$。

那么，就会有$\lfloor\frac{n}{\frac{n}{p}}\rfloor=\lfloor\frac{n}{k}\rfloor$。在此基础上，让我们再证明一个结论：$\lfloor\frac{n}{k}\rfloor=p$。我们令$n\div k=(p+t)...w$，则$n=p*k+t*k+w$。又因为$n=p*k+s$而，所以$n=p*k+t*k+w=n+(t*k+w-s)$，所以$t*k+w-s=0$。因为$k>w$且$k>=\sqrt n$，所以我们可以得出：$w,s\in[0,\sqrt n)$。所以：$(w-s)\in(-\sqrt n,\sqrt n)$。又因为$k>=\sqrt n$，所以$t$必须为$0$。所以$\lfloor\frac{n}{k}\rfloor=p$就证完了。而又由我们上面证的结论：$\lfloor\frac{n}{i}\rfloor$的取值总共不会超过$2\sqrt n$种可以知道当$p<=\sqrt n$的时候，所有$\lfloor\frac{n}{p}\rfloor$的值都不相同，即$p=i_{max}$。所以$i_{max}=\lfloor\frac{n}{\frac{n}{p}}\rfloor$，得证了。

**当$p>\sqrt n$时：**

我们设$n\div p=k...s$，那么就有$n=p*k+s$。根据除法的性质，$s<p$。而$\lfloor\frac{n}{p}\rfloor$的值就是$k$。

那么，就会有$\lfloor\frac{n}{\frac{n}{p}}\rfloor=\lfloor\frac{n}{k}\rfloor$。而我们现在要证明的，就是$\lfloor\frac{n}{k}\rfloor=i_{max}$。看上去很难证，但是我们注意到$i_{max}$必须符合且只需要符合的两个条件：$\lfloor\frac{n}{i_{max}}\rfloor=\lfloor\frac{n}{p}\rfloor=k,\lfloor\frac{n}{i_{max}+1}\rfloor\not=\lfloor\frac{n}{p}\rfloor=k$。那么，如果我们需要证明$\lfloor\frac{n}{k}\rfloor=i_{max}$，那么就只需要说明$\lfloor\frac{n}{\lfloor\frac{n}{k}\rfloor}\rfloor=\lfloor\frac{n}{p}\rfloor=k\space\space and\space\space\lfloor\frac{n}{\lfloor\frac{n}{k}\rfloor+1}\rfloor\not=\lfloor\frac{n}{p}\rfloor=k$。看到$\lfloor\frac{n}{\lfloor\frac{n}{k}\rfloor}\rfloor=k$你想到了什么？

聪明的读者一定已经想到了，因为这个$k<\sqrt n$（讲了这么多这个不可能不会自行理解吧），所以这个结论在上面讨论$p<=\sqrt n$的时候已经证过了。所以，得证啦！

$$\mathcal{Q.E.D}$$

综合上面两个结论，我们便可以得出整除分块的思路了：

1. 令$i=1$

2. 对于当前的$i$求得$i_{max}$，设为$j$

3. 通过前缀和或公式（例如等差数列公式）求得函数（$\sum_{i=1}^nf(i)$中的$f(i)$）的自变量从$i$一直到$j$的函数值的总和，并且加进$sum$

4. 我们已知$\lfloor\frac{n}{j}\rfloor\not=\lfloor\frac{n}{j+1}\rfloor$，所以我们让$i=j+1$。如果现在的$i$仍然小于$n$，则跳转第$2$步，否则结束

最终得到的$sum$就是答案了。

## 卡特兰数

### 递推公式1

$$a_1=1,a_i=a_1a_{i-1}+a_2a_{i-2}+...+a_{i-1}a_1$$

**定义，长为题目表面形式**

### 递推公式2

$$a_1=1,a_i=a_{i-1}*\frac{4n-6}{n}$$

**证明：转化成实际问题求得除递推公式1外另一个复杂度较高的递推公式，综合二公式求得递推公式2（$eg.$多边形三角剖分问题）**

### 组合公式 1

$$when\space i=1,a_i=1$$

$$when\space i>1,a_i=\frac{C_{2i-2}^{i-1}}{i}$$

**证明：由递推公式2用累积法求得**

### 组合公式 2

$$when\space i=1,a_i=1$$

$$when\space i>1,a_i=C_{2i-2}^{i-1}-C_{2i-2}^{i-2}$$

**证明：倒推可得到组合公式1**

