@[TOC](【Git】Git 命令行操作)

---
# 1 基本概念
## 1.1 工作区、暂存区、本地库
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/b6a20d03758a16083a67baad47294862.png#pic_center)

## 1.2 远程库

基于网络服务器的远程代码仓库，一般称为远程库，例如：`GitHub`

---
# 2 Git 本地相关操作

## 2.1 设置用户信息

1. 查看信息

```bash
git config --global --list  # 查看当前 git 的配置信息
```

2. 修改信息

```bash
git config --global user.name "username"
git config --global user.email "email"
```

## 2.2 本地仓库操作

1. 本地仓库初始化

```bash
git init
```

> **通过 `git status` 命令，查询当前仓库的状态。**
>
> ```shell
> git status
> 
> """
> On branch master
> 
> No commits yet
> 
> Untracked files:
>   (use "git add <file>..." to include in what will be committed)
>         version1.1/
> """
> ```
>
> **可看到当前仓库是空的，但存在 `Untracked files`。**

2. 将文件加入暂存区

```bash
git add .  # 添加当前路径的全部文件
git add xxx  # 也可指定某一文件
```

> **`git status` 查询仓库状态，可看到已被提交的文件。**
>
> ```bash
> git status
> 
> """
> On branch master
> 
> No commits yet
> 
> Changes to be committed:
>   (use "git rm --cached <file>..." to unstage)
>         new file:   version1.1/CORE/core_cm3.c
>         new file:   version1.1/CORE/core_cm3.h
>         new file:   version1.1/CORE/core_cmFunc.h
>         new file:   version1.1/CORE/core_cmInstr.h
> """
> ```
>
> **`git add` 命令并没有把文件提交到本地 `Git` 仓库，而是把文件添加到了「临时缓冲区」。**

3. 将文件从暂存区删除

```bash
git rm --cached file
git rm --cached -r folder

git rm file  # 同时删除本地文件
```

4. 将暂存区内容提交到本地仓库

```bash
git commit -m "备注信息"
```

> **通过 `git log` 命令，查询历史提交记录。**


## 2.2 分支操作

> - 一个分支对应一个本地库
> - `HEAD` 所指向的是当前分支

1. 查询分支

- 查询所有分支（本地库和远程库）
```bash
git branch -a  
```
- 查询所有远程库分支
```bash
git branch -r
```
- 查询所有本地库分支
```bash
git branch
```

2. 创建、切换分支

- 创建和切换
```bash
git branch xxx
git checkout xxx
```

- 创建并切换
```bash
git checkout -b xxx
```

3. 重命名分支

>**git branch -m 原分支名 新分支名**
>直接修改当前分支：git branch -m 新分支名
```bash
git branch -m xxx
```

4. 删除分支

```bash
git branch -d xxx
```

5. 合并分支

> 只会修改执行合并命令时所处的当前分支

```bash
git merge  
```

- **合并冲突**

> 当合并分支时，**两个分支在同一文件的同一位置有两套完全不同的修改**。此时 Git 无法决定选用哪个修改，需要我们**人为干预**。

---
# 3 Git 远程库相关操作
>对远程库的操作是基于别名的（别名类似于句柄）

1. 创建远程库别名


> **git remote add 别名 仓库链接[https or ssh]**

```bash
git remote add origin git@github.com:GChenH/NB-IoT.git 
```

2. 查看远程库别名

> **git remote**

```bash
git remote
"""
origin
"""

git remote -v  # 可以看到每个别名的实际链接地址
"""
# 因为既可以拉取也可以推送，所以有两个。
origin  git@github.com:GChenH/NB-IoT.git (fetch)
origin  git@github.com:GChenH/NB-IoT.git (push)
""
```

3. 删除远程库别名


> **git remote rm [别名]**

```bash
git remote rm origin
```

4. 修改远程库别名

> **git remote rename old_name new_name**

```bash
git remote rename origin dev
```

5. 本地库推送到远程库

> **git push 远程库别名 本地库分支:远程库分支**

```bash
# 本地分支与远程分支相同 可省略冒号后的
git push origin master  
```

6. 远程库拉取到本地库


> **git pull 远程库别名 远程库分支:本地库分支**

- 取回 origin/master 分支，再与本地的 master 分支合并
```bash
# 合并当前本地分支与远程分支 可省略冒号后的
git pull origin master  
```

- 取回 origin/master 分支，再与本地的 dev 分支合并

```bash
git pull origin master:dev
```


7. 下载远程仓库到本地


> **git clone [url] -b [tag/branch]**
>
> clone 的仓库已完成初始化以及创建远程库别名(默认为 `origin`)

```bash
git clone git@github.com:GChenH/NB-IoT.git
```

8. 删除远程库分支


> **git push 远程库别名 -d 远程库分支**

```bash
git push origin -d main

"""
To github.com:GChenH/NB-IoT.git
 - [deleted]         main
"""
```
