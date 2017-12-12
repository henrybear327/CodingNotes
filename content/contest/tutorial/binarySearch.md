---
title: "Binary Search"
description: "Introduction to binary search"
date: 2017-12-12T10:19:36+08:00
draft: false

tags: ["Binary search", "Codeforces", "POJ", "UVa"]
categories: ["Competitive Programming"]

weight: 10
---

<!--more-->

1. 二分搜的前提：**單調性**！所以記得先 sorting 之後再二分搜
2. 注意你所訂下的上界與下界，必要時要在對答案做檢查
3. `lower_bound()` 和 `upper_bound()`請想清楚再用
4. 浮點數要注意精度問題

# 整數上的 binary search

## [CF 474B Worms](http://codeforces.com/problemset/problem/474/B)

{{% expand "Hint" %}}
prefix sum + lower_bound
{{% /expand %}}

### AC Code

{{< highlight cpp "linenos=inline" >}}

#include <bits/stdc++.h>

using namespace std;

int main()
{
    int n;
    scanf("%d", &n);

    int inp[n + 1];
    inp[0] = 0;
    for (int i = 1; i <= n; i++)
        scanf("%d", &inp[i]);
    for (int i = 2; i <= n; i++)
        inp[i] += inp[i - 1];

    int q, k = 0;
    scanf("%d", &q);

    for (int i = 0; i < q; i++) {
        scanf("%d", &k);
        printf("%d\n", (int)(lower_bound(inp, inp + n + 1, k) - inp));
    }

    return 0;
}

{{< / highlight >}}

## [CF 706B Interesting drink](http://codeforces.com/problemset/problem/706/B)

{{% expand "Hint" %}}
upper_bound
{{% /expand %}}

### AC Code

{{< highlight cpp "linenos=inline" >}}

#include <bits/stdc++.h>

using namespace std;

int main()
{
    int n;
    scanf("%d", &n);

    int inp[n];
    for (int i = 0; i < n; i++)
        scanf("%d", &inp[i]);
    sort(inp, inp + n);

    int m;
    scanf("%d", &m);
    // 3 6 8 10 11
    for (int i = 0; i < m; i++) {
        int num;
        scanf("%d", &num);

        auto it = upper_bound(inp, inp + n, num);
        printf("%d\n", (int)(it - inp));
    }

    return 0;
}

{{< / highlight >}}

# 浮點數的 binary search

## [POJ1064 Cable master](http://poj.org/problem?id=1064)

{{% expand "Hint" %}}
請小心他**沒**要你四捨五入
{{% /expand %}}

{{% expand "Solution" %}}
非常好且經典的 floating point 二分搜題目，需要非常小心的 implementation。

