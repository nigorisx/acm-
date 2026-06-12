# C++中 $\text{STL}$ 的简单应用

[toc]



## STL(standard template libaray -标准模板库)的介绍

- **是C++标准库的重要组成部分，不仅是一个可复用的组件库，而且是一个包罗数据结构与算法的软件框架，几乎所有C++编译器和所有操作系统平台都支持的一种库简单来说，是C++ 提供的一个高度优化的基础模板的集合。**

---

## STL核心组件

- **容器**作为STL的最主要组成部分，分为向量（vector），双端队列(deque)，表(list)，队列（queue），堆栈(stack)，集合(set)，多重集合(multiset)，映射(map)，多重映射(multimap)。

- **算法**主要由头文件<algorithm>， <numeric>和<functional>组成。<algorithm>是所有STL头文件中最大的一个，它是由一大堆模板函数组成的，可以认为每个函数在很大程度上都是独立的，其中常用到的功能范围涉及比较、交换、查找、遍历操作、复制、修改、移除、反转、排序、合并等等。<numeric>体积很小，只包括几个在序列上面进行简单数学运算的模板函数。<functional>则定义了一些模板类，用以声明函数对象。

- **迭代器(iterator)**：一旦选定一种容器类型和数据行为(算法)，那么剩下唯一要他做的就是用迭代器使其相互作用。可以把迭代器看作一个指向容器中元素的普通指针。可以如递增一个指针那样递增迭代器，使其依次指向容器中每一个后继的元素。迭代器是STL的一个关键部分，因为它将算法和容器连在一起。

---

## STL 容器概述

### 	1.序列容器

`vector、list、deque，适合线性数据存储。`

### 	2.关联容器

`map、set、multimap、multiset，基于红黑树，适合有序键值或集合操作。`	

### 3.无需关联容器

`unordered_map、unordered_set、unordered_multimap、unordered_multiset，基于哈希表，适合高效查找。`

### 4.容器适配器

`stack、queue、priority_queue，提供特定操作模式（如优先级）`

---

## 向量（vector)

### 1.vector的创建

首先，使用vector时需要调用头文件<vector>

```#include <vector>```

vector的本质是模板，可以存储任何类型的数据。

```c++
vector<int> v1;
vector<char> v2;
vector<string> v3;
```

### 2.vector的初始化

````c++
vector<int> v1;                           //一个空数组
vector<int> v2{1,2,3,4,5};                //创建5个变量
vector<int> v3(5);                        //开辟5个空间，默认值为0
vector<int> v4(5,1);                      //开辟5个空间，初始值为1
vector<int> v5(v4);                       //把v4的值复制给v5
```
````

==注意==：使用vector<int> v1来进行初始化时，v1里此时并没有任何值，直接访问会报错。可以用v1.assign(n,num)来进行初始化

如v1.assign(10,0)

### 3.vector的访问

（1）下标访问：类似于数组

（2）迭代器访问 类似指针一样的访问 ，首先需要声明迭代器变量，和声明指针变量一样

```c++
vector<int> v1{ 1,2,3,4,5,6,7 };
​    for (vector<int>::iterator it = v1.begin();it != v1.end();it++) {
​        cout << *it << ' ';
​    }
\\简化版
for (auto it = v1.begin();it != v1.end();it++)
{
    cout << *it << ' ';    
}  
for (auto it = v1.rbegin();it != v1.rend();it++) 
{
    //倒序遍历(后面的容器倒序遍历都可以这样类似写）       
    cout << *it << ' ';    
}    
```

### 4.vector的增删改查

```c++
v.front() //返回第一个数据 O（1）
v.back() //返回数组中的最后一个数据  O（1）
v.begin() //返回首元素的迭代器（通俗来说就是地址） O（1）
v.end() //返回最后一个元素后一个位置的迭代器（地址）O（1）              
v.rbegin() //返回逆序迭代器，指向容器元素最后一个位置  O(1) 
v.rend() //返回逆序迭代器，指向容器第一个元素前面的位置  O(1)
v.pop_back() //删除最后一个数据  O（1）
v.push_back(element) //在尾部加一个数据 O（1）
v.empty() //判断是否为空，为空返回真，反之返回假O（1）
v.size() //返回实际数据个数 O（1）
v.clear() //清除元素个数 ，O（N），N为元素个数
/*通过insert函数可以在所给迭代器位置插入一个或多个元素，通过erase函数可以删除所给迭代器位置的元素，或删除所给迭代器区间内的所有元素（左闭右开）。  都是O（N）*/
v.insert(it, x) 向任意迭代器 it 插入一个元素 x 
v.erase(first,last) 删除 [first,last) 的所有元素
例： v.insert(v.begin() + 2,-1) //将 -1 插入 c[2] 的位置 
v.insert(v.begin(),4,10);// 在容器开头插入4个10
v.erase(v.begin(), v.begin() + 4); 删除在该迭代器区间内的元素
```

注意:end() 返回的是最后一个元素的后一个位置的地址，不是最后一个元素的地址，rend() 返回的是第一个元素前面的位置，所有STL容器均是如此

### 5.vector的排序

```c++
//使用 sort 排序
sort(v.begin(), v.end());
//对所有元素进行排序，如果要对指定区间进行排序，可以对 sort() 里面的参数进行加减改动。
//例：		
    vector<int> a(n + 1);
 	sort(a.begin() + 1, a.end());  //对[1, n]区间进行从小到大排序
