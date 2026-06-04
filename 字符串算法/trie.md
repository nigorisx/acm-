# 字典树（trie）

[toc]

##  字典树的定义

***字典树，英文名 trie．顾名思义，就是一个像字典一样的树．***

形态如下

> [!note]
>
> ![trie1](https://oi-wiki.org/string/images/trie1.png)

***可以发现，这棵字典树用边来代表字母，而从根结点到树上某一结点的路径就代表了一个字符串．***

## Trie的创建



> [!note]
>
> 我们用二维$nex[p][c]$数组保存节点p通过c链接的节点，exist[p]保存是否有以p结尾的字符串
>
> ```c++
> #include <bits/stdc++.h>
> using namespace std;
> 
> class trie {
> private:
>     static const int MAXN = 100000;
>     static const int ALPHABET = 26;
>     
>     int nex[MAXN][ALPHABET];
>     int cnt;
>     bool exist[MAXN];
>     
> public:
>     // 构造函数：初始化所有成员
>     trie() {
>         cnt = 0;
>         memset(nex, 0, sizeof(nex));
>         memset(exist, 0, sizeof(exist));
>     }
>     
>     // 插入字符串（不需要传长度）
>     void insert(string s) {
>         int p = 0;
>         for (int i = 0; i < s.length(); i++) {
>             int c = s[i] - 'a';
>             if (!nex[p][c]) {
>                 nex[p][c] = ++cnt;
>             }
>             p = nex[p][c];
>         }
>         exist[p] = true;
>     }
>     
>     // 查找字符串
>     bool find(string s) {
>         int p = 0;
>         for (int i = 0; i < s.length(); i++) {
>             int c = s[i] - 'a';
>             if (!nex[p][c]) {
>                 return false;  // 明确返回false
>             }
>             p = nex[p][c];
>         }
>         return exist[p];
>     }
>     
>     // 可选：清空Trie
>     void clear() {
>         cnt = 0;
>         memset(nex, 0, sizeof(nex));
>         memset(exist, 0, sizeof(exist));
>     }
> };
> ```
>
> 



