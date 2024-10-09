---
title: "CTFHUB"
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

### 小结

简单地说，当你只输入 `curl http://example.com`，`curl` 会：

- 默认发送一个 HTTP `GET` 请求。
- 使用 HTTP/1.1 协议。
- 添加一些默认的请求头（如 `User-Agent`、`Host`）。
- 打印响应内容到终端。



<img src="/Users/zhuan/Library/Application Support/typora-user-images/image-00061008000815289.png" alt="image-00061008000815289" style="zoom:50%;" />

http://challenge-9ae6005042f8a194.sandbox.ctfhub.com:10800/



```python
import requests

base_url="http://challenge-a8315587307d42ea.sandbox.ctfhub.com:10800/"

file_name=["web","website","backup""back","www","wwwroot""temp"]

extensions=["tar", "zip", "tar.gz","rar"]

for i in file_name:
    for j in extensions:
        url=base_url+i+"."+j
        try:
            response = requests.get(url, timeout=5)
            if response.status_code == 200:
                print(f"File Found: {url}")
                with open(f"{i}.{j}", "wb") as file:
                    file.write(response.content)
        except requests.exceptions.RequestException as e:
            print(f"Error: {e}")
```

网站源码备份文件名："web","website","backup""back","www","wwwroot""temp"

备份文件后缀："tar", "zip", "tar.gz","rar"

url=base_url+i+"."+j   最终的url

response = requests.get(url, timeout=5)		requests.get()返回响应数据，对象是response，状态码





***with open(f"{i}.{*j**}", "wb") as file:******
                    file.write(response.content)******		



**`with open(f"{i}.{j}", "wb") as file`**

- **`with open(...) as file`**：
  使用 `with open` 语句来打开一个文件，确保在文件操作完成后自动关闭文件，避免资源泄露或文件损坏。
- **`f"{i}.{j}"`**：
  这里使用了 f-string（格式化字符串），表示文件的名称。其中 `i` 和 `j` 是变量，它们的值会被替换到字符串中，构成文件名。
  例如，如果 `i=1` 且 `j=png`，文件名将是 `"1.png"`。
- **`"wb"`**：
  这是文件打开模式。
  - **`w`**：表示以写入模式打开文件。如果文件已经存在，则会被覆盖（即清空内容重新写入）。如果文件不存在，则会创建一个新文件。
  - **`b`**：表示以二进制模式打开文件。与文本模式（`"w"`）不同，二进制模式适用于处理非文本文件，如图像、音频、视频等文件。



**`file.write(response.content)`**

- **`file.write(...)`**：
  使用 `write` 方法将内容写入到打开的文件中。
- **`response.content`**：
  这里是之前的 `requests.get()` 请求返回的 `Response` 对象的 `content` 属性，包含了服务器返回的二进制数据（如图片、音频等）。
- 将 `response.content` 中的二进制数据写入到打开的文件中，完成后自动关闭文件。



1. **`except requests.exceptions.RequestException as e`**

   - 这里的 `except` 块用于捕获在 `try` 块中发生的所有 `requests` 库相关的异常，并将其赋值给变量 `e` 以供处理。

   - `requests.exceptions.RequestException`

      

     是requests库中所有异常的基类，捕获这个异常就意味着可以处理所有与requests库相关的错误，包括：

     - **`ConnectionError`**：网络连接错误，例如网络断开或服务器不可达。
     - **`HTTPError`**：HTTP 请求返回错误状态码（例如 404，500 等）。
     - **`Timeout`**：请求超时，在指定时间内未收到服务器响应。
     - **`TooManyRedirects`**：请求遇到过多的重定向。

   - 由于使用了通用的 `RequestException`，所以能捕获所有的上述错误以及其他 `requests` 库中定义的异常。

2. **`print(f"Error: {e}")`**

   - **`print()`**：将错误信息打印到控制台，帮助用户了解发生了什么错误。

   - `f"Error: {e}"`

     ：使用 f-string（格式化字符串）来打印错误信息，其中e是异常对象，{e}

     会被替换成异常的详细描述。

     - 例如，如果发生了连接错误，`e` 可能包含的信息类似于 `"ConnectionError: Failed to establish a new connection"`。

![image-00061008003944636](/Users/zhuan/Library/Application Support/typora-user-images/image-00061008003944636.png)

当开发人员在线上环境中对源代码进行了备份操作，并且将备份文件放在了web目录下，并将其进行备份，将其命名为:xxxx.bak。而大部分Web server对 *.bak 文件并不做任何处理，导致可以直接下载，从而获取到某个文件的源代码。



http://challenge-7dc6c7cb064813cd.sandbox.ctfhub.com:10800/

对于这个网站，末尾加index.php就能访问php网页的源码，再加.bak就能访问php文件的源码备份文件





![image-00061008031406915](/Users/zhuan/Library/Application Support/typora-user-images/image-00061008031406915.png)

![image-00061008031413959](/Users/zhuan/Library/Application Support/typora-user-images/image-00061008031413959.png)

1. **了解 Vim 缓存机制**：
   Vim 在编辑文件时会生成交换文件（swap file），通常命名为 `.filename.swp` 或 `.filename.swo`。这些文件用于在编辑过程中保存临时内容。
2. **利用缓存文件中的敏感信息**：
   在 CTF 中，缓存文件可能会泄露原始文件的内容或敏感信息，如 `flag`。你可以尝试直接下载这些文件并查看内容。
3. **恢复缓存文件**：
   使用 Vim 的 `-r` 参数来恢复交换文件，以查看编辑会话中的内容，或者用 `strings` 工具来提取其中的可读字符串。

plus：http://challenge-a10e77ecaf076bce.sandbox.ctfhub.com:10800/

访问他的swp文件时，里面的 `.index.php.swp` 是 Vim 编辑器生成的交换文件（swap file），而其中的 `.` 符号是文件名的一部分。这个 `.`表示该文件是一个**隐藏文件**，按照 Linux/Unix 文件系统的惯例，文件名前面的 `.` 表示该文件默认是隐藏的。

你可以尝试在浏览器中输入这个完整的 URL（包括 `.`），以下载并查看这个 `.swp` 文件的内容，通常这个交换文件可能会包含敏感信息或者正在编辑的内容，比如 `flag`。

URL 中的 `/` 是路径分隔符，表示目录结构。例如，`/index.php.swp` 表示在网站的根目录下访问 `index.php.swp`文件。路径结构类似于文件系统中的目录，例如：

- `http://example.com/dir1/file.php`：这里的 `dir1` 是目录，`file.php` 是文件。
  在 `http://challenge.../.index.php.swp` 中，`/` 分隔了路径部分。



##### kali中wget下载后vim恢复原有文件查看

wget http://challenge-a10e77ecaf076bce.sandbox.ctfhub.com:10800/.index.php.swp
![image-00061008041808764](/Users/zhuan/Library/Application Support/typora-user-images/image-00061008041808764.png)

它支持 HTTP、HTTPS 和 FTP 协议，可以用来下载网页、文件、以及镜像整个网站。`wget` 可以处理递归下载、断点续传，还支持通过脚本进行自动化下载。它通常预装在大多数 Linux/Unix 系统中，但也可以在其他操作系统上安装。使用方式非常简单，例如 `wget <URL>` 就可以开始下载指定的文件。

vim -r .index.php.swp

这是kali预装的vim编辑器，这将使用 Vim 恢复交换文件中的内容，尝试查看文件中是否有 `flag` 或其他敏感信息。按 `Enter` 进入 Vim 编辑界面，然后可以使用 Vim 的搜索功能来寻找特定内容
