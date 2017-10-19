---
title: "Exam A Problem B"
date: 2017-10-19T15:33:15+08:00
draft: false

tags: ["Math"]
categories: ["Competitive Programming"]

weight: 1000
---

費馬小定理應用題。

在 $mod\ p$ 底下，指數是對 $p - 1$ 取餘數。

<!--more-->

### Solution sketch

費馬小定理為 $x^{p - 1} \equiv 1 \pmod{p}$

如果要對 $x^y \pmod p$ 的 $y$ 取餘數，我們令 $y = (p - 1)r + s$

可以推得，

$$ x^y \equiv (x^{p - 1})^r(x)^s \equiv 1^r(x)^s \equiv x^s \pmod p $$

利用 $s = y \% (p - 1)$ 可以得知，指數在 $\pmod p$ 底下，是要 $mod\ p - 1$ 而不是 $mod\ p$

### AC Code

{{< highlight cpp "linenos=inline" >}}

To be added...

{{< / highlight >}}