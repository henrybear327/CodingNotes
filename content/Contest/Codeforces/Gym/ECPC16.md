---
title: "2016-2017 ACM-ICPC, Egyptian Collegiate Programming Contest (ECPC 16)"
date: 2017-10-13T20:36:01+08:00
draft: false

tags: ["Codeforces", "Gym", "BFS"]
categories: ["Competitive Programming"]

weight: 5
---

[Problem statements](http://codeforces.com/gym/101147)

3 easy problems, but the rest are hard.

<!--more-->

## B

3 cases:

1. x-axis overlapping
2. y-axis overlapping
3. no overlapping

The case that might got you is the case of rectangles touching on the y-axis. Try this case:

```
1
4 200 100
20 80 10 1
80 20 20 0
20 90 120 1
30 8 150 0
```

The answer is `60.198039`.

{{< highlight cpp "linenos=inline" >}}
#include <bits/stdc++.h>

using namespace std;

#define N 111
#define MAX 1e18

typedef long long ll;

struct Pt {
    int x, y;
};

struct Rect {
    Pt upperLeft, lowerRight;
    int k;
};

double dist(int x1, int y1, int x2, int y2)
{
    ll dx = x2 - x1;
    ll dy = y2 - y1;
    return sqrt(dx * dx + dy * dy);
}

void solve()
{
    int n, l, u;
    scanf("%d %d %d", &n, &l, &u);

    // read
    double m[N][N];
    for (int i = 0; i < N; i++)
        for (int j = 0; j < N; j++)
            m[i][j] = (i == j ? 0 : MAX);

    vector<Rect> rect;
    rect.push_back({{0, 0}, {u, 0}, 0}); // bottom
    rect.push_back({{0, l}, {u, l}, 0}); // top
    m[0][1] = m[1][0] = l;

    for (int i = 0; i < n; i++) {
        int h, w, d, k;
        scanf("%d %d %d %d", &h, &w, &d, &k);

        if (k == 0) {
            Pt upperLeft = {0, h + d};
            Pt lowerRight = {w, d};
            rect.push_back({upperLeft, lowerRight, k});
        } else {
            Pt upperLeft = {u - w, h + d};
            Pt lowerRight = {u, d};
            rect.push_back({upperLeft, lowerRight, k});
        }

        m[i + 2][1] = m[1][i + 2] = l - h - d;
        m[i + 2][0] = m[0][i + 2] = d;
    }

    // build
    int sz = rect.size();
    for (int i = 2; i < sz; i++) {
        for (int j = 2; j < sz; j++) {
            if (i == j)
                continue;

            int &ax = rect[i].upperLeft.x;
            int &ay = rect[i].upperLeft.y;
            int &bx = rect[i].lowerRight.x;
            int &by = rect[i].lowerRight.y;
            int &ki = rect[i].k;

            int &cx = rect[j].upperLeft.x;
            int &cy = rect[j].upperLeft.y;
            int &dx = rect[j].lowerRight.x;
            int &dy = rect[j].lowerRight.y;
            int &kj = rect[j].k;

            double val;
            if (ax <= cx && cx <= bx) { // case1 - 1
                if (ki != kj && cx == bx && by <= cy && cy <= ay) // touching case
                    val = 0;
                else
                    val = min(abs(ay - dy), abs(by - cy));
            } else if (ax <= dx && dx <= bx) { // case1 - 2
                if (ki != kj && dx == bx && by <= dy && dy <= ay) // touching case
                    val = 0;
                else
                    val = min(abs(ay - dy), abs(by - cy));
            } else if (by <= cy && cy <= ay) { // case2 - 1
                val = abs(bx - cx);
            } else if (by <= dy && dy <= ay) { // case2 - 2
                val = abs(bx - cx);
            } else { // case3
                val = min(dist(bx, ay, cx, dy), dist(bx, by, cx, cy));
                /*
                if(ay < dy) {
                    val = dist(bx, ay, cx, dy);
                } else {
                    val = dist(bx, by, cx, cy);
                }
                */
            }

            m[i][j] = min(m[i][j], val);
            m[j][i] = min(m[j][i], val);
            //printf("%d %d %.6f\n", i, j, m[i][j]);
            //printf("%d %d,  %d %d,  %d %d, %d %d\n", ax, ay, bx, by, cx, cy, dx, dy);
        }
    }

    // solve
    for (int k = 0; k < sz; k++)
        for (int i = 0; i < sz; i++)
            for (int j = 0; j < sz; j++) {
                if (m[i][j] > 1e9)
                    continue;
                m[i][j] = min(m[i][j], m[i][k] + m[k][j]);
            }

    printf("%.6f\n", m[0][1]);
}

int main()
{
    freopen("street.in", "r", stdin);

    int ncase;
    scanf("%d", &ncase);

    while (ncase--)
        solve();

    return 0;
}
{{< / highlight >}}

## D

$C(n, m)$

{{< highlight cpp "linenos=inline" >}}
lude<bits / stdc++.h>

using namespace std;

typedef long long ll;
ll dp[30][30];
ll C(ll a, ll b)
{
    if (dp[a][b] != -1)
        return dp[a][b];
    ll ans = 1;
    for (int i = 1; i <= b; i++) {
        ans = ans * (a - i + 1);
        ans = ans / i;
    }
    return dp[a][b] = ans;
}

void solve()
{
    int n, m;
    scanf("%d %d", &n, &m);

    printf("%lld\n", C(n, m));
}

int main()
{
    freopen("popcorn.in", "r", stdin);
    int ncase;
    scanf("%d", &ncase);
    memset(dp, -1, sizeof(dp));
    while (ncase--)
        solve();

    return 0;
}
{{< / highlight >}}

## E

BFS. Add edges, but reverse them!

{{< highlight cpp "linenos=inline" >}}
#include <bits/stdc++.h>

using namespace std;

#define N 100010
vector<int> g[N];

void solve()
{
    int n;
    scanf("%d", &n);

    for (int i = 0; i < n; i++)
        g[i].clear();

    for (int i = 0; i < n; i++) {
        int d;
        scanf("%d", &d);

        int nxt = i + d;
        int pre = i - d;

        if (nxt < n)
            g[nxt].push_back(i);
        if (pre >= 0)
            g[pre].push_back(i);
    }

    queue<int> q;
    int dist[N];
    memset(dist, -1, sizeof(dist));
    dist[n - 1] = 0;
    q.push(n - 1);

    while (q.empty() == false) {
        int u = q.front();
        q.pop();

        for (auto v : g[u]) {
            if (dist[v] == -1) {
                dist[v] = dist[u] + 1;
                q.push(v);
            }
        }
    }

    for (int i = 0; i < n; i++)
        printf("%d\n", dist[i]);
}

int main()
{
    freopen("jumping.in", "r", stdin);

    int ncase;
    scanf("%d", &ncase);

    while (ncase--) {
        solve();
    }

    return 0;
}
{{< / highlight >}}