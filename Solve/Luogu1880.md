# Luogu1880 合并果子

> <del>orz这道题竟然卡了我一整周</del>
>
> 在此 %%% igronemyk 和 kgrox

## 大致题意

> 圆形操场的四周摆放N堆石子（环形结构）,现要将石子有次序地合并成一堆.规定每次只能选相邻的2堆合并成新的一堆，并将新的一堆的石子数，记为该次合并的得分。
>
> 然后求最大最小得分

## 题解

orz...由于环形结构导致长度最大为 N，把它化成一个长度 2\*N 的数组慢慢比

然后我们就有了`prefix_arr`数组记录前缀和，由于既要最大值又要最小值，用两个二维数组`dp1_arr`和`dp2_arr`来记录最大/最小得分。

`dp1(2)_arr[i][j]`表示[i,j]闭区间内能得到的最大/最小值。

DP式为dp[i][j]=max/min(dp[i][k]+dp[k+1][j]+sum(i,j))（sum(i,j)表示[i,j]内石子总数）

## 代码实现

```cpp
#include <bits/stdc++.h>

using namespace std;
int dp1_arr[101][101]= {0};
int dp2_arr[101][101]= {0};
int main()
{
    int N;
    cin>>N;
    int prefix_arr[201];
    // input data and switch it to prefix arr
    for(int i=0; i<N; ++i)
    {
        int tmp;
        cin>>tmp;
        if(i==0)
        {
            prefix_arr[i]=tmp;
        }
        else
        {
            prefix_arr[i]=prefix_arr[i-1]+tmp;
        }
    }
    // prefix arr times two
    for(int i=N; i<2*N; ++i)
    {
        if(i==N)
        {
            prefix_arr[i]=prefix_arr[i-1]+prefix_arr[0];
        }
        else
        {
            prefix_arr[i]=prefix_arr[i-1]+(prefix_arr[i-N]-prefix_arr[i-N-1]);
        }
    }
    for(int range=1; range<=N; ++range)
    {
        if(range==1)
        {
            for(int start_pos=0; start_pos<N; ++start_pos)
            {
                dp1_arr[start_pos][start_pos]= 0;
                dp2_arr[start_pos][start_pos]= 0;
            }
        }
        else
        {
            for(int start_pos=0; start_pos<N; ++start_pos)
            {
                int end_pos=start_pos+range-1;
                int minn=INT_MAX;
                int maxn=0;
                for(int mid=start_pos; mid<end_pos; ++mid)
                {
                    int m2=mid+1;
                    int e2=end_pos;
                    if(m2>=N)
                    {
                        m2%=N;
                        e2%=N;
                    }
                    if(start_pos==0)
                    {
                        minn=minn<(dp1_arr[start_pos][mid]+dp1_arr[m2][e2]+prefix_arr[end_pos])?minn:(dp1_arr[start_pos][mid]+dp1_arr[m2][e2]+prefix_arr[end_pos]);
                        maxn=maxn>(dp2_arr[start_pos][mid]+dp2_arr[m2][e2]+prefix_arr[end_pos])?maxn:(dp2_arr[start_pos][mid]+dp2_arr[m2][e2]+prefix_arr[end_pos]);
                    }
                    else
                    {
                        minn=minn<(dp1_arr[start_pos][mid]+dp1_arr[m2][e2]+prefix_arr[end_pos]-prefix_arr[start_pos-1])?minn:(dp1_arr[start_pos][mid]+dp1_arr[m2][e2]+prefix_arr[end_pos]-prefix_arr[start_pos-1]);
                        maxn=maxn>(dp2_arr[start_pos][mid]+dp2_arr[m2][e2]+prefix_arr[end_pos]-prefix_arr[start_pos-1])?maxn:(dp2_arr[start_pos][mid]+dp2_arr[m2][e2]+prefix_arr[end_pos]-prefix_arr[start_pos-1]);
                    }
                }
                //cout<<start_pos<<' '<<end_pos<<" => "<<minn<<endl;
                dp1_arr[start_pos][end_pos]=minn;
                dp2_arr[start_pos][end_pos]=maxn;
            }
        }
    }
    int ans1=INT_MAX;
    int ans2=0;
    for(int i=0; i<N; i++)
    {
        if(dp1_arr[i][i+N-1]<ans1)
            ans1=dp1_arr[i][i+N-1];
        if(dp2_arr[i][i+N-1]>ans2)
            ans2=dp2_arr[i][i+N-1];
    }
    cout<<ans1<<endl<<ans2;

}

```

## 题外话

orz...我的 [1880.cpp](https://github.com/billchenchina/cppcodes/blob/master/luogu/1880.cpp)由于玄♂学原因不知哪里错了，然后就敲了一发 [1880_2.cpp](https://github.com/billchenchina/cppcodes/blob/master/luogu/1880_2.cpp)

然后。。。

然后它就AC了。

