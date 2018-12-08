---
title: Git密钥配置
date: 2018-12-08 17:58:30
tags:
categories: Git
description: 
---


### 生成SSH-KEY
Git使用HTTPS协议，每次pull、 push都要输入密码，使用SSH协议，并配置ssh密钥，可免去每次都输密码的麻烦。

使用ssh-keygen工具生成，在Git安装目录下，我的是 `D:\Git\usr\bin`

输入命令：

```
ssh-keygen -t rsa -C "你的Git邮箱"
```

```
Enter file in which to save the key (/c/Users/flashbuy/.ssh/id_rsa): //密钥保存位置，回车默认
Enter passphrase (empty for no passphrase):                          //输入密码，直接回车
Enter same passphrase again:                                         //输入确认密码，直接回车
```


执行成功后的效果：

```
D:\Git\usr\bin>ssh-keygen -t rsa -C "wangcj@z-code.cn"
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/flashbuy/.ssh/id_rsa):
/c/Users/flashbuy/.ssh/id_rsa already exists.
Overwrite (y/n)? y
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/flashbuy/.ssh/id_rsa.
Your public key has been saved in /c/Users/flashbuy/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:mI7e1KlanBiZYiI8uE1fNomqsbiZCiEax/w/pIAWRk4 wangcj@z-code.cn
The key's randomart image is:
+---[RSA 2048]----+
|                 |
| E               |
|+                |
|o*   + +         |
|O+O = B S        |
|=@o= O.+ .       |
|* o.=o* o        |
|o* ..=..         |
|X.  o.+.         |
+----[SHA256]-----+

D:\Git\usr\bin>
```

### 添加 SSH-KEY 信息到 GitHub或GitLab

密钥位置在：`C:\Users\你的用户名\.ssh` 目录下，找到 `id_rsa.pub` 并使用编辑器打开

登陆GitHub，点击头像 -> Settings -> SSH and GPG keys -> New SSH key，然后复制上面的公钥内容粘贴进“Key”文本域内，“Title”文本域随便起个名字，点击 Add key。

### 添加完毕，验证公钥设置是否成功

```
ssh -T git@github.com
```
```
Hi wangchangjing! You've successfully authenticated, but GitHub does not provide shell access.
```



### Git配置用户名和邮箱
```
git config --global user.name "你的用户名"
git config --global user.email "你的邮箱"
# 注意：此用户名和邮箱是Git提交代码时用来显示你身份和联系方式的，并不是GitHub或Gitlab用户名和邮箱
```




> 参考：https://git-scm.com/book/en/v2/Git-on-the-Server-Generating-Your-SSH-Public-Key