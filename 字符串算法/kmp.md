# kmp算法

[toc]

## 前置知识

### 前缀函数

***给定一个长度为$n$的字符串s，其前缀函数定义为一个长度为n的数组 $\pi$，其中$\pi[i]$定义是：***

1.***$\text{如果字符串}s[0\dots i]有一段相等的真前缀和真后缀：s[0\dots k]和s[i-（k+1）\dots i]，那么\pi[i]就是这个相等的真前缀的长度，就是\pi[i]=k$***

2.***$如果不止一对，那么就保存最大值$***

3.***不存在 则为0***

#### 计算前缀函数的朴素算法

对于每个位置 `i`，从最长的可能长度开始尝试，检查前缀和后缀是否相等。

```c++
// 注：
// string substr (size_t pos = 0, size_t len = npos) const;
vector<int> prefix_function(string s) {
  int n = (int)s.length();
  vector<int> pi(n);
  for (int i = 1; i < n; i++)
    for (int j = i; j >= 0; j--)
      if (s.substr(0, j) == s.substr(i - j + 1, j)) {
        pi[i] = j;
        break;
      }
  return pi;
}//复杂度O(n²) 
```

#### 高效算法

***优化1，相邻的前缀函数要么增加一，要么不变，要么减少***

```c++
vector<int> prefix_function(string s) {
  int n = (int)s.length();
  vector<int> pi(n);
  for (int i = 1; i < n; i++)
    for (int j = pi[i - 1] + 1; j >= 0; j--)  // improved: j=i => j=pi[i-1]+1
      if (s.substr(0, j) == s.substr(i - j + 1, j)) {
        pi[i] = j;
        break;
      }
  return pi;
}//复杂度O(n²)
```

*kmp算法*

```c++
vector<int> prefix_function(string s) {
  int n = (int)s.length();
  vector<int> pi(n);
  for (int i = 1; i < n; i++) {
    int j = pi[i - 1];
    while (j > 0 && s[i] != s[j]) j = pi[j - 1];
    if (s[i] == s[j]) j++;
    pi[i] = j;
  }
  return pi;
}
```

完整代码

```c++
#include<bits/stdc++.h>
using namespace std;
#define int long long
namespace SOL
{
    using ll = long long;
    using pii = pair<int, int>;
    using vi = vector<int>;
    using vvi = vector<vector<int>>;
    const int INF = 0x3f3f3f3f;
    const ll LLINF = 0x3f3f3f3f3f3f3f3fLL;
    template<typename T, typename L>
    bool chkmax(T &x, L y) { if (x < y) return x = y, true; return false; }
    template<typename T, typename L>
    bool chkmin(T &x, L y) { if (y < x) return x = y, true; return false; }
    void solve()
    {
        string s1,s2;		
        cin>>s1>>s2;
        vi pmt(s2.size());
        pmt[0]=0;
        for (int i=1,j=0;i<s2.size();i++) {
            while (j&&s2[i]!=s2[j])j=pmt[j-1];
            if (s2[i]==s2[j])j++;
            pmt[i]=j;
        }
        vi ans;
        for (int i=0,j=0;i<s1.size();i++) {
            while (j&&s1[i]!=s2[j])j=pmt[j-1];
            if (s1[i]==s2[j])j++;
            if (j==s2.size()) {
                ans.push_back(i-j+2);
                j=pmt[j-1];
            }
        }
        for (auto t:ans)cout<<t<<"\n";//s2在s1出现的位置
        for (int i=0;i<s2.size();i++) {
            cout<<pmt[i]<<" ";
        }
    }
}
signed main()
{
    ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);
    int t;
    t=1;
    while(t--)
    {
        SOL::solve();
    }
}
```

