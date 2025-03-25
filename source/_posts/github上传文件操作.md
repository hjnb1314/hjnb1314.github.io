---
title: github上传文件操作
categories:
  - 其他
---

要把项目工程上传到 GitHub，你可以按以下步骤操作：
1. **注册并登录 GitHub**
如果你还没有 GitHub 账号，需要先在 GitHub 官网 注册。注册完成后，登录到你的账号。
2. **创建新的仓库**
登录 GitHub 后，点击右上角的 “+” 号，选择 “New repository”。
在 “Repository name” 中为你的项目取一个合适的名称。
可以添加一个描述，选择仓库的可见性（公开或私有）。
其他选项可以根据需要选择，最后点击 “Create repository” 完成创建。
3. **安装 Git**
你需要在本地安装 Git 工具，可从 Git 官网 下载适合你操作系统的版本并完成安装。
4. **配置 Git**
安装完成后，打开终端（Windows 用户可以使用 Git Bash），设置你的用户名和邮箱，这将关联到你在 GitHub 上的操作：

```
git config --global user.name "your_username"
git config --global user.email "your_email@example.com"
```

5. **初始化本地仓库**
在终端中，使用 `cd` 命令进入你项目工程所在的目录，然后执行以下命令来初始化一个新的 Git 仓库：

```
git init
```

6. **添加文件到暂存区**
使用以下命令将项目中的所有文件添加到 Git 的暂存区：

```
git add .
```

如果你只想添加特定的文件，可以把 `.` 替换为文件名。
7. **提交文件到本地仓库**
添加完文件后，需要提交这些更改到本地仓库，并附上一个描述性的提交信息：

```
git commit -m "Initial commit"
```

8. **关联本地仓库和 GitHub 仓库**
在 GitHub 上创建的仓库页面中，复制仓库的 HTTPS 或 SSH 地址。然后在终端中执行以下命令，将本地仓库与 GitHub 仓库关联起来：

```
git remote add origin <repository_url>
```

这里的 `<repository_url>` 是你从 GitHub 复制的仓库地址。
9. **推送文件到 GitHub**
最后，使用以下命令将本地仓库中的文件推送到 GitHub 仓库：

```
git push -u origin main
```

如果你使用的是旧版本的 Git，默认分支可能是 `master`，则命令为：

```
git push -u origin master
```

执行完以上步骤后，你的项目工程就成功上传到 GitHub 了。之后，你可以使用 `git add`、`git commit` 和 `git push` 命令来更新项目。


执行完以上步骤后，你的项目工程就成功上传到 GitHub 了。之后，你可以使用 `git add`、`git commit` 和 `git push` 命令来更新项目。

### 遇到错误的解决办法

```
fatal: unable to access 'https://github.com/hjnb1314/Esp8266_healthy_HuaweiCloud.git/': OpenSSL SSL_read: SSL_ERROR_SYSCALL, errno 10054
```

`OpenSSL SSL_read: SSL_ERROR_SYSCALL, errno 10054` 错误通常表示在与 GitHub 服务器进行 SSL 通信时连接被远程服务器重置，这可能由多种原因导致，下面为你提供一些可能的解决办法：
1. **检查网络连接**
网络不稳定、防火墙限制或者代理设置都可能引发此问题。你可以尝试以下操作：
    - 切换网络：尝试切换到不同的网络环境，比如从 Wi-Fi 切换到移动数据，或者相反。
    - 关闭防火墙或代理：暂时关闭本地的防火墙或者代理软件，然后再次尝试推送代码。例如，如果你使用的是 Windows 系统，可以在控制面板中找到防火墙设置并暂时关闭它；如果你使用了代理软件，如 Shadowsocks 等，将其关闭。
2. **检查 GitHub 状态**
GitHub 偶尔会出现服务中断的情况。你可以访问 GitHub 状态页面 查看 GitHub 当前的服务状态。若 GitHub 正在经历服务中断，你只能等待其恢复正常。
3. **更新 Git 和 OpenSSL**
使用旧版本的 Git 或 OpenSSL 可能会导致与 GitHub 服务器的通信出现问题。你可以尝试更新这些软件：
    - 更新 Git：访问 Git 官方下载页面，下载并安装最新版本的 Git。
    - 更新 OpenSSL：如果你使用的是 Windows 系统，Git 自带了 OpenSSL，更新 Git 通常也会更新 OpenSSL。
4. **重新配置远程仓库地址**
有时候，远程仓库地址可能会出现问题。你可以移除现有的远程仓库配置，然后重新添加：
```
# 移除现有的远程仓库配置
git remote remove origin
# 重新添加远程仓库配置
git remote add origin ...url
# 再次尝试推送
git push -u origin master
```
