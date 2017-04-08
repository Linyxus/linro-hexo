---
title: Git使用备忘
date: 2017-04-08 16:54:59
tags:
- github
- memo
categories:
- 0

---

## .gitignore书写方法

```
dir_name/
file_name
```

**Example:**

```
node_modules/
public/
db.json
```

## 删除仓库中已忽略的文件夹

```shell
git rm -r --cached dir_name/
```

**Example**:

```shell
git rm -r --cached .node_modules/
```

<!-- more -->

