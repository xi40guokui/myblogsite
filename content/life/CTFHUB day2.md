\---

title: "CTFHUB Day2"

date: "2024-10-09T03:39:53+09:00"

draft: false

\---

### 1. **`apt-get`**：

- **用途**：用于 Linux 系统（如 Ubuntu、Debian）上的系统级包管理，负责安装、更新和管理系统工具和库。
- **适用场景**：系统管理层面的软件，如编译器、系统工具等。
- **示例**：`sudo apt-get install curl` 用于安装 `curl` 命令行工具。

### 2. **`brew` (Homebrew)**：

- **用途**：主要用于 macOS 和 Linux 系统的包管理器，帮助用户安装操作系统不自带的第三方开发工具和应用。
- **适用场景**：安装用户级的软件包，特别是在开发环境中使用。
- **示例**：`brew install git` 用于安装 Git 版本控制工具。

### 3. **`pip`** 和 **`pip3`**：

- **用途**：`pip` 和 `pip3` 都是 Python 的包管理工具，`pip` 用于 Python 2.x，而 `pip3` 专门为 Python 3.x 设计，用于安装和管理 Python 库。
- **适用场景**：用于安装 Python 项目中所需的依赖库。
- **示例**：`pip install requests` 或 `pip3 install requests` 安装 `requests` 库。

### 4. **`pipx`**：

- **用途**：专门用于安装独立的 Python 应用程序，并创建隔离环境，避免与系统中的其他应用发生依赖冲突。
- **适用场景**：安装 Python 命令行工具时使用，如 `black`（代码格式化工具）。
- **示例**：`pipx install black` 安装 Python 格式化工具 `black`。



dvcs-ripper

1. **扫描和发现暴露的版本控制目录**：
   - 当开发者或管理员在部署网站或应用时，没有正确地配置服务器导致版本控制系统的目录被暴露（例如 `.git/` 或 `.hg/`）。攻击者可以利用 `dvcs-ripper` 来扫描这些目录，尝试提取其中的元数据和源码。
   - 常见的暴露目录有 `.git/`（Git 版本控制系统）、`.hg/`（Mercurial 版本控制系统）等。
2. **恢复被删除的版本控制数据**：
   - `dvcs-ripper` 具有从部分暴露的版本控制数据中恢复项目历史的能力。即使开发者意识到问题并删除了部分文件，只要元数据或部分历史存在，攻击者仍然可能利用这些残留数据恢复出完整的源代码。
3. **提取敏感信息**：
   - 通过访问 `.git` 或 `.hg` 目录中的文件，攻击者可以恢复项目的源代码、配置文件，甚至是开发过程中留下的敏感信息，如密码、API 密钥、数据库凭证等。
4. **支持的版本控制系统**：
   - **Git**：可以扫描和提取 `.git/` 目录中的数据。
   - **Mercurial (Hg)**：可以扫描和提取 `.hg/` 目录中的数据。
   - **Subversion (SVN)**：可以扫描 `.svn/` 目录并提取 SVN 残留数据。
   - **Bazaar (Bzr)**：支持 `.bzr/` 目录中的数据提取。

dvcs-ripper git http://example.com/.git/

dvcs-ripper svn http://example.com/.svn/

zhuan@appledeMacBook-Pro-2 ~ % **git clone https://github.com/kost/dvcs-ripper**

zhuan@appledeMacBook-Pro-2 dvcs-ripper（切换到工具目录） % **./rip-svn.pl -v -u http://challenge-036a1905d1217c01.sandbox.ctfhub.com:10800/.svn**

**-v**：verbose详细信息  **-u**指定url 	**./**当前目录下运行

![image-00061010025729464](/Users/zhuan/Library/Application Support/typora-user-images/image-00061010025729464.png)

其中的**.svn**就是隐藏文件夹包含源代码

![image-00061010025819398](/Users/zhuan/Library/Application Support/typora-user-images/image-00061010025819398.png)

切换展示以后pristine就是**Subversion（SVN）** 版本控制系统中的一个特殊目录，存放着项目文件的**原始（精确、未修改）的版本**。它的作用是帮助 SVN 跟踪工作目录中文件的修改，并为文件的恢复或比较提供参照。

![image-00061010025946721](/Users/zhuan/Library/Application Support/typora-user-images/image-00061010025946721.png)

展开以后逐步打开目录，得到其中一个**.svn-base**（`.svn-base` 文件保存的是项目中某个文件的基线版本，也就是该文件在上次提交时的状态。SVN 使用这些基线版本来追踪文件的修改情况。）

使用**cat**命令打开后得到**flag**





![image-00061010062847009](/Users/zhuan/Library/Application Support/typora-user-images/image-00061010062847009.png)

使用dvcs-ripper工具的 **rip-hg.pl** 模块下载泄露的文件

切换目录docs-ripper

./rip-hg.pl -u -v http://challenge-5e4b7b902bdc3c57.sandbox.ctfhub.com:10800/.hg/

用tree命令打开刚才的.hg目录

![image-00061015205854638](/Users/zhuan/Library/Application Support/typora-user-images/image-00061015205854638.png)

发现last-message.txt文件以后，用cat命令打开本地文件发现“add flag”，用“grep -a -r flag”匹配关键字

1. **grep**：这是一个用于在文件中搜索特定模式的命令行工具。
2. **-a**：这个选项告诉 `grep**` 将所有文件都视为文本文件**，即使是二进制文件（如图片、压缩文件等），也会按照文本文件来处理并搜索其中的内容。通常情况下，`grep` 会跳过二进制文件，但加上 `-a` 选项后，它会尝试在二进制文件中搜索指定的模式。
3. **-r**：表示递归搜索**recursion**，`grep` 会从当前目录开始，递归地进入子目录，并在所有文件中查找符合条件的内容。
4. **flag**：这是要搜索的关键字，`grep` 将查找所有包含 "flag" 这个单词的行。

所以，`grep -a -r flag` 的作用是：

- 在当前目录以及所有子目录中的所有文件中（包括二进制文件），查找并显示包含 "flag" 关键字的行。

发现flag_398631200.txt文件，在原url添加后缀以后进行访问得到flag

![image-00061015210212602](/Users/zhuan/Library/Application Support/typora-user-images/image-00061015210212602.png)

![image-00061015210149153](/Users/zhuan/Library/Application Support/typora-user-images/image-00061015210149153.png)

