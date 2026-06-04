# DFS

​	

[toc]

## 1 核心思想和框架

```c++
type dfs（int u){
    //入
    //code
    	
    //递归
    for (int i=0；i<=n；i++)
    {
        //code
        int nxt；
       	dfs(nxt);
        
        //回溯
        //code
    }
    
    //离
    //code
    return(type)
}

```

### eg1.斐波那契数列

```c++
int dfs(int x)
{
    if(x==1||x==2)return 1;
    int res=0;
    res+=dfs(x-1);
    res+=dfs(x-2);
    return res;
}
```

### eg2.遍历求森林中最大的树高度

```c++
int dfs(int u, int len) {
	if (u ==-1)return len;// 如果当前节点不存在（-1表示空），则返回当前长度
	int res = dfs(p[u], len + 1);  // 否则递归到父节点，长度+1
	return res;
}
```

### eg3.二维图遍历

```c++
#include <iostream>
#include <vector>
using namespace std;
int n,m;
int ma[100][100];
bool flag=false;
int nx[4]={0,0,-1,1};
int ny[4]={1,-1,0,0};
bool dfs(int x,int y)
{
    if(x==n && y==m)
    {
        return true;
    }
    ma[x][y]=1;
    for(int i=0;i<4;i++)
    {
        int x1=x+nx[i];
        int y1=y+ny[i];
        if(x1<1||x1>n||y1<1||y1>m)continue;
        if(ma[x1][y1])continue;
        if(dfs(x1,y1))return true;
    }
    return false;
}
int main()
{
    cin>>n>>m;
    for(int i=1;i<=n;i++)
    {
        for(int j=1;j<=m;j++)
        {
            char x;
            cin>>x;
            if(x=='#')ma[i][j]=1;
        }
    }
    if(dfs(1,1))cout<<"Yes";
    else cout<<"No";
}
//只需添加一个数组p去添加路径节点，即可实现路径的保存
//但dfs完之后要对节点进行回溯，即将当前路径中末端节点pop
```



## 2.DFS优化

### 记忆化优化：

因为在**dfs中的更新过程是向上更新的**，在计算$\text{mem[x][y]}$时,$\text{mem[nx][ny]}$是已知的,（nx，ny）是（x,y)在树中的下层节点，在递归时可能是已知的。通过记忆化可以避免重复计算例如路径数之类的问题。

```c++
#include <iostream>
#include <vector>
#define mod 100003
using namespace std;
int n,m;
int nx[2]={0,1};
int ny[2]={1,0};
int mem[1005][1005];
int vis[1005][1005];
int ma[1005][1005];
int dfs(int x,int y)
{
    if(x==n&&y==n)
    {
        mem[x][y]=1;
        return 1;
    }
    vis[x][y]=1;
    for(int i=0;i<2;i++)
    {
        int x1=x+nx[i];
        int y1=y+ny[i];
        if(x1<1||x1>n||y1<1||y1>n)continue;
        if(ma[x1][y1]==1)continue;
        if(vis[x1][y1])
        {
            mem[x][y]+=mem[x1][y1]%mod;
            continue;
        }
        mem[x][y]+=dfs(x1,y1);
        mem[x][y]%=mod;
    }
    return mem[x][y]%mod;
}
int main()
{
    cin>>n>>m;
    for(int i=0;i<m;i++)
    {
        int x,y;
        cin>>x>>y;
        ma[x][y]=1;
    }
    cout<<dfs(1,1);
}
```

## 3.DFS变种

1.**广义走迷宫**

**也许我们每一次走的是单纯的往上下左右四个方向走，有可能是x,y移动距离是一个范围，可以用两层循环来遍历每一步x，y两个方向可以进行的移动。**

2.**变量保存**

**我们的变量在过程中需要递归更改，但我们只需它的一个特定状态，例如最大值，那我们可以引入全局变量去保存，并将该变量丢入dfs的参数之中**

```c++
//排地雷 地雷分布在多个地窖之间 地窖间有联通关系
#include <iostream>
#include <deque>
using namespace std; 
int va[25];int ma[25][25];
int vis[25];
int n;int res=-1;
int sum=0;
deque<int>q;
deque<int>p;
int dfs(int u,int sum)
{
   sum+=va[u];
    q.push_back(u);
   if(sum>res)
   {
    res=sum;//res的作用是为了保证路径的完整性，如果直接递归，会只记录一个节点。
    p=q;
   }
   int maxn=sum;
   for(int i=u+1;i<=n;i++)
   {
        if(ma[u][i]==1)
        {
            int t=dfs(i,sum);//从当前节点出发能获得的最大权重
            if(t>maxn)maxn=t;
        }
   }
   q.pop_back();
   return maxn;
}
int main()
{
    cin>>n;
    for(int i=1;i<=n;i++)
    {
        cin>>va[i];
    }
    for(int i=1;i<=n;i++)
    {
        for(int j=i+1;j<=n;j++)
        {
            cin>>ma[i][j];
        }
    }
    int ans=-1;
    for(int i=1;i<=n;i++)
    {
        int sum=0;
        dfs(i,0);
        if(res>ans)//res储存最值
        {
            ans=max(ans,res);
        }
    }
    for(int i=0;i<p.size();i++)
    {
        if(i!=p.size()-1)
        cout<<p[i]<<" ";
        else 
        cout<<p[i];
    }
    cout<<endl;
    cout<<ans;
}
```

