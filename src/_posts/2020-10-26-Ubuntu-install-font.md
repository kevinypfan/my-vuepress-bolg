---
category: memo
tags:
  - ubuntu
  - font
date: 2021-10-26
title: Ubuntu 安裝中文字型
lang: zh-Hant-TW
vssue-id: 7
---

開發時在本地端使用`pdf-creator-node`此套件時中文能正常顯示，但是上到server後中文字卻全部消失。

所以開始從安裝字型，我們使用的是 Ubuntu 18.04.5 LTS。 

![](https://img.shields.io/github/license/meteorlxy/vuepress-theme-meteorlxy.svg?style=flat)

# 安裝字型
```shell
cd /usr/share/fonts/opentype # 如沒此目錄，自行創建
cp *.ttf .
cp *.otf .
```