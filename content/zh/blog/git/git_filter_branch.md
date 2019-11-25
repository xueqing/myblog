---
title: "重写分支"
authors: [kiki]
tags: [git]
categories: [blog]
draft: false
---

- [git filter-branch](#git-filter-branch)
- [参考](#%e5%8f%82%e8%80%83)

## git filter-branch

```sh
# 从所有提交中删除文件 filename
# 当提交中不包含此文件时，`rm filename`会失败提交，可使用`rm -f filename`
git filter-branch --tree-filter 'rm filename' HEAD
# 比 --tree-filter 更快
git filter-branch --index-filter 'git rm --cached --ignore-unmatch filename' HEAD
```

## 参考

- [git filter-branch doc](https://git-scm.com/docs/git-filter-branch)
