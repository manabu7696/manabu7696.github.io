---
title: 利用hugo+github pages+stack模板快速架設部落格
description: 應該有蠻多人有聽過hugo或hexo這類靜態網頁產生器用來搭配github pages架設部落格的案例，甚至有不少常看到的個人部落格就是利用這種方式搭建的。接下來我們就利用stack模板來學習如何快速建立這樣的網站吧！
slug: firstPost
date: 2024-01-28 16:00:00+0800
image: page.png
categories:
    - blogs
tags:
    - hugo 
    - github
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
---

## 利用stack模板創建repo
1. 進入[stack模板頁面](https://github.com/CaiJimmy/hugo-theme-stack-starter)
2. 按Use this tmeplate
![複製模板](1.png)
3. 選擇 create a new repository 

![創建repo](2.png)

4. 在 repository name 輸入 使用者名稱.github.io

![網站名稱](3.png)

5. 按下 create repository

## 新增codespace及基本設置
1. 進入剛剛創建的repo
2. 按下 code ，並選擇 codespace ，按下"+"符號新增 codespace

![code](4.png)

![codespace](5.png)

3. 等待創建 codespace
![創建過程](6.png)

4. 在終端機輸入 hugo server ，測試網站
![hugo server](7.png)

5. 修改設定檔config.toml及其他設定檔
![設定檔](8.png)

## 新增頁面
* 在 post 新增 blog 文章
* 在 page 新增網站頁面

## 網站上線
1. 在終端機輸入hugo
![hugo](9.png)

2. 進入原始檔控制頁面

![原始檔控制頁面](10.png)

3. 將更動包成commit

![commit](11.png)

![commit change](12.png)

記錄此次commit的更動，並按下確認
![紀錄commit](13.png)

4. 同步到 github

![同步](14.png)

5. 等github action 跑完
![github action](15.png)

6. 到 settings 的 pages 頁面修改渲染網頁來源至 gh-page 分支
![改網頁來源](16.png)

7. 再等 github action 跑完就大功告成

## 結語
這是我第一次寫技術文章，順便記錄自己架設部落格的歷程，~~可以水學習歷程了好欸~~，希望這篇文章能幫助到也想使用hugo架設部落格的人，也希望未來能生出更多優質的技術文章幫助到大家。