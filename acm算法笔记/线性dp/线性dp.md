# 线性dp

[toc]

## eg1.最长严格升序子序列

***状态转移方程***
$$
\begin{align}
\text{dp[i]}=\max(\text{dp[j]}+1,\text{dp[i]})\quad if(nums[i]>nums[j])
\end{align}
$$

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#define int long long
using namespace std;
void solve()
{
    int n;
    cin>>n;
    vector<int>a(n+1,0);
    vector<int>dp(n+1,0);
    for(int i=1;i<=n;i++)
    {
        cin>>a[i];
    }
    for(int i=0;i<=n;i++)
    {
        for(int j=i+1;j<=n;j++)
        {
            if(a[i]<a[j])dp[j]=max(dp[i]+1,dp[j]);
        }
    }
    int maxn=-1;
    for(int i=1;i<=n;i++)
    {
        maxn=max(maxn,dp[i]);
    }
    cout<<maxn;
}
signed main()
{
    int t;
    t=1;
    while(t--)
    {
        solve();
    }
}
```

## eg2.最长有效括号

$$

$$

$$
\left \{\begin{array}{ll}
dp[i]=0\quad s[i]=( \\
dp[i]=dp[i-2]+2\quad s[i]=)且s[i-1]=( \\
dp[i] = dp[i-1] + dp[i - 1 - dp[i-1] - 1] + 2\quad s[i-1] == )\, 且\,s[i - 1 - dp[i-1]] == (且s[i]=)
\end{array}\right.
$$

```c++
#include <iostream>
#include <string>
#include <algorithm>
using namespace std;
int f[100000];
int main()
{
   string s;
   cin>>s;
   s='0'+s;int ans=-1;
   for(int i=0;i<s.size();i++)
   {
    if(s[i]=='('||s[i]=='['||s[i]=='{')continue;
    else if(s[i]==')'&&s[i-1-f[i-1]]=='('||s[i]==']'&&s[i-1-f[i-1]]=='['||s[i]=='}'&&s[i-1-f[i-1]]=='{')
    f[i]=f[i-1]+2+f[i-1-f[i-1]-1];
    ans=max(ans,f[i]);
   }
   cout<<ans;
}
```

## eg3.最长公共子序列

$\text{dp[i][j]}$定义为text1$\text{[:i]}$和text2$\text{[:j]}$的最长公共子序列
$$
\left \{\begin{array}{ll}
dp[i][j]=dp[i-1][j-1]+1\quad text1[i]==text2[j]\\
dp[i][j]=max(dp[i-1][j]，dp[i][j-1])\quad text1[i]!=text2[j]
\end{array}\right.
$$

```c++
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int m = text1.length(), n = text2.length();
        int[][] dp = new int[m+1][n+1];
        for (int i = 1; i <= m; i++) {
            char c1 = text1.charAt(i - 1);
            for (int j = 1; j <= n; j++) {
                if (c1 == text2.charAt(j - 1)) {
                    dp[i][j] = dp[i-1][j-1] + 1;
                } else {
                    dp[i][j] = Math.max(dp[i-1][j], dp[i][j-1]);
                }
            }
        }
        return dp[m][n];
    }
}

