---
title: "NCPC 2017 Preliminary Round"
description: "Problem E, what a pity!"
date: 2017-10-14T22:49:57+08:00
draft: false

tags: ["NCPC", "Implementation", "Enumeration"]
categories: ["Competitive Programming"]

weight: 20170
---

<!--more-->

## Problem

{{% panel theme="success" header="比賽題目" %}}

{{%attachments pattern=".*(pdf)"/%}}

{{% /panel %}}

## A

Just 3n + 1.

{{< highlight cpp "linenos=inline" >}}
#include <bits/stdc++.h>
using namespace std;

#define sz(x) (int(x.size()))
typedef long long ll;
typedef double db;

int main() {
    ll x;
    while (scanf(" %lld", &x)) {
        if (x == 0) break;
        printf("%lld ", x);

        ll mx = -1;
        int i;
        for (i = 0; ; i++) {
            if (x & 1) {
                x = 3 * x + 1;
            }
            else {
                x = x / 2;
            }
            mx = max(mx, x);
            if (x == 1) {
                break;
            }
        }

        printf("%d %lld\n", i + 1, mx);
    }


    return 0;
}

{{< / highlight >}}

## B

{{< highlight cpp "linenos=inline" >}}
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;

void solve()
{
    int n;
    scanf("%d", &n);

    int inp[n];
    for(int i = 0; i < n; i++) {
        scanf("%d", &inp[i]);
    }

    sort(inp, inp + n);

    // implement
    // -------++++++++
    ll product = inp[0] * inp[1];
    ll ans = LLONG_MIN;
    for(int i = 2; i < n; i++) {
        ans = max(ans, product * inp[i]);
    }

    product = inp[n - 1] * inp[n - 2];
    for(int i = 0; i < n - 2; i++) {
        ans = max(ans, product * inp[i]);
    }

    printf("%lld\n", ans);
}

int main()
{
    int ncase;
    scanf("%d", &ncase);

    while(ncase--)
        solve();

    return 0;
}

{{< / highlight >}}

## C

Ahh...... We can't do the math with mod inverse... so we resort to enumeration...

{{< highlight cpp "linenos=inline" >}}
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;
const ll P = 65537; 

ll x[3];
ll y[3];
ll cal(ll a0, ll a1, ll a2, int which)
{
    ll x_2 = x[which] * x[which] % P;
    return (((a0 + a1 * x[which]) % P + a2 * x_2) % P);
}

void solve()
{
    for(int i = 0; i < 3; i++)
        scanf("%lld %lld", &x[i], &y[i]);
    
    ll x_2 = (x[0] * x[0]) % P;
    for(ll a1 = 0; a1 < P; a1++) {
        for(ll a2 = 0; a2 < P; a2++) {
            ll a0 = 5 * P + y[0] - (a1 * x[0]) % P - (a2 * x_2) % P;
            a0 %= P;
            assert(cal(a0, a1, a2, 0) == y[0]);

            if(cal(a0, a1, a2, 1) == y[1] && cal(a0, a1, a2, 2) == y[2]) {
                printf("%lld %lld %lld\n", a0, a1, a2);
                return;
            }
        }
    }
}

int main()
{
    int ncase;
    scanf("%d", &ncase);

    while(ncase--)
        solve();

    return 0;
}

{{< / highlight >}}

## D

Problem statement contains error..............

{{< highlight cpp "linenos=inline" >}}
#include <bits/stdc++.h>
using namespace std;

const int S = 4;
const int INA = 2; // inactive
const int ADA = 0;
const int XY = 1;
const int NON = 3; // nonreachable
const int ACT = 5; // active

int sx, sy;
int A[8][8];
int Z[8][8];

bool xy_check(int tx, int ty) {
    for (int x = min(sx, tx); x <= max(sx, tx); x++) {
        if (A[x][sy] == INA) {
            return false;
        }
    }
    for (int y = min(sy, ty); y <= max(sy, ty); y++) {
        if (A[tx][y] == INA) {
            return false;
        }
    }
    return true;
}

bool ada_check(int tx, int ty) {
    for (int x = min(sx, tx); x <= max(sx, tx); x++) {
        for (int y = min(sy, ty); y <= max(sy, ty); y++) {
            if (A[x][y] == INA) {
                return false;
            }
        }
    }
    return true;
}

void print(int a[8][8]) {
    for (int y = 0; y < 8; y++) {
        for (int x = 0; x < 8; x++) {
            cout << a[x][y];
        }
        cout << endl;
    }
}

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);

    string s;
    while (cin >> s) {
        if (s == "0") break;

        for (int x = 0; x < 8; x++) {
            for (int y = 0; y < 8; y++) {
                Z[x][y] = NON;
            }
        }

        for (int i = 0; i < 64; i++) {
            int y = i / 8;
            int x = i % 8;
            if (s[i] == '2') {
                A[x][y] = S;
                Z[x][y] = S;
                sx = x;
                sy = y;
            } else if (s[i] == '1') {
                A[x][y] = INA;
                Z[x][y] = INA;
            } else {
                A[x][y] = ACT;
            }
        }

        // print(A);
        // cout << endl;

        for (int x = 0; x < 8; x++) {
            for (int y = 0; y < 8; y++) {
                if (x == sx && y == sy) continue;
                if (xy_check(x, y)) {
                    Z[x][y] = XY;
                }
            }
        }

        for (int x = 0; x < 8; x++) {
            for (int y = 0; y < 8; y++) {
                if (x == sx && y == sy) continue;
                if (ada_check(x, y)) {
                    Z[x][y] = ADA;
                }
            }
        }

        for (int y = 0; y < 8; y++) {
            for (int x = 0; x < 8; x++) {
                cout << Z[x][y];
            }
        }
        cout << endl;

        // print(Z);
    }
    return 0;
}

{{< / highlight >}}

## G

I know using graph for this one is overkill, but this is what makes us feel like 1 AC :)

{{< highlight cpp "linenos=inline" >}}
#include <bits/stdc++.h>

using namespace std;

#define N 10010

typedef pair<int, int> ii;

void solve(int n)
{
    int inp[n + 1];
    for(int i = 1; i <= n; i++)
        scanf("%d", &inp[i]);
    // for(int i = 1; i <= n; i++)
        //printf("%d%c", inp[i], i == n ? '\n' : ' ');

    vector<int> g[N];
    int in[N] = {0};
    int out[N] = {0};
    for(int i = 1; i <= n; i++) {
        g[i].push_back(inp[i]);
        in[inp[i]]++;
        out[i]++;
    }
    
    vector<int> s;
    for(int i = 1; i <= n; i++) {
        if(in[i] == 0)
            s.push_back(i);
    }
    
    vector<ii> ans;
    for(int i = 0; i < (int) s.size(); i++) {
        int cnt = 0;
        int who = s[i];
        // printf("cnt %d who %d\n", cnt, who);
        while((int)g[who].size() > 0) {
            // printf("cnt %d who %d\n", cnt, who);
            cnt++;
            who = g[who][0];
        }

        ans.push_back(ii(s[i], cnt));
    }   
    sort(ans.begin(), ans.end());
    
    printf("%d", (int)ans.size());
    for(int i = 0; i < (int)ans.size(); i++)
        printf(" %d %d", ans[i].first, ans[i].second);
    printf("\n");
}

int main()
{
    int n;
    while(scanf("%d", &n) == 1 && n)
        solve(n);

    return 0;
}

{{< / highlight >}}