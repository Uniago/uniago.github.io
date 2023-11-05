---
title: git command
date: 2023-11-04 13:18:42 +0800
categories: ['Tool', 'Git']
tags: ['学习', 'todo']
---

## Git 命令

> 这里将会记录一些常用的git命令

- 恢复删除的文件

```bash
git checkout commentId filename
```

- 强制推送

```bash
git push -f
```

- 在本地dev分支拉取远端main分支代码

```bash
git pull origin main --rebase
```

- 将本地dev分支推送到远端main分支

```bash
git push origin dev:main
```

- 删除被跟踪的文件和目录

```bash
git rm --cached .DS_Store
git rm -r --cached _sites/
```

