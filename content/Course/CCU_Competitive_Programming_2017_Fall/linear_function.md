---
title: "Linear function"
date: 2017-10-14T23:11:41+08:00
draft: false

tags: ["Stack", "Implementation"]
categories: ["Competitive Programming"]

weight: 100
---

<!--more-->

## Problem

There are $n$ linear functions $f_i(x)=a[i]x+b[i], 1<= i <=n$. Define $F(x)=max{f_i(x) : 1<= i <=n}$. 

Given $m$ x-values $c[1],c[2],…, c[m]$, please compute the sum of all $F(c[j])$, i.e., $F(c[1])+ F(c[2])+…+ F(c[m])$. 

## Solution sketch

To be continued... 

## AC Code

{{< highlight cpp "linenos=inline" >}}
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;
typedef pair<ll, ll> ii;

bool cmp(const ii &a, const ii &b)
{
    if (a.first == b.first)
        return a.second > b.second;
    return a.first < b.first;
}

ll cal(ii coeff, ll x)
{
    return coeff.first * x + coeff.second;
}

double x_val(ii a, ii b)
{
    return (b.second - a.second) / (a.first - b.first);
}

#define EPS 1e-9

void solve(int n, int m)
{
    ii coeff[n];
    for (int i = 0; i < n; i++)
        scanf("%lld %lld", &coeff[i].first, &coeff[i].second);
    ll x[m];
    for (int i = 0; i < m; i++)
        scanf("%lld", &x[i]);

    sort(coeff, coeff + n, cmp);
    sort(x, x + m);

    vector<ii> ok;
    ok.push_back(coeff[0]);
    for (int i = 1; i < n; i++) {
        while (ok.size() > 1) {
            int sz = ok.size() - 1;
            if (ok[sz].first == coeff[i].first ||
                (x_val(ok[sz], ok[sz - 1]) - x_val(ok[sz], coeff[i]) > -EPS))
                ok.pop_back();
            else
                break;
        }
        ok.push_back(coeff[i]);
    }

    // for(auto i : ok)
    // printf("%lld %lld\n", i.first, i.second);

    ll ans = 0;
    int idx = 0;
    for (int i = 0; i < m; i++) {
        while (idx != (int)ok.size() - 1 &&
               cal(ok[idx], x[i]) < cal(ok[idx + 1], x[i])) {
            idx++;
        }
        ans += cal(ok[idx], x[i]);
    }

    printf("%lld\n", ans);
}

int main()
{
    int n, m;
    while (scanf("%d %d", &n, &m) == 2 && (n != 0)) {
        solve(n, m);
    }

    return 0;
}


{{< / highlight >}}