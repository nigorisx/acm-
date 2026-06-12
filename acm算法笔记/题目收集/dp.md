# problem

[toc]

## p1 Cow Roller Coaster S（类似二维费用背包dp）

**题目描述**

有 $n$ 条线段，每条线段只能放在数轴上的一个特定位置，并且第 $i$ 根线段有如下几个属性：$X_i$（该线段放在数轴上的起点），$W_i$（该线段长度），$F_i$（你若使用该线段你能获得的价值），$C_i$（你若使用该线段你所需要的费用）。现在让你从中选出一些线段，使得这些线段能够铺满数轴上的区间 $[0,L]$ 且线段必须首尾相接（也就是不能重叠，也不能空缺），并且所花费用和不超过 $B$，同时要使你收获的价值尽量大，请你找到这个方案。

**思路**

***可以把位置和成本看作背包问题中的约束条件，然后采用类似背包的状态转移方式来。这题和背包的区别在于，背包不用考虑物品进入的连续性，状态转移也不依赖于物品放在背包中的位置，所以只借用背包的转移思路 即取与不取 不采用背包dp。而是线性dp，状态转移方程为***

$$
dp[h[i].w][j+h[i].c]=max(dp[h[i].w][j+h[i].c],dp[h[i].x][j]+h[i].f)
$$

还要注意这题对可达性也有要求，所以dp初始化都为-1，转移时要考虑前一状态是否存在。即$dp[h[i].x][j]$是否为-1.

**代码**

```c++
#include <bits/stdc++.h>
using namespace std;
struct node
{
    int x,w,f,c;
    friend inline bool operator<(node a,node b)
    {
        return a.x<b.x;
    }//排序是的满足dp的无后效性要求。
};
int main()
{
    int l,n,b;
    cin>>l>>n>>b;
    vector<node>h(n+1);
    for(int i=1;i<=n;i++)
    {
        cin>>h[i].x>>h[i].w>>h[i].f>>h[i].c;
        h[i].w=h[i].x+h[i].w;//调整为结尾坐标
    }
    vector<vector<int>>dp(l+1,vector<int>(b+1,-1));
    sort(h.begin()+1,h.end());
    dp[0][0]=0;//initial
    for(int i=1;i<=n;i++)
    {
        for(int j=0;j+h[i].c<=b;j++)
        {
            if(dp[h[i].x][j]!=-1)
            {
                dp[h[i].w][j+h[i].c]=max(dp[h[i].w][j+h[i].c],dp[h[i].x][j]+h[i].f);//状态转移
            }
        }
    }
    int ans=-1;
    for(int i=1;i<=b;i++)
    {
        ans=max(ans,dp[l][i]);
    }
    if(ans!=0)cout<<ans;
    else cout<< -1;
}
```

## p2 Segments Cover(概率计算+dp)

***描述***：有一条直线，被分成了 **m** 个格子，编号从左到右依次是 **1** 到 **m**。

给你 **n** 段区间。每段区间由四个数字定义：**l**、**r**、**p** 和 **q**，这段区间覆盖了从 **l** 到 **r** 的格子（包含两端），并且这段区间存在的概率是 $\frac{p}{q}$（各段区间的存在情况相互独立）。

你的任务是算出，每个格子恰好被 **一段**区间覆盖的概率(mod 998244353)

> [!NOTE] 思路
>
> ***思路***
>
> 此题纯看题解还理解半天，这道题是一道概率和dp的混合考察题目。
>
> 难点1：如何规划dp
>
> > 这道题目要求每个格子仅被覆盖一次，又给的是区间，那么考虑去用区间的结尾和开头来代表区间，转换区间为点，那么只要找到一个点间的状态转移方程即可转移区间，dp[n]为到n为止满足题意的概率.还要以r排序保证dp的无后效性。
>
> 难点2：概率计算
>
> >假如有多个区间以同一节点结尾，那么概率如何算。显然，假如满足条件，任何一个事件都可以满足，意味这这些事件是互斥的，又因为互斥事件的总概率是累加的，不同结尾的区间是累乘的。循环时要遍历所有以i为结尾的区间
>
> 难点3：复杂度优化
>
> >如何使得dp的效率高一些呢，接下来就是此题的我觉得一种很巧妙的思路，令P=$\frac{p}{q}$,Q=$\frac{q-p}{q}$,一开始假设整个区间都没覆盖，概率为$\Pi_{1}^{n}Q$
> >
> >当区间i需要覆盖时只要在dp[i.l-1]上乘上$\frac{P_i}{Q_i}$
> >
> >复杂度约为O(nlogn+m)

***代码***

```c++
#include <bits/stdc++.h>
using namespace std;
#define int long long 
#define mod 998244353
const int N=2e5+5;
int dp[N];
int qpow(int a,int b)
{
    int ans=1;
    while(b)
    {
        if(b%2==1)ans=ans*a%mod;
        a=a*a%mod;
        b/=2;
    }
    return ans;
}
int inv(int a) { return qpow(a, mod - 2); }//快速幂和逆元为分数去模做准备
struct node
{
    int l,r,p,q;
    int val;
};
signed main()
{
    int n,m;
    cin>>n>>m;
    vector<node> v(n);
    int N0 = 1;
    for(int i=0;i<n;i++)
    {
        cin>>v[i].l>>v[i].r>>v[i].p>>v[i].q;
        int P = v[i].p * inv(v[i].q) % mod;  // P = p/q
        int Q = (1 - P + mod) % mod;  // Q = 1-p/q
        N0 = N0 * Q % mod;
        v[i].val = P * inv(Q) % mod;
    }
    sort(v.begin(), v.end(), [](node &a, node &b) {
        return a.r < b.r;
    });
    dp[0] = N0; 
    int j = 0;
    for(int i=1;i<=m;i++)
    {
        dp[i] = 0;//没有取到此结尾时，概率为零
        while(j < n && v[j].r == i)
        {
            int l = v[j].l;
            dp[i] = (dp[i] + dp[l-1] * v[j].val) % mod;//互斥事件，概率累加
            j++;
        }
    }
    cout << dp[m] % mod << endl;
    return 0;
}       
```

