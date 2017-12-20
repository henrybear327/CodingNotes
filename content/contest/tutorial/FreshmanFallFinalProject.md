---
title: "大一程式設計期末專題"
description: "教學"
date: 2017-12-20T15:20:09+08:00
draft: false

tags: ["Implementation"]
categories: ["Tutorial"]

weight: 20
---

{{% panel theme="success" header="作業題目" %}}

{{%attachments pattern=".*(docx)"/%}}

{{% /panel %}}

<!--more-->

## 猜數字

數字分解 + 檢查重複數值。這些都是作業有的技巧歐！

### AC Code

{{< highlight cpp "linenos=inline" >}}

#include <stdio.h>

int check(int ans, int guess)
{
    int ansDecompose[4] = {0}, guessDecompose[4] = {0};
    int aCnt[10] = {0}, gCnt[10] = {0};
    int orig = guess;
	
	int i;
    for (i = 0; i < 4; i++) {
        ansDecompose[i] = ans % 10;
        aCnt[ans % 10]++;
        ans /= 10;
    }
    for (i = 0; i < 4; i++) {
        guessDecompose[i] = guess % 10;
        if (gCnt[guess % 10] != 0) {
            printf("%d is an invalid guess.\n", orig);
            return 0;
        }
        gCnt[guess % 10]++;
        guess /= 10;
    }

    int a = 0, b = 0;

    for (i = 0; i < 4; i++) {
        if (ansDecompose[i] == guessDecompose[i])
            a++;
        else if (aCnt[guessDecompose[i]] > 0) {
            b++;
        }
    }

    printf("%dA%dB\n", a, b);

    return a == 4;
}

void solve()
{
    int ans;
    scanf("%d", &ans);

    int guess;

    int history[9999] = {0};
    while (scanf("%d", &guess) == 1) {
        // check guessing history
        if (history[guess] == 1) {
            printf("%d is already guessed.\n", guess);
            continue;
        }
        history[guess] = 1;

        if (check(ans, guess) == 1)
            break;
    }
}

int main()
{
    int ncase;
    scanf("%d", &ncase);
    while (ncase--) {
        solve();
        if (ncase)
            printf("\n");
    }

    return 0;
}

{{< / highlight >}}

## 2048

要如何把四個方向有效率的實作移動呢？沒錯，就是透過旋轉！

只要實作其中一個方向的移動後，其他方向就是先選轉到剛剛實作好的方向後，做完，再轉回去就好啦！

注意 Game over 條件，別看錯了QQ

### AC Code

{{< highlight cpp "linenos=inline" >}}

#include <stdio.h>

void print(int inp[4][4])
{
    // printf("==============\n");
    int i, j;
    for (i = 0; i < 4; i++)
        for (j = 0; j < 4; j++)
            printf("%d%c", inp[i][j], j == 3 ? '\n' : ' ');
    //   printf("==============\n");
}

int checkSame(int inp[4][4])
{
    int i, j, k;
    int hasSame = 0;
    for (i = 0; i < 4; i++) {
        for (j = 0; j < 4; j++) {
            if (inp[i][j] == 0)
                continue;
            for (k = j + 1; k < 4; k++) {
                if (inp[i][k] == 0)
                    continue;
                if (inp[i][j] == inp[i][k]) {
                    hasSame = 1;
                } else {
                    j = k - 1;
                    break;
                }
            }
        }
    }
    return hasSame;
}

void rotate90Left(int inp[4][4])
{
    int i, j, tmp[4][4];
    for (i = 0; i < 4; i++)
        for (j = 0; j < 4; j++) {
            tmp[i][j] = inp[j][4 - 1 - i];
        }

    for (i = 0; i < 4; i++)
        for (j = 0; j < 4; j++)
            inp[i][j] = tmp[i][j];
}

// 0, no message
// 1, has 64
// 2, no move
int move(int inp[4][4])
{
    // printf("Before\n");
    // print(inp);

    int i, j, k;
    int has0 = 0;
    int has64 = 0;
    for (i = 0; i < 4; i++) {
        for (j = 0; j < 4; j++) {
            if (inp[i][j] == 0)
                continue;

            for (k = j + 1; k < 4; k++) {
                if (inp[i][k] == 0) // 0, continue
                    continue;
                if (inp[i][j] == inp[i][k]) { // same, merge
                    inp[i][j] += inp[i][k];
                    inp[i][k] = 0;
                    break;
                } else { // not the same, skip
                    break;
                }
            }
        }
    }

    for (i = 0; i < 4; i++) {
        int lastZero = -1;
        for (j = 0; j < 4; j++) {
            if (inp[i][j] == 64)
                has64 = 1;
            if (inp[i][j] == 0)
                has0 = 1;

            if (inp[i][j] != 0 && lastZero != -1) {
                inp[i][lastZero] = inp[i][j]; // move
                inp[i][j] = 0;                // fill zero
                lastZero = j;                 // crucial
                continue;
            }
            if (inp[i][j] == 0 &&
                lastZero == -1) // update last when no previous zero is present
                lastZero = j;
        }
    }

    // printf("after\n");
    // print(inp);

    if (has64 == 1)
        return 1;
    else if (has64 == 0 && has0 == 0) {
        int hasSame = checkSame(inp);
        if (hasSame == 0) {
            rotate90Left(inp);
            hasSame = checkSame(inp);
            rotate90Left(inp);
            rotate90Left(inp);
            rotate90Left(inp);
        }
        return hasSame == 0 ? 2 : 0;
    } else
        return 0;
}

void swap(int *a, int *b)
{
    int tmp = *a;
    *a = *b;
    *b = tmp;
}

void solve()
{
    int i, j, inp[4][4];
    for (i = 0; i < 4; i++)
        for (j = 0; j < 4; j++)
            scanf("%d", &inp[i][j]);
    char dir[10];
    scanf("%s", dir);

    // a left
    // s down
    // d right
    // w up

    // merge and move
    int ret = 0;
    if (dir[0] == 'a') {
        ret = move(inp);
    } else if (dir[0] == 'd') {
        rotate90Left(inp);
        rotate90Left(inp);
        ret = move(inp);
        rotate90Left(inp);
        rotate90Left(inp);
    } else if (dir[0] == 's') {
        rotate90Left(inp);
        rotate90Left(inp);
        rotate90Left(inp);
        ret = move(inp);
        rotate90Left(inp);
    } else { // w
        rotate90Left(inp);
        ret = move(inp);
        rotate90Left(inp);
        rotate90Left(inp);
        rotate90Left(inp);
    }

    print(inp);
    if (ret == 1)
        printf("You win\n");
    else if (ret == 2)
        printf("Game over\n");
}

int main()
{
    int ncase;
    scanf("%d", &ncase);
    while (ncase--) {
        solve();
        if (ncase)
            printf("\n");
    }

    return 0;
}

{{< / highlight >}}