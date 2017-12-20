> 退役了没事做的 billchenchina 突然想写新手教程 hhh

> 这篇文章主要讲信息学竞赛中图论题目如何输入

信息学竞赛中很多图论的输入文件是这样的

```
第一行包含三个整数N、M、S，分别表示点的个数、有向边的个数、出发点的编号。
接下来M行每行包含三个整数Fi、Gi、Wi，分别表示第i条有向边的出发点、目标点和长度。
```
（文字来源：[Luogu](https://www.luogu.org/problemnew/show/3371)）

这时你会想，怎么输入呢？

## 二维数组？

这是初学者的第一想法，二维数组的确是最好想的

```cpp
#include <bits/stdc++.h>
using namespace std;

const int MAXN = 1e4+10;

int mapn[MAXN][MAXN];

int N,M,S;

void addEdge(int a,int b,int weight)
{
    mapn[a][b]=weight;
}

int main()
{
    cin>>N>>M>>S;
    for(int i=1; i<=M; i++)
    {
        int u,v,w;
        cin>>u>>v>>w;
        addEdge(u,v,w);
    }
}
```

### 有什么优点？

貌似除了好想&好写以外没什么优点了...

### 缺点呢？

不能高效的从某一个点往其它点延伸，因为想要延伸的话需要把所有点都扫一遍，看看相不相连。

而且可以算算内存：`1e4 * 1e4 * 4 /( 1024 * 1024) = 381M`

然而多数题是`128M`内存的...这么做会爆炸...

怎么办呢？我们可以请出我们的`vector`

## vector 存边法

这个是除了上面的那个以外最好写的

先看看代码

```cpp
#include <bits/stdc++.h>

using namespace std;

const int MAXN = 1e4+10;

struct Edge
{
    int from;
    int to;
    int weight;
    Edge(int f,int t,int w)
    {
        this->from=f;
        this->to=t;
        this->weight=w;
    }
    Edge(){}
};

vector<Edge> mapn[MAXN];
int N,M,S;
// 加边
void addEdge(int a,int b,int w)
{
    mapn[a].push_back(Edge(a,b,w));
}

// main()与上边一样，省略
```

### 什么意思？没看懂？

对于图中的每一个点建立一个vector，将每一条以这个点为出口的边都放到这个vector里。

### 有什么优点？

- 便于遍历。想从某一个点出发遍历他所连着的点只需要把这个点的vector遍历一遍即可。

- 省内存。可以计算出这个程序存储图所用空间最大大致为`边数*12/(1024*1024)` 即6M左右

### 缺点呢？

和上边二维数组比没有缺点了吧...但是一些<del>毒瘤出题人</del>还是会卡这种做法（具体能被卡的原因是vector是动态数组，很慢） 所以才有了下面的存图方法

### 能改进吗？

首先，在使用中我们发现`struct Edge`里的`from`字段通常是用不到的，删。（节省了33%空间）

再看看缺点。动态数组慢？为什么？

动态数组是动态申请空间，你可以粗略的认为在`vector`的`push_back`操作中会申请多次内存，而内存的申请&调用是很慢的，能不能提前申请出来？

貌似很难？毕竟如果对于每个vector都提前申请出来的话和二维数组貌似没啥区别。。。

换个思路，能不能把所有的边都存到一个数组？

## 链式前向星

这个是一种“奇淫技巧” OIer用了都说好（大雾）

先看看代码实现

```cpp

const int MAXM = 5e5 + 10;
const int MAXN = 1e4 + 10;
struct Edge
{
    int to;
    int next;
}edges[MAXM];

int first[MAXN];

int edges_stamp=0;

// 需要在main()中 memset(first,-1,sizeof(first));

void addedge(int a,int b)
{
    edges[edges_stamp].to=b;
    edges[edges_stamp].next=first[a];
    first[a]=edges_stamp;
    edges_stamp++;
}
```
