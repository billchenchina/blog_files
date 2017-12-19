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

可以算算内存：`1e4 * 1e4 * 4 /( 1024 * 1024) = 381M`

然而多数题是`128M`内存的...这么做会爆炸...

怎么办呢？我们可以请出我们的`vector`

## vector 存边法

这个是除了上面的那个以外最好写的

先看看代码

```cpp
#include <bits/stdc++.h>

using namespace std;

const int MAXN = 1e4+10;
vector<int> mapn[MAXN];
int N,M,S;
// 加边
void addEdge(int a,int b)
{
    mapn[a].push_back(b);
}

// main()与上边一样，省略
```
### 有什么优点？

- 省内存。可以计算出这个程序存储图所用空间最大大致为`边数*4/(1024*1024)` 即1.9M左右

### 缺点呢？

和上边二维数组比没有缺点了吧...但是一些<del>毒瘤出题人</del>还是会卡这种做法 所以才有了下面的存图方法

(To be continued)
