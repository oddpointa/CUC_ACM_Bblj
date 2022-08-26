# 2022吉林ccpc D题
##  [Assumption is All You Need](https://codeforces.com/gym/103409/problem/D)
---
##  题意：


给定一个1到n的两个排列A与B，最终的目标是将A经过下列操作成B,输出操作个数k与k个操作下标

+ 选定下标i，j,并交换(要求i<j，并且$ A_i >A_j$,即是只能对逆序对操作)

---

## 思路：
从左到右依次遍历，分三种情况：
- $A_i==B_i$ 直接跳过，后面的操作也不会影响前面的
- $A_i < B_i$   直接退出，要把后面的小的拿到前面，不符合操作要求，直接退出输出-1
- $A_i > B_i$  在A后不断循环，有符合操作要求的都进行交换，知道一次交换完后$A_i==B_i$退出(把大的不断往前提，保证了后面操作的最优化，使得操作个数尽可能的多)

---

## AC代码
```C++
#include <bits/stdc++.h>
using namespace std;
const int N = 3000;
int n;
int a[N], b[N];
vector<pair<int, int>> ans; //用pair类型来存储答案
int main()
{
    int T;
    scanf("%d", &T);
    while (T--)
    {
        ans.clear();
        scanf("%d", &n);
        for (int i = 1; i <= n; ++i)
        {
            scanf("%d", &a[i]);
        }
        for (int i = 1; i <= n; ++i)
        {
            scanf("%d", &b[i]);
        }
        bool ok = true;
        for (int i = 1; i <= n; ++i) //从左到右比较遍历
        {
            if (a[i] == b[i]) //能匹配的直接跳过，而且后续操作不会影响之前的
                continue;
            if (a[i] < b[i]) //因为是是能对逆序对操作，所以有小于的就要将后面的大数挪到前面，不符合规则，直接退出
            {
                ok = false;
                break;
            }
            int j = i + 1;                 //从这一项的后面开始找
            while (j <= n && a[i] != b[i]) //截止到相等为止
            {
                if (a[j] < a[i] && a[j] >= b[i]) //符合操作要求，将比此位大的都往前放，便于更多的后续操作
                {
                    swap(a[i], a[j]);
                    ans.push_back({i, j});
                } //使得后续操作最优化
                j++;
            }
            if (a[i] != b[i])
            {
                ok = false;
                break;
            }
        }
        if (ok)
        {
            printf("%d\n", (int)ans.size());
            for (auto v : ans)
            {
                printf("%d %d\n", v.first, v.second);
            }
        }
        else
        {
            puts("-1");
        }
    }
}

```