@[TOC](【Git】将本地工程推送至 GitHub 仓库的基本流程)

# 1 Prerequisite
## 1.1 Git 更新

>参考文档: [Link](https://git-scm.com/download/linux)

```bash
sudo add-apt-repository ppa:git-core/ppa
sudo apt update
sudo apt install git
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/41a6aa4a06aff38fa9e9e3608473d9c3.png#pic_center)

## 1.2 Git 设置

- 设置 Git 的用户名和邮箱

```bash
git config --global user.name "username"
git config --global user.email "email"
```

如果不设置用户名和邮箱，就无法执行 `git commit`，报错如下: `*** Please tell me who you are.`

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/f3117641e07f3ac3e3987a30f9bf5a61.png#pic_center)


- 设置 Git 的默认文本编辑器 (VSCode or Vim)

```bash
git config --global core.editor "code --wait --new-window"

git config --global core.editor "vim"
```

- 设置 Git 的默认分支名为 `main`

在 Git 2.28 及以上版本才会生效

```bash
git config --global init.defaultBranch main
```

- 查看设置信息

```bash
git config --global --list
```

## 1.3 GitHub 身份验证
配置 GitHub 对本机的身份验证，参考我的这篇文章: [Link](https://blog.csdn.net/G_C_H/article/details/127981765)

# 2 基本流程

## 2.1 在 GitHub 上创建一个新的仓库

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/79bb8b57d34ba62a4e4018bf765b0b6a.png#pic_center)

## 2.2 初始化本地仓库

1. 在工作目录初始化
```bash
git init
```

2. 将文件添加到暂存区

- 可以用 `.gitignore` 文件来忽略某些文件或目录，书写格式参考文档: [Link](https://www.git-scm.com/docs/gitignore#_pattern_format)
- 用 `.gitkeep` 文件来跟踪空文件夹以保持目录结构

```bash
git add .
```

3. 提交文件到本地仓库

简单的信息用 `-m` 参数，复杂的信息建议使用 `git commit` 打开文本编辑器

```bash
git commit -m "Initialize repository"
# or
git commit
```

如果在 `git push` 之前要修改本次提交的信息，可以用下面的命令，参考文档: [Link](https://docs.github.com/zh/pull-requests/committing-changes-to-your-project/creating-and-editing-commits/changing-a-commit-message)

```bash
git commit --amend
```


4. 查看本地分支
```bash
git branch
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/5459333fc1e08a6fcb6e2111786b5d91.png#pic_center)


## 2.3 添加远程仓库引用

将 `origin` 设置为远程仓库的引用（别名），通常使用 `origin` 作为默认远程仓库的名字，也可以用其他自定义的名称


```bash
git remote add origin [ssh url]
# 查看信息
git remote -v 
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/a2237896facb57ed6773a40219340d3c.png#pic_center)



如果报错如下: `fatal: remote origin already exists.`，则先删除原有的远程仓库配置，再进行关联

```bash
git remote rm origin
```

==如果远程仓库是新建的空仓库，可以直接将本地分支推送至远程仓库==，无需后面的步骤

```bash
git push -u origin main
```

- -u: 设置本地分支与远程分支的上游（upstream）相关联，后面可以只使用 git push 或 git pull，Git 会知道将本地的 main 分支与远程的 main 分支进行同步

## 2.4 更新远程仓库状态

从远程仓库中获取最新的提交和数据，并将其下载到本地仓库，但它并不会合并这些提交到当前的工作分支，即不会改变工作区文件内容。

1. 获取远程仓库的最新状态

```bash
git fetch origin
```


2. 查看本地分支和远程跟踪分支

```bash
git branch -a
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/77ea0ce6e4bfdaf5260c5be87cb2c50e.png#pic_center)

## 2.5 关联本地分支与远程分支

设置了分支关联，就可以轻松地使用 `git pull` 和 `git push` 等命令，而不需要每次都指定分支的名称。

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/699f9ec421e69a1268fc576fd1aaf5cd.png#pic_center)

```bash
git branch -u origin/main main
```
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/8c0d6e0f925b44f4a05adad1744c128e.png#pic_center)


- 查看本地分支与远程分支之间的关联信息
```bash
git branch -v
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/cb7f54f4e056693ecbab40bad40ea31d.png#pic_center)

## 2.6 合并本地分支与远程分支

将远程分支合并入当前本地分支

```bash
git merge origin/main
```

若执行后报错: `fatal: refusing to merge unrelated histories`，则加参数 `--allow-unrelated-histories`

```bash
git merge origin/main --allow-unrelated-histories
```

## 2.7 处理合并冲突

>参考文档: [Link](https://docs.github.com/zh/pull-requests/collaborating-with-pull-requests/addressing-merge-conflicts/resolving-a-merge-conflict-using-the-command-line)

合并时可能会有文件存在冲突，如下图:

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/d59ad42d5cbbcece31b82e03fb91b70d.png#pic_center)

 输出信息中提示 `fix conflicts and then commit the result`:

1. 打开存在冲突的文件

冲突的地方会以普通文本的方式标注出来：
- `=====` 为分割线
- `<<<<<` 为当前分支内容
- `>>>>>` 为合并进来的分支内容

```txt
<<<<<<< HEAD
....
=======
....
>>>>>>> xxx
```
2. 自行对冲突的地方进行修改，VSCode 上比较容易操作

3. 处理完冲突后，重新 commit

```bash
git add .
git commit -m "Resolved merge conflicts"
```

4. 再次合并，查看是否还存在问题
```bash
git merge origin/main
# 输出 Already up to date.
```

## 2.8 推送至远程仓库

如果当前分支关与远程分支进行了关联，这里就不用写分支名了，直接 `git push`

```bash
git push origin main
```

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/e6fe1649aa23cc77e1a3f51dd55afcd7.png#pic_center)


如果想要删除 GitHub 仓库中已 push 的某文件或目录，并且在工作区保留，以 `.vscode` 文件夹为例:

1. 在暂存区中删除

```bash
git rm --cached -r .vscode/
```

2. 将 `.vscode/` 写入 `.gitignore` 文件，确保之后不会再跟踪

3. `commit` & `push`

```bash
git commit -m "Update .gitignore to ignore .vscode folder"

git push origin main
```

# 3 在新的分支中继续开发

建立新的分支，在其中实现新的功能或是完善之前的工程，这样在开发过程中就不会影响到之前完整的工程代码，待完成后，可以向 `main` 分支发起合并请求。


1. 创建、切换到本地新分支
```bash
git branch develop
git checkout develop
```
上面的可以用一条命令代替
```bash
git checkout -b develop
```

2. 创建远程新分支

```bash
git push origin develop:develop
# 查看所有分支
git branch -a
```

3. 远程分支与本地分支关联

```bash
git branch --set-upstream-to=origin/develop develop
# 查看信息
git branch -vv
```
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/b6c684b2fbf3b71afc6916e43cdf49b8.png#pic_center =600x)
4. `push` & `pull` 测试

>进行了上一步的关联，如果只对 `origin/develop` 进行操作，就不用加分支名

```bash
git pull
git push
```

>但如果没有进行关联，就要加上分支名

```bash
git pull origin develop:develop
git push origin develop:develop
```

# 4 删除远程分支

1. 删除 GitHub 仓库的分支
```bash
git push origin -d <remote-branch-name> 
```

2. 删除本地存储的远程分支

若 `git branch -r` 中仍存在该远程分支名，可以用下面的命令删除

```bash
git branch -d -r origin/<remote-branch-name>
```







