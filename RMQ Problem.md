# RMQ 问题及解决算法

## RMQ 问题

RMQ 问题，即区间最值查询，是在长度为 N 的序列中求出其连续的子序列中最大/最小值的问题。

## ST 算法

### 算法介绍

ST 算法适用于解决 RMQ 问题，是一个较长时间预处理，（时间复杂度为 O(NlogN)），在 O(1) 的时间内回答每个查询的算法。

### 算法思想及类型

算法思想为二分，将序列经过多次二分使最终区间为一个单位，类似线段树，本质为动态规划。

实现顺序为：

1. 区间大小为单位大小，计算出每个单位中最大/最小值（即为本身）

2. 逐渐增大区间大小（每次乘二），利用其两个子连续区间动态规划出这次计算的区间的最大/最小值

如图

![graph.png](./RMQ%20Problem/graph.png)

(Thanks to [Ferric](https://ferric.cf) and Graphviz)

### 代码实现

(from [https://www.topcoder.com/community/data-science/data-science-tutorials/range-minimum-query-and-lowest-common-ancestor/](https://www.topcoder.com/community/data-science/data-science-tutorials/range-minimum-query-and-lowest-common-ancestor/) )


```cpp
void process2(int M[MAXN][LOGMAXN], int A[MAXN], int N) {
    int i, j;
    // initialize M for the intervals with length 1
    // 将每个单位对应的最大/最小值都设为自身
    // 为上图的 1,2,3,4 的处理过程
    for (i = 0; i < N; i++)
        M[i][0] = i;
    // compute values from smaller to bigger intervals
    // 开始DP
    for (j = 1; 1 << j <= N; j++)
        for (i = 0; i + (1 << j) - 1 < N; i++)
            if (A[M[i][j - 1]] < A[M[i + (1 << (j - 1))][j - 1]])
                M[i][j] = M[i][j - 1];
            else
                M[i][j] = M[i + (1 << (j - 1))][j - 1];
}
```
### 参数

int \*\*M: 二维数组，用于接收返回结果。其中第一维为从 i 个数字开始，第二维为表示 [i,i+2^j]的区间，数组值为最大/最小值在A数组中的位置。

int \*A: 一位数组，为原数组。

int N: A的长度

### 求区间最值（O(1)）

当求 [a,b] 区间最值时，应找到小于 a 的 2^k（记 max1(min1)为 M[a][k]）和 比 b 小 2^k 的值（记为d）（记 max2(min2)为M[d][k]），再求出 ans=max(max1,max2)（或 ans=min(min1,min2)）

需要注意的是，上面求的 M[a][k] 和 M[d][k] 有可能有重叠部分，但是由于查询时间复杂度为 O(1) ，这里可以忽略

### 例题

#### [POJ 3264](http://poj.org/problem?id=3264)

 > 这道题是个奇葩题？（大雾）
 >
 > 在 OpenJudge上的 [这道题](http://hljssyzx.openjudge.cn/bitseg/1002/)（如链接不可用，百度搜全局题号2439）线段树可过，POJ 却过不了
 >
 > 几乎就是模板题？（大雾

updated on 2017/09/04

#### [Luogu 3379](https://www.luogu.org/problem/show?pid=3379)

> err...不太友好的一点是卡`vector`

其他的还好...直接上代码

```cpp
#include <bits/stdc++.h>
using namespace std;
#define MAXN 500001

int N,M,root;
vector<int>mapn[MAXN];
vector<int>dfs_list;
int dfn[MAXN];
bool vis[MAXN];
int first_pos[MAXN];
int last_pos[MAXN];
int defaultdfn=1;
int _M[MAXN][25];
int another_dfn[MAXN];


void process();
void process2();
int getmin(int a,int b);
void connect();
void dfs(int i);

int main()
{
    cin>>N>>M>>root;
    for(int i=0; i<N-1; ++i)
    {
        connect();
    }
    dfs(root);

    process();
    for(int i=0; i<M; ++i)
    {
        process2();
    }
}

void connect()
{
    int x,y;
    cin>>x>>y;
    mapn[x].push_back(y);
    mapn[y].push_back(x);
}

void dfs(int i)
{
    vis[i]=1;
    int vsize=mapn[i].size();
    dfn[i]=defaultdfn++;
    another_dfn[dfn[i]]=i;
    dfs_list.push_back(dfn[i]);
    first_pos[dfn[i]]=dfs_list.size()-1;
    for(int j=0; j<vsize; ++j)
    {
        int nextp=mapn[i][j];
        if(!vis[nextp])
        {
            dfs(nextp);
            dfs_list.push_back(dfn[i]);
        }
    }
    last_pos[dfn[i]]=dfs_list.size()-1;
}

void process2()
{
    int x,y;
    cin>>x>>y;
    x=dfn[x];
    y=dfn[y];
    if(x>y)
    {
        swap(x,y);
    }
    x=first_pos[x];y=last_pos[y];

    cout<<another_dfn[getmin(x,y)]<<endl;
}

int getmin(int a,int b)
{
    int k=log2(b-a+1);
    if((b-a+1)&(b-a))
    {
        int min1=dfs_list[_M[a][k]];
        int min2=dfs_list[_M[b-(1<<(k))][k]];
        return min1<min2?min1:min2;
    }
    else
    {
        return dfs_list[_M[a][k]];
    }
}

void process() {
    vector<int>&A=dfs_list;
    int N=A.size();
    int i, j;
    for (i = 0; i < N; i++)
        _M[i][0] = i;
    for (j = 1; 1 << j <= N; j++)
        for (i = 0; i + (1 << j) - 1 < N; i++)
            if (A[_M[i][j - 1]] < A[_M[i + (1 << (j - 1))][j - 1]])
                _M[i][j] = _M[i][j - 1];
            else
                _M[i][j] = _M[i + (1 << (j - 1))][j - 1];
}
```

