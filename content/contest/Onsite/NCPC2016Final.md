---
title: "NCPC 2016 Final"
date: 2017-10-19T18:44:50+08:00
draft: false

tags: ["NCPC", "Enumeration"]
categories: ["Competitive Programming"]

weight: 20161
---

充滿回憶的比賽，尤其是B QAQ

<!--more-->

## [B](http://e-tutor.itsa.org.tw/e-Tutor/mod/programming/view.php?a=17928)

簡單爆搜。

{{% expand "Hint" %}}
幾個1?
{{% /expand %}}

{{% expand "Solution" %}}

{{% /expand %}}

### AC Code

{{< highlight cpp "linenos=inline" >}}

#include <bits/stdc++.h>

using namespace std;

typedef long long ll;

ll solve(ll n)
{
    if (n == 2) {
        return 1;
    }

    int ans = 1;
    ll orig = n;
    for (; ans < 33; ans++) {
        n = orig;
        ll sum = ans;

        n -= ans;
        for (int i = 0; i < ans; i++) {
            if (n - (ans - i + 1) == 0)
                return ans;
            n -= sum + 1;
            sum += sum + 1;
        }

        if (n <= sum + 1)
            break;
    }

    return ans;
}

int main()
{
#ifndef DEBUG
    int ncase;
    scanf("%d", &ncase);
    while (ncase--) {
        ll n;
        scanf("%lld", &n);
        printf("%lld\n", solve(n));
    }
#else
    for (int i = 2; i <= 100; i++) {
        printf("n = %d, %lld\n", i, solve(i));
    }
#endif

    return 0;
}

{{< / highlight >}}