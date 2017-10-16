# Manacher 算法

## 介绍

err...好的博客真的难找。。。

之前 Ferric 大佬讲课之前自己预习过 Manacher 算法，当时看到篇挺好的 Blog。后来复习的时候。。。发现 Manacher 全都忘了，找了半天才找到 orz 我太弱了。

这篇博客讲的很详细，可以看看。。。其实是我懒得写了（逃

[Manacher's ALGORITHM: O(n)时间求字符串的最长回文子串 - Felix021 - 将所有欢脱倾翻](https://www.felix021.com/blog/read.php?2040)

## 代码（HDOJ居然挂了。。。还没测试对不对呢

```cpp
#include <bits/stdc++.h>

using namespace std;

int main()
{
    string str;
    int P[110010*2];
    while(getline(cin,str))
    {
    if(str.length()==0)break;
    string str2="$#";
    int len=str.length();
    for(int i=0;i<len;++i)
    {
        str2.append(1,str[i]);
        str2.append("#");
    }
    len=str2.length();
    int maxn=0,id=0;
    int ans=0;
    memset(P,0,sizeof P);
    for(int i=1;i<len;++i)
    {
        if(maxn>i)
        {
            if(P[2*id-i]<maxn-i)
            {
                P[i]=P[2*id-i];
            }
            else
            {
                P[i]=maxn-i;
            }
        }
        else
        {
            P[i]=1;
        }
        while(str2[i+P[i]]==str2[i-P[i]])P[i]++;
        if(i+P[i]>maxn)
        {
            maxn=i+P[i];
            id=i;
        }
        if(P[i]>ans)ans=P[i];
    }
    cout<<ans-1<<endl;}
}
```