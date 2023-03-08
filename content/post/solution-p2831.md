---
author: "Night Space"
title: 题解 P2831 [NOIP2016 提高组] 愤怒的小鸟
date: 2022-08-07T15:14:00+08:00
lastmod: 2022-09-03T22:12:00+08:00
tags:
    - 算法
    - 题解
categories:
    - 算法
    - 题解
---
# 题目分析
已知 $n$ 个在第一象限的点，最少需要几条形如 $y=ax^2 + bx(a<0)$ 的曲线可以覆盖所有点

# 思路
显然，只有那些至少经过一个点的曲线是需要考虑的

因此，我们枚举每一个点对 $(x_1,y_1)$ $(x_2,y_2)$ ，设经过这两点的曲线为 $y=ax^2 + bx$

解得

$$
\left\\{
\begin{array}{l}
a=\dfrac{x_2y_1-x_1y_2}{x_1x_2(x_1-x_2)}\\\\
b=\dfrac{y_1-ax_1^2}{x_1}
\end{array}
\right.
$$

另外，我们还要为每个点单独添加一条经过它的曲线

因为点的数量较少，我们可以通过状态压缩对每条曲线进行编码

对于每一条曲线，枚举每一个点，若 $(x_i,y_i)$ 在该曲线上，则将该曲线编码的第 $i - 1$ 为设为 $1$

每条曲线的编码表示了它所经过的点

根据编码进行去重

我们还可以对每个状态进行编码，若 $(x_i,y_i)$ 已被至少一条曲线经过，则将该状态编码的第 $i - 1$ 为设为 $1$

通过加入一条曲线，一个状态可以转移到该状态编码与该曲线编码按位或的结果代表的状态，则可在该状态与按位或的结果代表的状态间连边

从初始状态 $0$ 开始计算最短路即可

# Code
```cpp
#include <bits/stdc++.h>
using namespace std;
struct Pig {
    double x, y;
};
struct Line {
    double a, b;
};
const double EPS = 1e-7;
int T;
int n, m;
Pig pig[21];
vector<uint32_t> line;
int minw[262144];
Line getLineByPig(const Pig &a, const Pig &b) {
    Line line;
    line.a = (b.x * a.y - a.x * b.y) / (a.x * b.x * (a.x - b.x));
    line.b = (a.y - line.a * a.x * a.x) / a.x;
    return line;
}
uint32_t encodeLine(const Line &line) {
    if(line.a >= 0) {
        return 0;
    }
    uint32_t code = 0;
    for(int i = 1; i <= n; i++) {
        if(abs(line.a * pig[i].x * pig[i].x + line.b * pig[i].x - pig[i].y) <= EPS) {
            code |= (1 << (i - 1));
        }
    }
    return code;
}
void bfs() {
    memset(minw, 0x3f, sizeof(minw));
    queue<uint32_t> q;
    minw[0] = 0;
    q.push(0);
    while(!q.empty()) {
        uint32_t at = q.front();
        q.pop();
        for(auto i = line.begin(); i != line.end(); i++) {
            uint32_t new_at = at | *i;
            if(minw[new_at] > minw[at] + 1) {
                minw[new_at] = minw[at] + 1;
                if(new_at == (1 << n) - 1) {
                    return;
                }
                q.push(new_at);
            }
        }
    }
}
void solve() {
    line.clear();
    cin >> n >> m;
    for(int i = 1; i <= n; i++) {
        cin >> pig[i].x >> pig[i].y;
    }
    for(int i = 1; i <= n; i++) {
        for(int j = i + 1; j <= n; j++) {
            line.push_back(encodeLine(getLineByPig(pig[i], pig[j])));
        }
        line.push_back(1 << (i - 1));
    }
    {
        set<uint32_t> tmp;
        for(auto i = line.begin(); i != line.end(); i++) {
            tmp.insert(*i);
        }
        line.clear();
        for(auto i = tmp.begin(); i != tmp.end(); i++) {
            line.push_back(*i);
        }
    }
    bfs();
    cout << minw[(1 << n) - 1] << endl;
}
int main() {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);
    cin >> T;
    for(int i = 1; i <= T; i++) {
        solve();
    }
}
```
