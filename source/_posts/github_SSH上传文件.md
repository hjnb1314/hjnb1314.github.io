---
title: github上传文件过大时遇到的问题解决方法
categories:
  - 其他
top_img: ../img/index.jpg
---

# 在 Windows‑Git Bash 环境下解决 `git push` 报错 `SSL_ERROR_SYSCALL (errno 10054)` 并成功推送到 GitHub

> 适用场景：仓库大小 < 100 MB，HTTPS 推送频繁中断，需要切换到 SSH  
> 流程经过实测可行，如有差异请自行调整 `master`/`main` 分支名。

---

## 1. 现象与错误信息

```bash
Enumerating objects: 1858, done.
Compressing objects: 100% (1720/1720), done.
error: RPC failed; curl 7 OpenSSL SSL_read: SSL_ERROR_SYSCALL, errno 10054
fatal: the remote end hung up unexpectedly
```

- WinSock 10054 ⇒ 连接被远端或中间网络设备强制重置  
- 最常见原因：网络/代理不稳、HTTPS 长连接被中断  
- 解决思路：改用 **SSH 推送**，彻底绕过 HTTPS/SSL

---

## 2. 生成 SSH 密钥

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
# 连续三次回车（默认保存路径 ~/.ssh/id_ed25519，空口令亦可）
```

输出示例：

```
Your identification has been saved in /c/Users/<user>/.ssh/id_ed25519
Your public key has been saved in /c/Users/<user>/.ssh/id_ed25519.pub
```

---

## 3. 把公钥添加到 GitHub

1. 复制公钥内容  
   ```bash
   cat ~/.ssh/id_ed25519.pub | clip   # Windows 专用，自动复制到剪贴板
   ```
2. 浏览器登录 GitHub → **Settings → SSH and GPG keys → New SSH key**  
3. Title 任意，Key 粘贴 → **Add SSH key**

---

## 4. 切换本地仓库远程地址为 SSH

```bash
# 查看当前远程
git remote -v

# 修改为 SSH URL
git remote set-url origin git@github.com:<用户名>/<仓库>.git
```

示例：

```bash
git remote set-url origin git@github.com:hjnb1314/STM32_ESP8266_AccessControl.git
```

---

## 5. 测试 SSH 连通性

```bash
ssh -T git@github.com
```

- 首次连接会提示 “Are you sure you want to continue connecting (yes/no/[fingerprint])?”  
  输入 **yes** 并回车  
- 成功提示：  
  `Hi <用户名>! You've successfully authenticated, but GitHub does not provide shell access.`

---

## 6. 提交并推送代码

```bash
# 如仓库首次初始化
git init
git add .
git commit -m "Initial commit"

# 推送到 GitHub
git push -u origin master   # 若默认分支为 main → git push -u origin main
```

看到完整进度条且无报错即大功告成，刷新 GitHub 页面即可确认文件已上传。

---

## 7. 可选优化

| 目的 | 命令 / 操作 |
|------|-------------|
| 避免每次输入私钥口令（如果当初设置了口令） | `eval $(ssh-agent -s)`<br>`ssh-add ~/.ssh/id_ed25519` |
| 查看当前分支名 | `git branch --show-current` |
| 放大 HTTP 缓冲区（若仍想用 HTTPS） | `git config --global http.postBuffer 524288000` |
| 检查是否存在 ≥ 100 MB 的单文件 | 见下方脚本 |

```bash
# 查找大文件脚本
git rev-list --objects --all |
git cat-file --batch-check='%(objectname) %(objecttype) %(objectsize)' |
awk '$3 >= 100*1024*1024 {printf "%.1f MB\t%s\n", $3/1024/1024, $1}'
```

---

## 8. 总结

1. **SSH 永久免疫 HTTPS/SSL 中断**  
2. 三步：`ssh-keygen` → GitHub 添加公钥 → `git remote set-url`  
3. `git push -u origin master/main` 成功后，后续推送直接 `git push` 即可。

至此，`SSL_ERROR_SYSCALL (errno 10054)` 引发的推送失败已彻底解决。
