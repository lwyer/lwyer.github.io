# Codeforces Round 871 (Div. 4)小结


[contest link](https://codeforces.com/contest/1829)

### A

#### 题意

给一个10个字母的字符串，问和codeforces相比有几个字母不同

#### 解析

逐字比较即可



### B

#### 题意

给一个二进制数组，问最长的连续的0是多少

#### 解析

枚举寻找



### C

#### 题意

一个人可以花时间读【00，01，10，11】四种书，问达成【11】所需要的最少时间是多少？其中【11】既可以通过直接读【11】得到，也可以通过分别读【01，10】得到。

#### 解析

分别枚举两种方式的最少时间，取最小值。

#### 

### D

#### 题意

设一块金子重量是3x，这块金子能够以分成x和2x的方式被切割，比如6可以被切割为2和4。给定一个n和m，问重为n的金子能否通过切割得到重为m的金子？

####  解析

根据题意dfs搜索答案即可，时间复杂度为O($n^{log_23} \times t$)

``` cpp
#include<bits/stdc++.h>
#define pi acos(1.0)
#define e exp(-1.0)
using namespace std;
typedef long long LL;
bool solve(int n, int m)
{
    if(n == m) return true;
    else
    {
        if(n % 3 == 0)
        {
            return (solve(n / 3, m) || solve(n / 3 * 2, m));
        }
        else
        {
            return false;
        }
    }
}
int main()
{
    int t;
    cin >> t;
    while (t --)
    {
        int n, m;
        cin >> n >> m;
        if(solve(n, m)) cout << "yes";
        else cout << "no";
        cout << endl;
    }
    return 0;
    
}
```



### E

#### 题意

给一个n*m的数组，每个数字表示湖的深度，深度为0的地方代表陆地，问最大容量（所有深度之和）的湖有多大？

#### 解析

用bfs找一下，被访问到的就标记一下，累加一下答案，然后比较最大值即可。

小结一下pair的使用，在一些二元组的场合还是挺方便的。

- 定义 

  ``` cpp
  pair<int,int> p;
  ```

  - 嵌套定义

    ```cpp
    queue<pair<int, int>> q;
    ```

    

- 生成

  ```cpp
  make_pair(x,y);
  ```

- 使用

  ```cpp
  p.first // 第一个变量
  p.second // 第二个变量
  ```

  

``` cpp
#include<bits/stdc++.h>
#define pi acos(1.0)
#define e exp(-1.0)
using namespace std;
typedef long long LL;
const int x[4] = {0,0,-1,1};
const int y[4] = {1,-1,0,0};
int a[1010][1010];
bool b[1010][1010];
int solve()
{
    int n, m;
    cin >> n >> m;

    for(int i = 1; i <= n; i ++)
    {
        for(int j = 1; j <= m; j ++)
        {
            cin >> a[i][j];
            if(a[i][j] != 0) b[i][j] = true;
            else b[i][j] = false;
        }
    }
    queue<pair<int, int>> q;
    int ans = 0;
    for(int i = 1; i <= n; i ++)
    {
        for(int j = 1; j <= m; j ++)
        {
            if(!b[i][j]) continue;
            
            int now = 0;
            q.push(make_pair(i, j));
            b[i][j] = false;
            while(!q.empty())
            {
                pair<int, int> p;
                p = q.front();
                now += a[p.first][p.second];
                q.pop();
                for(int k = 0; k < 4; k ++)
                {
                    int x1 = p.first + x[k]; // 这里i，j弄错了
                    int y1 = p.second + y[k]; // 这里first，second弄错了
                    if(x1 >=1 && x1 <= n && y1 >=1 && y1 <= m && b[x1][y1])
                    {
                        q.push(make_pair(x1, y1));
                        b[x1][y1] = false;
                    }
                }
            }
            ans = max(ans, now);
        }
    }
    return ans;
}
int main()
{
    int t;
    cin >> t;
    while(t --)
    {
        cout << solve() << endl;
    }
    return 0;
}
```



### F

#### 题意

有一棵树，这棵树只有三层节点，其中父节点只有一个，子结点有x个，孙子结点有y个，给你每个点之间的连接关系，求x和y的值

#### 解析

注意到孙子结点的度只有1，找到度为1的点，然后遍历上去求子节点和父节点的度即可。

虽然父节点的度也可能为1，但其实不影响答案。因为当父节点度为1时，树翻转过来的结构也还是一样的。

``` cpp
#include<bits/stdc++.h>
#define pi acos(1.0)
#define e exp(-1.0)
using namespace std;
typedef long long LL;
int x,y;

int solve()
{
    int n, m;
    cin >> n >> m;
    vector<int> a[n + 10];
    int num[n + 10];
    memset(num, 0 ,sizeof(num));
    for(int i = 1; i <= m; i ++)
    {
        int u,v;
        cin >> u >> v;
        a[u].push_back(v);
        a[v].push_back(u);
        num[u] ++;
        num[v] ++;
    }
    for(int i = 1; i <= n; i ++)
    {
        if(num[i] == 1)
        {
            for(int j = 0; j < a[i].size(); j ++)
            {
                if(num[a[i][j]] != 1)
                {
                    y = num[a[i][j]] - 1;
                    for(int k = 0; k < a[a[i][j]].size(); k ++)
                    {
                        if(num[a[a[i][j]][k]] != 1) 
                        {
                            x = num[a[a[i][j]][k]];
                        }
                        if(y == 0) x = 1;
                    }
                }
            }
            break;
        }
    }
}
int main()
{
    int t;
    cin >> t;
    while(t --) 
    {
        x = 0, y = 0;
        solve();
        cout << x << " " << y << endl;
    }
    return 0;
}
```

看了一下官方题解的做法，也是相近的，就是计算每个点的度，度为x的点只有1个，度为y+1的点有x个，度为1的点有x*y个。

不过当x=y+1的时候有特殊情况。度为y+1的点有x个，度为1的点有x*y个。

根据这两种情况解二元方程就可以了。

### G

#### 题意

有一些罐头按序号从上到下堆成金字塔形，拿掉其中一个，在他上面的罐头都会倒塌，问拿掉那个罐头后倒塌的所有的序号的平方和时多少？

#### 解析

一开始我是一位搜索能够的，直接写了个搜索，结果T了，然后比赛时间也到了，就结束了。然后定睛一看O($n \times t$)已经到$10 ^ 9$的数量级了，一般情况超过了$10^8$就会超时，记错了。后面一看完全可以写个dp，状态转移方程也很简单，运用了一下容斥原理也可以得到。

dp [i，j] 表示推倒第i行第j个罐子所得到的序号平方和，状态转移方程为：

$$dp[i][j] = dp[i - 1][j] + dp[i - 1][j - 1] + a[i][j] * a[i][j] - dp[i - 2][j - 1]$$

之前写的搜索就是注释的那些，其实写记忆化搜索也可以减少重复查询，应该也能过，不过懒得写了。

```cpp
#include<bits/stdc++.h>
#define pi acos(1.0)
#define e exp(-1.0)
using namespace std;
typedef long long LL;
vector<LL> a[2100];
LL dp[2100][2100];
bool b[2100][2100];
// LL cal(int x, int y)
// {
//     if(!b[x][y]) return 0;
//     if(x < 1 || y >= a[x].size() || y < 0) return 0;
//     LL ans = a[x][y] * a[x][y];
//     b[x][y] = false; // 这个弄反了？
//     if(b[x - 1][y]) ans += cal(x - 1, y);
//     if(b[x - 1][y - 1]) ans += cal(x - 1, y - 1);
//     // 对于dfs，计算过程不写在return里方便debug
//     return ans;
// }

LL solve()
{
    int n;
    cin >> n;
    memset(b,true,sizeof(b));
    for(int i = 1; i <= 2023; i ++)
    {
        for(int j = 0; j < a[i].size(); j ++)
        {
            if(a[i][j] == n)
            {
                // return cal(i ,j);
                return dp[i][j];
            }
        }
    }
}

int main()
{
    LL p1,p2;
    p1 = p2 = 0;
    for(int i = 1; i <= 2023; i ++)
    {
        p1 ++;
        for(int j = 0; j < p1; j ++)
        {
            p2 ++;
            a[i].push_back(p2);
        }
    }
    dp[1][0] = 1;
    for(int i = 2; i <= 2023; i ++)
    {
        dp[i][0] = dp[i - 1][0] + a[i][0] * a[i][0];
        for(int j = 1; j < a[i].size(); j ++)
        {
            dp[i][j] = dp[i - 1][j] + dp[i - 1][j - 1] + a[i][j] * a[i][j] - dp[i - 2][j - 1];
        }
    }
    int t;
    cin >> t;
    while(t --)
    {
        cout << solve() << endl;
    }
    return 0;
}
```



### H

这道题是赛后补上的，对于这种用到位运算的题一直没啥头绪，虽然题目意思很简单

#### 题意

给一个包含n个正整数的数组，这个数不超过63。可以任去一段子序列，对序列内的数进行AND（按位与），问运算后的二进制数刚好有k个1的方案有几种？

#### 解析

由于数组的数非常小，可以考虑用dp直接存每个数的方案数,然后再求和刚好有k个1的数的方案数。

dp[i,j]表示数组取前i个数能得到结果为j的方案数

那么分三种情况

1. 直接在第i个数开始一个新子序列，那么 $dp[i][a[i]] ++$
2. 我们不取第i个数做按位与，那么$dp[i][a[i]] += dp[i - 1][a[i]]$
3. 我们取第i个数做按位与，那么$dp[i][a[i] \& j] += dp[i - 1][j]$

```cpp
__builtin_popcount(mask) // 就是求这个数的二进制表达有多少个1的一个函数
```

一般碰到这种要mod大数字的题，我都会考虑采用long long，但看题解非常自信的没有用，每一处可能超的地方都做好了mod限制，这是我需要学习的地方。

```cpp
#include<bits/stdc++.h>
#define pi acos(1.0)
#define e exp(-1.0)
using namespace std;
typedef long long LL;
const int mod = 1e9 + 7;

int solve()
{
    int n, k;
    cin >> n >> k;
    vector<int> a(n + 1);
    vector<vector<int>> dp(n + 1, vector<int>(1 << 6, 0));
    for(int i = 1; i <= n; i ++)
    {
        cin >> a[i];
        for(int mask = 0; mask < (1 << 6); mask ++) 
        {
            dp[i][mask] += dp[i - 1][mask];
            if(dp[i][mask] >= mod) dp[i][mask] -= mod;
            dp[i][mask & a[i]] += dp[i - 1][mask];
            if(dp[i][mask & a[i]] >= mod) dp[i][mask & a[i]] -= mod;
        }
        dp[i][a[i]] = (dp[i][a[i]] + 1) % mod;
    }
    int ans = 0;
    for(int mask = 0; mask < (1 << 6); mask ++)
    {
        if(__builtin_popcount(mask) == k)
        {
            ans = (ans + dp[n][mask]) % mod;
        }
    }
    return ans;
}
int main()
{
    int t;
    cin >> t;
    while(t --)
    {
        cout << solve() << endl;
    }
    return 0;
}
```