```

## eg4.交错字符串问题

## 问题定义

给定三个字符串 `s1`、`s2` 和 `s3`，判断 `s3` 是否由 `s1` 和 `s2` **交错**组成。

**定义**: `dp[i][j]` 表示 `s1` 的前 `i` 个字符和 `s2` 的前 `j` 个字符能否交错组成 `s3` 的前 `i+j` 个字符。

```c++
bool isInterleave(string s1, string s2, string s3) {
    int m = s1.length(), n = s2.length(), t = s3.length();
    if (m + n != t) return false;
    
    vector<vector<bool>> dp(m + 1, vector<bool>(n + 1, false));
    dp[0][0] = true;
    
    // 初始化：只使用s1
    for (int i = 1; i <= m; i++) {
        dp[i][0] = dp[i-1][0] && (s1[i-1] == s3[i-1]);
    }
    
    // 初始化：只使用s2
    for (int j = 1; j <= n; j++) {
        dp[0][j] = dp[0][j-1] && (s2[j-1] == s3[j-1]);
    }
    
    // 填充DP表
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            // 如果s1的第i个字符匹配s3的第(i+j)个字符
            if (dp[i-1][j] && s1[i-1] == s3[i+j-1]) {
                dp[i][j] = true;
            }
            // 如果s2的第j个字符匹配s3的第(i+j)个字符
            if (dp[i][j-1] && s2[j-1] == s3[i+j-1]) {
                dp[i][j] = true;
            }
        }
    }
    
    return dp[m][n];
}
```

## eg5.编辑距离


 题目描述

设 $A$ 和 $B$ 是两个字符串。我们要用最少的字符操作次数，将字符串 $A$ 转换为字符串 $B$。这里所说的字符操作共有三种：

1. 删除一个字符；
2. 插入一个字符；
3. 将一个字符改为另一个字符。

$A, B$ 均只包含小写字母

```c++
#include <iostream>
#include <vector>
#include <algorithm>
#define int long long
using namespace std;
void solve()
{
    string s1;string s2;
    cin>>s1>>s2;
    int len1=s1.size();
    int len2=s2.size();
    vector<vector<int>>dp(len1+1,vector<int>(len2+1));
    for(int i=0;i<=len1;i++)
    {
        dp[i][0]=i;
    }
    for(int i=0;i<=len2;i++)
    {
        dp[0][i]=i;
    }
    for(int i=1;i<=len1;i++)
    {
        for(int j=1;j<=len2;j++)
        {
            if(s1[i-1]==s2[j-1])dp[i][j]=dp[i-1][j-1];
            else dp[i][j]=min(min(dp[i-1][j],dp[i][j-1]),dp[i-1][j-1])+1;
        }  
    }
    cout<<dp[len1][len2];
}
signed main()
{
    int t;
    t=1;
    while(t--)
    {
        solve();
    }
}
```

## eg.6 数的划分（花的摆放）二维dp（方案数）

`dp[i][j]` 表示将整数 `j` 划分为恰好 `i` 个正整数的划分数。

```c++
dp[i][j] = dp[i-1][j-1] + dp[i][j-i]
```

$$
\begin{align}
&dp[ i ][ j ]表示将整数 i 划分为 j 份 的方案数。\\
&首先，如果拿到一个整数 i ，因为题目中要求每份不能为空，因此必须先拿出 j 个数位将 j 份分别放上1，此时剩下 i - j个数。\\
&那么剩下的数如何处理呢？可以将其全部分到一份当中（dp[ i-j ][1]），也可以分到两份中(dp[ i-j ][2])，也可以分到 j 份中(dp[ i-j ][ j ])\\
&将每一种分法全部加起来，和即为dp[ i ][ j ]。\\
\end{align}
$$

 ```c++
 dp[i][j]=dp[1][j-i]+dp[2][j-i]+...+dp[i][j-i]
 dp[i-1][j-1]=dp[1][j-i]+dp[2][j-i]+...+dp[i-1][j-i]
 //所以
 dp[i][j] = dp[i-1][j-1] + dp[i][j-i]
 ```

```c++
//摆花 每种花的数量不能超过a[i]朵
//dp[i][j]表示到第i种花时一共摆了j朵
#include <iostream>
#include <vector>
#include <algorithm>
#define int long long
using namespace std;
const int mod=1e6+7;
void solve()
{
    int n,m;
    cin>>n>>m;
    vector<int>a(n+1);
    for(int i=1;i<=n;i++)
    {
        cin>>a[i];
    }
    vector< vector<int> >v(n+1,vector<int>(m+1,0));
    v[0][0]=1;
    for(int i=1;i<=n;i++)
    {
        for(int j=0;j<=m;j++)
        {
            for(int k=0;k<=min(a[i],j);k++)
            {
                v[i][j]+=v[i-1][j-k]%mod;
            }
        }
    }
    cout<<v[n][m]%mod;
}
signed main()
{
    int t;
    t=1;
    while(t--)
    {
        solve();
    }
}
```