//从大到小排序可以写成
    sort(a.begin() + 1, a.end(),greater<int>());
	或者
    sort(a.rbegin(), a.rend());
```

## 栈（stack）
1.是STL中实现的一个先进后出，后进先出的容器。

2.栈的操作主要集中在栈顶，只允许在栈顶进行插入（入栈）和删除（出栈）操作。

3.常见的应用场景包括递归调用，括号匹配等。

4.在编程中常用的递归就是用栈来实现的。栈需要用空间存储，如果栈的深度太大，或者存进栈的数组太大，那么总数就会超过系统为栈分配的

空间，就会爆栈导致栈溢出。这是深度递归需要注意的主要问题。

### 1.栈的操作

```c++
//头文件需要添加 #include<stack>
stack<int> s; 	
stack<string> s; 
stack<node> s;node是结构体类型用法：
s.push(ele) //元素ele入栈，增加元素 
s.pop() //移除栈顶元素 
s.top() //取得栈顶元素（但不删除） 
s.empty() //检测栈内是否为空，空为真 
s.size() //返回栈内元素的个数时间
//复杂度都是O（1）
```

### 2.栈的应用

- 括号匹配（dp也是一种不错的方法），字符翻转
- 单调栈：

​	1. 单调栈不是一种新的栈结构，它在结构上仍然是一个普通的栈，它是栈的一种使用方式。

​	2. 单调栈内的元素是单调递增或递减的，有单调递增栈，有单调递减栈。

​	3.单调栈可以用来处理比较问题。

​	4.单调栈实际上就是普通的栈，只是在使用时始终保持栈内的元素是单调的。例如单调递减栈从栈顶到栈底是从小到大的顺序，当一个数入栈

​	时，与栈顶比较，若比栈顶小，则入栈；若比栈顶大，则弹出栈顶，直到这个数能入栈为止。

- 应用例题：[Look Up S](#https://www.luogu.com.cn/problem/P2947)

题干 **约翰的 *N*(1≤*N*≤105) 头奶牛站成一排，奶牛 *i* 的身高是 $H_i$(1≤$H_i$≤106)。现在，每只奶牛都在向右看。对于奶牛$i$，如果奶牛$j$ 满足 $i<j$ 且 $H_i<H_j$，我们可以说奶牛*i*可以仰望奶牛*j*。求出每只奶牛离她最近的仰望对象。**

思路：自左往右维护一个栈底大的单调栈，当当前元素大于栈顶时，由于站内是自底到顶是单调减少的，所以不存在仰望关系，所以可以出栈元素的仰望对象都是当前元素。

于是可以编写如下代码

```c++
#include <iostream>
#include <stack>
#include <vector>
using namespace std;
struct z
{
    int id;
    int v;
};//结构体存储奶牛的索引和高度，以防在栈内丢失索引
int main()
{
    int n;
    cin>>n;
    vector<int>f(n);
    stack<z>s;
    for(int i=0;i<n;i++)
    {
        int x;
        cin>>x;
        while(!s.empty()&&s.top().v<x)//循环处理出栈关系
        {
            f[s.top().id]=i+1;
            s.pop();
        }
        s.push({i,x});
    }
    while(!s.empty())
    {
        f[s.top().id]=0;
        s.pop();
    }
    for(int i=0;i<n;i++)
    {
        cout<<f[i]<<endl;
    }
}
//每个元素入栈最多入栈出栈一次，复杂度O（n)
```

## 队列（queue)

### 1.队列的操作

```c++
#include<queue> 
queue<int> q;
q.front() //返回队首元素
q.back() //返回队尾元素
q.push(element) //尾部添加一个元素 element进队
q.pop() //删除第一个元素出队
q.size() //返回队列中元素个数
q.empty() //判断是否为空，队列为空，返回 true
//时间复杂度都是O（1）
//双端队列deque类似
```

### 2.单调队列的应用

滑动窗口：

*** 题目描述：有一个长为 n 的序列 a，以及一个大小为 k 的窗口。现在这个窗口从左边开始向右滑动，每次滑动一个单位，求出每次滑动后窗口中的最小值和最大值。***

**思路**：***经典的滑动窗口使用单调队列来实现，时间复杂度为 O（n）。在本题中，以最小值为例维护一个这样的单调队列：双端队列单调递增，队头元素始终是滑动窗口内的最小值。元素只能从队尾进入队列，从队头队尾都可以弹出。（可以根据需要输出或弹出）***

具体过程如下：

**队头：检查队头元素是否还在窗口范围内，（通过和当前元素下标之差是否<k,确保队列里面的元素对应的是原始滑动窗口内对应的元素，即保证原始滑动窗口内的元素个数小于等于k），把不在范围内的及时弹出。**

**队尾：序列中的每个元素按顺序进入队列，进入队尾时和原队尾比较，如果原队尾大于该元素就将原队尾弹出（因为在窗口内排在当前元素前面且比当前元素值更大的元素，不可能还是窗口内的最小值），直到当前元素进队，此时队列内是以当前元素为尾的上升序列。（类似单调栈）最大值同理，维护一个单调递减的单调队列，队头元素始终是滑动窗口内的最大值。**

```c++
#include <iostream>
#include <deque>
#include <vector>
using namespace std;
struct el
{
    int v;
    int id;
};//同单调栈时的作用
int n,m;
vector<el> list;
void fmin()
{
    deque<el>q;
    for(int i=0;i<n;i++)
    {
        while(!q.empty() && i - q.front().id >= m)//队首元素一定是此时最早进入的元素
        {
            q.pop_front();//如果左侧窗
            口已经超过了当前队首位置，队首出队，维护窗口
        }
        while(!q.empty()&&q.back().v>list[i].v)
        {
            q.pop_back();//维护单调队列
        }
        q.push_back(list[i]);
        if(i>=m-1)
        {
            cout<<q.front().v<<" ";//满足称为固定长度的窗口条件才输出
        }
    }
    cout<<endl;
}
void fmax()//同fmin，反方向维护
{
    deque<el>q;
    for(int i=0;i<n;i++)
    {
        while(!q.empty() && i - q.front().id >= m)
        {
            q.pop_front();
        }
        while(!q.empty()&&q.back().v<list[i].v)
        {
            q.pop_back();
        }
        q.push_back(list[i]);
        if(i>=m-1)
        {
            cout<<q.front().v<<" ";
        }
    }
    cout<<endl;
}
int main()
{
    cin>>n>>m;
    list.resize(n);
    for(int i=0;i<n;i++)
    {
        int x;
        cin>>x;
        list[i]={x,i};
    }//initial初始化
    fmin();
    fmax();
}
```

