---
title: "Pretest"
description: "大一前測"
date: 2017-11-28T13:53:56+08:00
draft: false

tags: ["eTutor", "Implementation"]
categories: ["Competitive Programming"]

weight: 5
---

<!--more-->

挑題： 叢壯滋

題目的解法都不唯一，我只是照著我的習慣打了 sample code。

英哩轉公里 和 判斷座標是否在圓形的範圍內，是基本功力題，如果做對前兩題代表你會寫程式，也會基本IO。

遞迴程式練習，是遞迴基本題，如果不會寫的話請一定要花時間畫圖弄懂 recursive call graph。

各位數和排序 是排序題，也算是看看大家進階一點的C/C++語言使用能力。解法多元，大家可以多多討論學習。

過半元素 和 一整數序列所含之整數個數及平均值，都是IO題，加上一點基本演算法。如果可以秒殺，恭喜！

## [題目2. 英哩轉公里](http://e-tutor.itsa.org.tw/e-Tutor/mod/programming/view.php?id=30747)

{{% expand "Solution" %}}
`%.1f` 會自動四捨五入喔！
{{% /expand %}}

### AC Code

{{< highlight cpp "linenos=inline" >}}

#include <bits/stdc++.h>

int main()
{
	int n;
	while(scanf("%d", &n) == 1)
		printf("%.1f\n", n * 1.6);
	return 0;
}

{{< / highlight >}}

## [題目3. 判斷座標是否在圓形的範圍內](http://e-tutor.itsa.org.tw/e-Tutor/mod/programming/view.php?id=30749)

{{% expand "Solution" %}}
$x^2 + y^2 = r^2$

這個公式大家記得嗎？
{{% /expand %}}

### AC Code

{{< highlight cpp "linenos=inline" >}}

#include <bits/stdc++.h>

int main()
{
	int x, y;
	while(scanf("%d %d", &x, &y) == 2) {
		if(x * x + y * y <= 200 * 200)
			printf("inside\n");
		else
			printf("outside\n");
	}
	return 0;
}

{{< / highlight >}}

## [題目12. 遞迴程式練習](http://e-tutor.itsa.org.tw/e-Tutor/mod/programming/view.php?id=30761)

{{% expand "Solution" %}}
遞迴的終止條件最重要了！
{{% /expand %}}

### AC Code

{{< highlight cpp "linenos=inline" >}}

#include <bits/stdc++.h>

int f(int n)
{
    if (n == 0 || n == 1)
        return n + 1;
    return f(n - 1) + f(n / 2);
}

int main()
{
    int n;
    while (scanf("%d", &n) == 1)
        printf("%d\n", f(n));

    return 0;
}

{{< / highlight >}}

## [題目26. 各位數和排序](http://e-tutor.itsa.org.tw/e-Tutor/mod/programming/view.php?id=30785)

{{% expand "Solution" %}}
這題做法很多！

可以開struct紀錄，也可以直接利用pair的特性。

最簡單的做法，直接在compare function做！

C++的compare function所利用的性質是如果$a$要在$b$前面就回傳`true`。
{{% /expand %}}

### AC Code (using pair)

{{< highlight cpp "linenos=inline" >}}

#include <bits/stdc++.h>

using namespace std;

typedef pair<int, int> ii;

int getSum(int n)
{
    int sum = 0;
    while (n > 0) {
        sum += n % 10;
        n /= 10;
    }
    return sum;
}

int main()
{
    int n;
    scanf("%d", &n);

    ii inp[n];
    for (int i = 0; i < n; i++) {
        int num;
        scanf("%d", &num);

        inp[i] = ii(getSum(num), num);
    }

    sort(inp, inp + n);
    for (int i = 0; i < n; i++)
        printf("%d%c", inp[i].second, i == n - 1 ? '\n' : ' ');

    return 0;
}

{{< / highlight >}}

### AC Code (using compare function)

{{< highlight cpp "linenos=inline" >}}

#include <bits/stdc++.h>

using namespace std;

typedef pair<int, int> ii;

int getSum(int n)
{
    int sum = 0;
    while (n > 0) {
        sum += n % 10;
        n /= 10;
    }
    return sum;
}

