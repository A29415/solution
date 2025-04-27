# solution
## 1 git命令无法对远程仓库进行操作

时间：2025.4.27

### 1.1 问题描述

有一天在windows环境下，对早已关联的远程仓库进行git push操作，发现报错，报错如下：

```markdown
ssh: connect to host github.com port 22: Connection refused
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

上网查询解决方案，大部分都是让切换443端口，但本人遇到的问题无法使用这个方案解决，报错如下：

```markdown
ssh: connect to host ssh.github.com port 443: Connection refused
```

### 1.2 解决方案

机缘巧合之下，终于找到一篇可行的文章。

文章链接：[GitHub 无法读取远程仓库 | 人人都懂物联网](https://getiot.tech/github/github-errata-port-443-connection-refused/)

大致如下步骤：

1. 找到GitHub服务器地址，手动解析，在终端使用nslookup命令，对 github.com 和 ssh.github.com 地址解析。

   ```c++
   C:\Users\A29415>nslookup github.com 8.8.8.8
   服务器:  dns.google
   Address:  8.8.8.8
   
   非权威应答:
   DNS request timed out.
       timeout was 2 seconds.
   名称:    github.com
   Address:  20.205.243.166
   ```

   ```c++
   C:\Users\A29415>nslookup ssh.github.com 8.8.8.8
   服务器:  dns.google
   Address:  8.8.8.8
   
   非权威应答:
   名称:    ssh.github.com
   Address:  20.205.243.160
   ```

2. 把这两个解析出来的IP地址填写到hosts文件里。

   windows系统：c:\Windows\System32\Drivers\etc\ 目录

   linux系统：/etc/hosts

   我以windows的hosts文件为例：

   ```c++
   ......
   # localhost name resolution is handled within DNS itself.
   #	127.0.0.1       localhost
   #	::1             localhost
   
   # GitHub
   20.205.243.166  github.com
   20.205.243.160  ssh.github.com
   ```

在这之后，我可以正常对远程仓库进行操作。
