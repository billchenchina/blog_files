# 动态规划

由 [Todobe](http://blog.csdn.net/todobe) 暑期培训讲义和 [tianyicui/pack](https://github.com/tianyicui/pack) 整理而来

## 背包问题

经典中的经典，崔添翼讲的背包问题九讲orz

再复习下吧

### 模型

有 n 种物品，每一种物品有自己的重量 w 和价值 v ，背包的容量为 m ，如何选择物品使得背包中价值最大

### 01背包

每种物品只有一个，对于某种物品，只有选/不选，求背包价值最大

这段代码来源于 [Todobe](http://blog.csdn.net/todobe) 暑期培训讲义，是经过空间压缩的

```cpp
for(int i=1;i<=n;i++){
    for(int j=m;j>=w[i];j--){
        f[j]=max(f[j],f[j-w[i]]+v[i]);
    }
}
```

### 完全背包

每种物品有无限个，求背包最大价值

orz代码还是来源于[Todobe](http://blog.csdn.net/todobe) 暑期培训讲义

```cpp
for(int i=1;i<=n;i++){
    for(int j=w[i];j<=m;j++){
        f[j]=max(f[j],f[j-w[i]]);
    }
}
```
