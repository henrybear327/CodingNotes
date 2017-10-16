---
title: "PC2 在 Mac 上的使用方法"
date: 2017-10-16T18:54:02+08:00
draft: false

tags: ["PC2", "Linux", "Mac"]
categories: ["PC2"]

weight: 5
---

 PC2 其實是可以在 Mac 上面執行的！不用開啟 Windows 一樣可以上傳 judge 交作業歐～

<!--more-->

## 流程

首先，請先開啟終端機。

1. 安裝 [Homebrew](https://brew.sh): 在終端機中執行 `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`。
    * Homebrew 就像是 Linux 的 `apt-get`，要安裝東西就 `brew install [package name]` 就對啦！
2. 安裝 Java: 在終端機中執行 `brew cask install java`，他會幫你裝好並設定好 Java 的路徑
3. 下載上課用的 $PC2$ 軟體資料夾，放在你喜歡的地方。
4. 打開 $PC2$ 的資料夾，裡面會有一個 `bin` 資料夾。其中，會有一個 `pc2team` 的檔案。將該檔案拖曳至終端機中，按下enter，等待片刻就好啦！
    * 將檔案拖曳至終端機，應該會看到類似這樣的字串出現在終端機中：`/Users/[你的Mac目前使用者帳號]/[中/間/一/堆/路/徑/...]/pc2-9.4.1_student/bin/pc2team`  