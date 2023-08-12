---
title: "关于我的GitHub Page曲折Git命令"
date: 2023-08-12
categories: ['开始的开始']
---

## 1. 如何强制推送本地项目覆盖远程仓库

| 命令 | 含义 |
| --- | --- |
| git init | 初始化Git项目 |
| git branch -m main | 创建main分支（默认master） |
| git remote add origin URL | 关联远程仓库 |
| git branch --set-upstream-to=origin/main main | 将本地main分支关联到远程main分支 |
| git push -u origin main --force | 强制推送本地Git项目覆盖远程仓库项目 |