---
title: THJCC CTF write-up
description: 應該有蠻多人有聽過hugo或hexo這類靜態網頁產生器用來搭配github pages架設部落格的案例，甚至有不少常看到的個人部落格就是利用這種方式搭建的。接下來我們就利用stack模板來學習如何快速建立這樣的網站吧！
slug: firstPost
date: 2024-04-30 09:00:00+0800
image: page.png
categories:
    - write-ups
tags:
    - THJCC
    - CTF
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
---

# THJCC write-up
## Welcome
### Welcome 0x1

由題目可以看到第一部分flag
![截圖 2024-04-29 中午12.42.28](https://hackmd.io/_uploads/BkwHri2Z0.png)
在規則底下可以發現第二部分flag
![截圖 2024-04-29 中午12.44.22](https://hackmd.io/_uploads/SyR6Sihb0.png)

### Discord 0x1
由橫幅及身份組可以發現第一及第三部分flag

![截圖 2024-04-29 中午12.50.08](https://hackmd.io/_uploads/rkqmDo2bR.png)

利用ls可以看到flag.txt

![截圖 2024-04-29 中午12.51.09](https://hackmd.io/_uploads/rJZ1OohZR.png)

用cat獲得第二部分flag

![截圖 2024-04-29 中午12.54.11](https://hackmd.io/_uploads/r1AmOo3WC.png)


## Misc

### 原神帳號外流

利用wireshark打開封包檔，找到最後一個POST請求(最後的登入，也就是說終於登進了)，可以發現正確的帳號密碼

![image](https://hackmd.io/_uploads/ryuw5mpW0.png)

登入後即可獲得flag

### 出題者大合照！

首先先安裝steghide

```sudo apt－get install steghide```

接著用```info```指令查看是否有藏東西

![image](https://hackmd.io/_uploads/ByTUiXaWC.png)

將其extract出來即可得到flag

![image](https://hackmd.io/_uploads/Sk912Qab0.png)

### PyJail-0

首先先連上機器

![image](https://hackmd.io/_uploads/HySBNl6-C.png)

由source code可以發現會把輸入拿去做eval

![image](https://hackmd.io/_uploads/BJRHBea-0.png)

利用以下敘述bypass python sandbox並拿到shell

```
__import__("os").system("sh")
```

利用ls及cat即可拿到flag

### I want to go to Japan!

由敘述及圖片可以發現是在湯之川的某間神社，將角色丟到google搜索後可以發現**湯倉神社**的繪馬便是該角色，因此地點即為湯倉神社

### PyJail-1

首先連上機器

![image](https://hackmd.io/_uploads/B1tNpQ6bA.png)

由source code可以發現payload長度必須小於15個字元才能被執行，因此不能用上述的方法

![image](https://hackmd.io/_uploads/HyXjam6ZR.png)

首先，我們利用python的內建函數```breakpoint()```進入pdb

![image](https://hackmd.io/_uploads/ryjeAQabC.png)

接著就可以任意使用python敘述，仿造PyJail-0的方法即可拿到flag

## Web

### Empty

進入後可以發現空空如也，因此我們打開Devtool可以發現如下程式碼

![image](https://hackmd.io/_uploads/HkvFvQ6-R.png)

在cookie可以發現第一段flag

![image](https://hackmd.io/_uploads/HJJbuQTZ0.png)

進入註解的網頁可以看到第二段flag

![image](https://hackmd.io/_uploads/BypE_7a-A.png)

### Blog

首先我們先進login畫面

![image](https://hackmd.io/_uploads/HJAOumpZR.png)

由上可知，我們要用admin登入，而題目有說密碼有留在哪，因此我們退回到前一頁面可以找到一個很像密碼的```iloveshark```，利用其登入後即可得到flag

### Simplify

一開始我們使用題目給的測試帳號(test:test1234)登入
利用開發者工具可以發現有個cookie式用來標示username的
![截圖 2024-04-29 下午1.14.04](https://hackmd.io/_uploads/BJqrKhhZR.png)

我們將其改成admin，重新整理後可以看到

![截圖 2024-04-29 下午2.10.48](https://hackmd.io/_uploads/Bkqe5n2Z0.png)

查看原始碼

![截圖 2024-04-29 下午2.11.35](https://hackmd.io/_uploads/H15Nqnh-A.png)

可以從註解看到提示提示我們利用SSTI

![截圖 2024-04-29 下午3.04.29](https://hackmd.io/_uploads/B14iUa2Z0.png)


我們發現渲染內容出現在網址列@後

嘗試在網址列輸入```{{7*7}}```測試是否有SSTI

![截圖 2024-04-29 下午2.16.52](https://hackmd.io/_uploads/ByUPjn3ZC.png)

從output可以看出有SSTI，因此參考[這個網站](https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection)的SSTI payload，找到Python的payload(可由自動分析工具得知)，將渲染內容改成如下

```
{{ cycler.__init__.__globals__.os.popen('ls').read() }}
```

利用ls指令看該目錄下有甚麼檔案，可以得到

![image](https://hackmd.io/_uploads/SJZhxgpb0.png)

引此我們仿造上述方法利用cat拿到flag檔案中的內容

## Crypto

### 博元婦產科

將```TUFDVlZ7cFBwLnU0VXJmVGQzay52MEYubVB9Cg==```base64 decode後可以得到```MACVV{pPp.u4UrfTd3k.v0F.mP}```，再對其做rot即可得```THJCC{wWw.b4BymAk3r.c0M.tW}```

### Baby RSA

因為我不會解RSA，所以我直接抄picoCTF類題write-up的code，將數字改成這題的即可得到flag

[參考的write-up](https://github.com/Dvd848/CTFs/blob/master/2021_picoCTF/Mini_RSA.md)

### JPG^PNG=?

這題用到了signature code的特性，首先我們觀察source code可以發現他只取png的前7個位元組當key加密，而png的signature code剛好是前7個位元組，所以我們可以知道key為```[137,80,78,71,13,10,26,10]```，利用上述特性便可以將其解密回去原本的圖片

附上solution code:

```python
from itertools import cycle
def xor(a,b):
    return [i^j for i, j in zip(a, cycle(b))]
enc=open("/Users/dennis/infoSecurity/TSJCC/jpgpng/enc.txt","rb").read()
key=[137,80,78,71,13,10,26,10]
flag=bytearray(xor(enc,key))
open("/Users/dennis/infoSecurity/TSJCC/jpgpng/flag.jpg","wb").write(flag)
```

## Reverse

### Baby C

這題題目的source code是將輸入字串的各個元素跟120做xor後跟```char a[50]```的各個元素去比對，如果不一樣就是密碼錯誤，那麼反過來只要把```char a[50]```的各個元素跟120做xor就是正確答案了。

附上solution code:
```cpp
#include <stdio.h>

int main(int argc, const char * argv[]) {
    char string[50];
    int a[50]={44, 48, 50, 59, 59, 3, 16, 12, 12, 8, 11, 66, 87, 87, 15, 15, 15, 86, 1, 23, 13, 12, 13, 26, 29, 86, 27, 23, 21, 87, 15, 25, 12, 27, 16, 71, 14, 69, 75, 32, 59, 46, 53, 75, 63, 75, 8, 22, 11, 5};
    for(int i=0;i<50;i++)
    {
        string[i]=(char)(a[i]^120);
        printf("%c",string[i]);
    }
    return 0;
}
```

## Pwn

### nc

連上機器並回答完問題即可拿到flag

![image](https://hackmd.io/_uploads/B1AaWNpbA.png)
