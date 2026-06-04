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

