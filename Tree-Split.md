# 树链剖分

自从上次写了个[树剖](http://www.lydsy.com/JudgeOnline/problem.php?id=1036)，最近什么都没写出来。。。

感觉自己 NOIP 就要退役了。。。

先补个坑吧。。。

# 构造

树剖其实是个挺有趣的方法，理解明白了就好写（[Andy Lu](https://github.com/luyu20010926)：我15分钟写完个树剖！）

一共两次DFS：

- 第一次 DFS：标记出每个节点的大小（他自己及他的子树总共结点个数），每个节点的重儿子

- 第二次 DFS：这次DFS需要得到DFN，而对于一个节点 u ，优先遍历他的重儿子（这样能够保证每条重链上所有节点都在线段树上连续的一段，这样才能够用线段树维护重链）

DFS1，DFS2的代码：

```cpp
void dfs1(Node *u)
{
    u->size=1;
    u->visited=true;
    for(Edge *e=u->first; e; e=e->next)
    {
        Node *v=e->to;

        if(v->visited==false)
        {
            v->depth=u->depth+1;
            v->father=u;
            dfs1(v);
            u->size+=v->size;
            if(u->maxChild==NULL||u->maxChild->size<v->size)
            {
                u->maxChild=v;
            }
        }
    }
}


void dfs2(Node *u)
{
    u->dfn=dfn;
    dfn++;
    if(u->father==NULL||u!=u->father->maxChild)
    {
        u->chain=new Chain;
        u->chain->top=u;
    }
    else
    {
        u->chain=u->father->chain;
    }
    if(u->maxChild!=NULL)
    {
        dfs2(u->maxChild);
    }
    for(Edge *e=u->first; e; e=e->next)
    {
        Node *v=e->to;
        if(v->father==u&&v!=u->maxChild)
        {
            dfs2(v);
        }
    }
}
```


# 查询

构造之后，还需要查询，这里有个很巧妙的方法

假如我们在查询`(Node*)u`和`(Node*)v`之间的信息，假设`u`所在的链的头部节点的深度大于`v`所在的链的头部节点的深度，那么可以把查询转化为 `u`到他所在的链的头部之间的查询 和`u`所在的链的头部节点的父亲节点和`v`之间的查询，具体实现看代码：

```cpp
// queryMax()
while(u1->chain!=u2->chain)
{
    if(u1->chain->top->depth > u2->chain->top->depth)
    {
        swap(u1,u2);
    }
    maxn=max(maxn,root->queryMax(u2->chain->top->dfn,u2->dfn));
    u2=u2->chain->top->father;
}

// querySum()
while(u1->chain!=u2->chain)
{
    if(u1->chain->top->depth > u2->chain->top->depth)
    {
        swap(u1,u2);
    }
    sum+=root->querySum(u2->chain->top->dfn,u2->dfn);
    u2=u2->chain->top->father;
}
```

# 参考

尤其感谢[Menci的博客](https://oi.men.ci/tree-chain-split-notes/)

他的代码是我看懂的第一份树剖代码= =。要不然还得卡在这儿