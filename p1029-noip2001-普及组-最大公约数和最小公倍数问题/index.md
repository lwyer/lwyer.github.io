# P1029 [NOIP2001 普及组] 最大公约数和最小公倍数问题


[P1029 [NOIP2001 普及组\] 最大公约数和最小公倍数问题 - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/P1029)

# [NOIP2001 普及组] 最大公约数和最小公倍数问题

## 题目描述

输入两个正整数 $x_0, y_0$，求出满足下列条件的 $P, Q$ 的个数：

1. $P,Q$ 是正整数。

2. 要求 $P, Q$ 以 $x_0$ 为最大公约数，以 $y_0$ 为最小公倍数。

试求：满足条件的所有可能的 $P, Q$ 的个数。

## 输入格式

一行两个正整数 $x_0, y_0$。

## 输出格式

一行一个数，表示求出满足条件的 $P, Q$ 的个数。

## 样例 #1

### 样例输入 #1

```
3 60
```

### 样例输出 #1

```
4
```

## 提示

$P,Q$ 有 $4$ 种：

1. $3, 60$。
2. $15, 12$。
3. $12, 15$。
4. $60, 3$。

对于 $100\%$ 的数据，$2 \le x_0, y_0 \le {10}^5$。

**【题目来源】**

NOIP 2001 普及组第二题



---

### 笔记

设 $a = x_a \times gcd(a,b) ,  b = x_b \times gcd(a,b)$ 

此时$x_a,x_b$互质

可以推出 $lcm(a,b) = x_a \times x_b \times gcd(a,b)$

也就可以得到 $a \times b=gcd(a,b) \times lcm(a,b)$

回到题目 $gcd(a,b),lcm(a,b)$已知，求$a,b$的个数

#### 法一

利用上式，枚举a，求满足条件的b，由于需要枚举到$\sqrt{gcd(a,b) \times lcm(a,b)}$，故复杂度为O（$n$）

#### 法二

利用$x_a,x_b$互质的关系，

对$lcm(a,b) \div gcd(a,b)$分解质因数，由于$x_a,x_b$互质，分解出来的重复的质因数只能分给$x_a$ 或 $x_b$，那么每遇到一个新的质因数，将ans乘2（因为有两种选择），注意ans初始化为1，也能解此题。由于分解质因数朴素算法的复杂度为O（$\sqrt{n}$)，故此法复杂度为O($\sqrt{n}$)

利用[分解质因数 - OI Wiki (oi-wiki.org)](https://oi-wiki.org/math/number-theory/pollard-rho/#pollard-rho-算法)中介绍的Pollard Rho 算法可优化至O($n^{1 \over 4}$)

### 参考代码

```
#include<bits/stdc++.h>
#define pi acos(1.0)
#define e exp(-1.0)
using namespace std;
typedef long long LL;
LL gcd(LL x, LL y) { // 不用比较大小
    if(y == 0) return x;
    return gcd(y, x % y);
}
int main() {
    LL x0, y0;
    cin >> x0 >> y0;
    int ans = 0;
    // /* 法一 */
    // for(int i = 1; i <= sqrt(x0 * y0); i ++) {
    //     if((x0 * y0) % i == 0) {
    //         int gcd_xy = gcd(i, x0 * y0 / i);
    //         if((gcd_xy == x0) && 1 == 1)
    //         {
    //             if(i == x0 * y0 / i) {
    //                 ans ++; // 如果PQ相同则满足条件的情况只有一种
    //             }
    //             else {
    //                 ans += 2;
    //             } 
    //         }
    //     }
    // }
    /* 法二 */
    if(y0 % x0 != 0) {
        ans = 0;
    }
    else {
        int p = y0 / x0;
        int cnt = 0;
        for(int i = 2; i <= sqrt(p); i ++) {
            if(p % i == 0)
            {
                cnt ++;
                while(p % i == 0) {
                    p /= i;
                }
            } 
        }   
        if(p != 1) cnt ++; // 除剩下的质因数个数要加上
        ans = 1 << cnt;
    }
    cout << ans;
    return 0;
}

```

### 后记

法一  
![image-20231102013607290](https://lwyer.linkpc.net/blog/2023/11/02/image-20231102013607290.png)

法二  
![image-20231102013557665](https://lwyer.linkpc.net/blog/2023/11/02/image-20231102013557665.png)

可以看到法二确实比法一略快一些


