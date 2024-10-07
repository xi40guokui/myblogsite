---
title: "First typora"
date: "2024-10-06T03:39:53+09:00"
draft: false
---

CTFHUB Day1

![image-00061007200233123](/Users/zhuan/Library/Application Support/typora-user-images/image-00061007200233123.png)

CTF**B Method是一种请求方法
curl -v -X CTFHUB http://challenge-8f997252b74e8345.sandbox.ctfhub.com:10800/index.php

curl执行http请求

-v verbose详细模式 讲请求和详细信息返回终端
-X CTFHUB 
-X用来指定请求办法 常见的如GET POST PUT DELETE
按照题目要求使用CTF**B Method 猜测是CTFHUB方法

你通过 `curl` 向指定的 URL 发送一个请求，请求方法是 `CTFHUB`。因为这是一个不常见的 HTTP 方法，服务器可能有特殊的处理逻辑，返回某种信息或者 `flag`。