bool cmp(const int &a, const int &b)
{
    int sa = getSum(a);
    int sb = getSum(b);
    if (sa == sb)
        return a < b;
    return sa < sb;
}

int main()
{
    int n;
    scanf("%d", &n);

    int inp[n];
    for (int i = 0; i < n; i++)
        scanf("%d", &inp[i]);

    sort(inp, inp + n, cmp);
    for (int i = 0; i < n; i++)
        printf("%d%c", inp[i], i == n - 1 ? '\n' : ' ');

    return 0;
}

{{< / highlight >}}

### AC Code (using lambda)

謝謝黃鈺程幫忙！

{{< highlight cpp "linenos=inline" >}}

#include <bits/stdc++.h>

using namespace std;

typedef pair<int, int> ii;

int getSum(int n)
{
    int sum = 0;
    while (n > 0) {
        sum += n % 10;
        n /= 10;
    }
    return sum;
}

int main()
{
    int n;
    scanf("%d", &n);

    int inp[n];
    for (int i = 0; i < n; i++)
        scanf("%d", &inp[i]);

    sort(inp, inp + n, [](const int &a, const int &b) -> bool {
        int sa = getSum(a);
        int sb = getSum(b);
        if (sa == sb)
            return a < b;
        return sa < sb;
    });
    for (int i = 0; i < n; i++)
        printf("%d%c", inp[i], i == n - 1 ? '\n' : ' ');

    return 0;
}

{{< / highlight >}}

## [題目31. 過半元素](http://e-tutor.itsa.org.tw/e-Tutor/mod/programming/view.php?id=30781)

{{% expand "Solution" %}}
讀取一行一筆測資，請多多研究字串！

`fgets` vs `scanf`。

解法不唯一，聽說過半元素有$O(n)$解法。
{{% /expand %}}

### AC Code (strtok)

{{< highlight cpp "linenos=inline" >}}

#include <bits/stdc++.h>
using namespace std;

int main()
{
    char str[10000];
    while (fgets(str, 10000, stdin) != NULL) {
        char *num = strtok(str, "\n\r ");
        map<int, int> cnt;
        int total = 0;
        while (num != NULL) {
            cnt[atoi(num)]++;
            total++;
            num = strtok(NULL, "\n\r ");
        }

        int ok = INT_MIN;
        // 5 -> 3
        // 6 -> 4
        for (auto i : cnt)
            if (i.second >= (total + 2) / 2) {
                ok = i.first;
            }

        if (ok == INT_MIN)
            printf("NO\n");
        else
            printf("%d\n", ok);
    }
    return 0;
}

{{< / highlight >}}

### AC Code (getline + istringstream)

謝謝黃鈺程幫忙！

{{< highlight cpp "linenos=inline" >}}

#include <bits/stdc++.h>
using namespace std;
#define sz(x) (int(x.size()))
int main()
{
    string inp;
    while (getline(cin, inp)) {
        istringstream iss(inp);
        vector<int> v;
        int item;
        while (iss >> item) {
            v.push_back(item);
        }
        sort(v.begin(), v.end());
        int res = v[sz(v) / 2];
        int cnt = count(v.begin(), v.end(), res);
        if (cnt > sz(v) / 2)
            cout << res << endl;
        else
            cout << "NO" << endl;
    }
    return 0;
}

{{< / highlight >}}

## [題目33. 一整數序列所含之整數個數及平均值](http://e-tutor.itsa.org.tw/e-Tutor/mod/programming/view.php?id=30791)

{{% expand "Solution" %}}
字串讚讚！
{{% /expand %}}

### AC Code 

{{< highlight cpp "linenos=inline" >}}

#include <bits/stdc++.h>

int main()
{
    char inp[10000];
    while (fgets(inp, 10000, stdin) != NULL) {
        char *num = strtok(inp, "\r\n ");
        int total = 0;
        int sz = 0;
        while (num != NULL) {
            sz++;
            total += atoi(num);

            num = strtok(NULL, "\r\n ");
        }

        printf("Size: %d\n", sz);
        printf("Average: %.3f\n", total * 1.0 / sz);
    }
    return 0;
}

{{< / highlight >}}