[題解](http://blog.csdn.net/discreeter/article/details/53285350)
{{% /expand %}}

### AC Code

{{< highlight cpp "linenos=inline" >}}

#include <cmath>
#include <cstdio>

int n, k;
bool check(double mid, double inp[])
{
    int cnt = 0;
    for (int i = 0; i < n; i++) {
        cnt += (int)(inp[i] / mid);
    }

    return cnt >= k;
}

int main()
{
    scanf("%d %d", &n, &k);

    double inp[n];
    for (int i = 0; i < n; i++)
        scanf("%lf", &inp[i]);

    double l = 0, r = 100000001;
    for (int i = 0; i < 100; i++) {
        double mid = (l + r) / 2;

        if (check(mid, inp))
            l = mid;
        else
            r = mid;
    }

    printf("%.2f\n", (floor)(l * 100) / 100);

    return 0;
}

{{< / highlight >}}

## [CF 492B Vanya and Lanterns](http://codeforces.com/problemset/problem/492/B)

{{% expand "Solution" %}}
有數學解，但是可以練習一下浮點數二分搜。
{{% /expand %}}

### AC Code

{{< highlight cpp "linenos=inline" >}}

#include <bits/stdc++.h>

using namespace std;

#define EPS 1e-10

int n, len;
bool check(vector<int> &inp, double mid)
{
    if (inp[0] - mid > -EPS)
        return false;
    double farthest = inp[0] + mid;
    for (int i = 1; i < (int)inp.size(); i++) {
        if (farthest - (inp[i] - mid) >= -EPS)
            farthest = inp[i] + mid;
        else
            return false;
    }

    if (farthest - len >= -EPS)
        return true;
    return false;
}

int main()
{
    scanf("%d %d", &n, &len);

    vector<int> inp;
    for (int i = 0; i < n; i++) {
        int num;
        scanf("%d", &num);

        inp.push_back(num);
    }

    sort(inp.begin(), inp.end());
    auto it = unique(inp.begin(), inp.end());
    inp.resize(distance(inp.begin(), it));

    double l = 0, r = len;
    for (int i = 0; i < 200; i++) {
        double mid = (l + r) / 2;

        if (check(inp, mid))
            r = mid;
        else
            l = mid;
    }

    printf("%.15f\n", l);

    return 0;
}


{{< / highlight >}}

# 挑戰題

## [UVa 12192 Grapevine](https://uva.onlinejudge.org/external/121/12192.pdf)

進階題，大家可以挑戰一下。

### AC Code

{{< highlight cpp "linenos=inline" >}}

#include <bits/stdc++.h>

// For every query, binary search for the upper-left corner of the square
// And then binary search for the lower-left corner "on the diagnal"

using namespace std;

int n, m;
int inp[555][555];
int low, high;

int search(int row, int col)
{
    // printf("row %d col %d\n", row, col);
    int l = -1, r = min(n - row, m - col);
    while(r - l > 1) {
        int mid = l + (r - l) / 2;
        // printf("l = %d, mid = %d, r = %d\n", l, mid, r);
        
        if(inp[row + mid][col + mid] <= high)
            l = mid;
        else
            r = mid;
    }
    // printf("l = %d\n", l);
    
    return l;
}

void solve()
{
    int ret = 0;
    for(int i = 0; i < n; i++) {
        int l = lower_bound(inp[i], inp[i] + m, low) - inp[i];
        if(l == m)
            continue;
        int r = search(i, l);
        if(r == -1)
            continue;
        // printf("i = %d l = %d r = %d\n", i, l, r);
        
        ret = max(ret, r + 1);
    }
    
    printf("%d\n", ret);
}

int main()
{
    while(scanf("%d %d", &n, &m) == 2 && (n || m)) {
        for(int i = 0; i < n; i++)
            for(int j = 0; j < m; j++) 
                scanf("%d", &inp[i][j]);
        
        int q;
        scanf("%d", &q);
        for(int i = 0; i < q; i++) {
            scanf("%d %d", &low, &high);
            
            solve();
        }
        printf("-\n");
    }
    
    return 0;
}
{{< / highlight >}}

## [CF 551C GukiZ hates Boxes](http://codeforces.com/contest/551/problem/C)

這題的 check function 需要有個神觀察！

[C++版本](http://codeforces.com/contest/551/submission/22782056)

### AC Code

{{< highlight cpp "linenos=inline" >}}

import java.io.*;
import java.util.*;

public class C {
	public static boolean check(int[] inp, long mid, int k) {
		int[] cur = Arrays.copyOf(inp, inp.length);

		int last = inp.length - 1;
		while (last >= 0 && cur[last] == 0)
			last--;

		for (int i = 0; i < k; i++) {
			long t = mid - last - 1;
			while (t > 0 && last >= 0) {
				if (t - cur[last] >= 0) { // move box
					t -= cur[last];
					cur[last] = 0;

					while (last >= 0 && cur[last] == 0)
						last--;
				} else {
					cur[last] -= t;
					t = 0;
				}
				if (!(t > 0 && last >= 0))
					break;
			}
		}

		for (int i = last; i >= 0; i--)
			if (cur[i] != 0)
				return false;
		return true;
	}

	public static void main(String[] args) {
		MyScanner sc = new MyScanner();
		out = new PrintWriter(new BufferedOutputStream(System.out));

		int n = sc.nextInt(), k = sc.nextInt();

		int[] inp = new int[n];
		for (int i = 0; i < n; i++)
			inp[i] = sc.nextInt();

		long l = 0, r = (long) 1e18;
		while (r - l > 1) {
			long mid = (l + r) / 2;

			if (check(inp, mid, k) == true)
				r = mid;
			else
				l = mid;
		}

		out.println(r);

		out.close();
	}

	// PrintWriter for faster output
	public static PrintWriter out;

	// MyScanner class for faster input
	public static class MyScanner {
		BufferedReader br;
		StringTokenizer st;

		public MyScanner() {
			br = new BufferedReader(new InputStreamReader(System.in));
		}

		boolean hasNext() {
			while (st == null || !st.hasMoreElements()) {
				try {
					st = new StringTokenizer(br.readLine());
				} catch (Exception e) {
					return false;
				}
			}
			return true;
		}

		String next() {
			if (hasNext())
				return st.nextToken();
			return null;
		}

		int nextInt() {
			return Integer.parseInt(next());
		}

		long nextLong() {
			return Long.parseLong(next());
		}

		double nextDouble() {
			return Double.parseDouble(next());
		}

		String nextLine() {
			String str = "";
			try {
				str = br.readLine();
			} catch (IOException e) {
				e.printStackTrace();
			}
			return str;
		}
	}
}

{{< / highlight >}}

