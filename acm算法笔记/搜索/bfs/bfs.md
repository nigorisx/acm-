# BFS

[toc]

## 实现过程

- ***1、从初始状态 S 开始，利用规则，生成下一层的状态。***

- ***2、顺序检查下一层的所有状态，看是否出现目标状态 G。若没有出现，就对该层所有状态节点，分别利用规则，生成再下一层的所有状态节点。***

- ***3、继续按照上面的思想，这样一层层往下开展，直到出现目标状态为止。***

## 代码框架

  ·***数据结构**：队列存储节点，数组保存是否经过

```c++
#include <iostream>
#include <vector>
#include <queue>
#include <cstring>
using namespace std;
int dx[a]
int dy[b]//每一步x，y的变化值
struct node
{
    int x;
    int y;
};//坐标
queue<node>q;//队列保存节点
int vis[405][405];	
int n,m;
void bfs(int x0,int y0)
{
    memset(vis,0,sizeof(vis));
    memset(ma,-1,sizeof(ma));
    while(!q.empty()) q.pop();//initial
    q.push({x0,y0});
    vis[x0][y0]=1;//第一个入队，并标记
    while(!q.empty())
    {
        int nx=q.front().x;
        int ny=q.front().y;
        q.pop();//出队进行更新
        for(int i=0;i<8;i++)
        {
            int x1=nx+dx[i];
            int y1=ny+dy[i];
            if(x1<1||x1>n||y1<1||y1>m||vis[x1][y1]) continue;
            q.push({x1,y1});
            vis[x1][y1]=1;
        }
    }
}
```

## 常见问题

1.***连通块***（很常见）：***扫一遍bfs，每次将经过的点标记，最后有几次bfs即有几个连通块。有时会带有连通块的判断，如矩形连通块的判别，只要bfs记录特征点或特征坐标，在外部再判别即可***

2.***逆向过程：*** ***若多起点单终点，可以倒着跑bfs***

3.***抽象图***：***状态***是节点，状态间的转移操作是***边***，***在 BFS 中动态构造状态转移关系***

- 题面：给定两个整数 $a$ 和 $b$ ，您可以在每一轮中执行以下两个操作之一：

  - 如果是 $a>0$ ，则将 $a$ 的值减少 $\gcd(a, b)$ 。

  - 如果是 $b>0$ ，则将 $b$ 的值减少 $\gcd(a, b)$ 。


​	Grace 想知道使 $a$ 和 $b$ 都变为 $0$ 所需的最少轮数。

 ```c++
 #include <iostream>
 #include <queue>
 using namespace std;
 #define int long long
 int  gcd(int  a,int  b)
 {
     if(b==0) return a;
     else return gcd(b,a%b);
 }
 struct node
 {
     int x,y;
     int cnt;
 };
 int cnt=0;
 int  bfs(int a,int b)
 {
     queue<node>q;
     q.push({a,b,0});
     while(!q.empty())
     {
         int x1=q.front().x;
         int y1=q.front().y;
         if(x1>0)
         {
             q.push({x1-gcd(x1,y1),y1,q.front().cnt+1});
             cnt++;
         }
         if(y1>0)
         {
             q.push({x1,y1-gcd(x1,y1),q.front().cnt+1});
         }
         if(x1==0&&y1==0)
         {
             return q.front().cnt;
         }
         q.pop();
     }
 }
 void solve()
 {
     int a,b;
     cin>>a>>b;
     cout<<bfs(a,b) <<endl;
 }
 signed main()
 {
     int t;
     cin>>t;
     while(t--)
     {
         solve();
     }
 }
 ```



