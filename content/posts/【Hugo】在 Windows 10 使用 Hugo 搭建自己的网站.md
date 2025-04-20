# 1 准备工作

## 1.1 安装 Powershell

Hugo 的文档中提到，如果是 Windows 系统，不要使用 Command Prompt 和 Windows PowerShell，要使用 PowerShell。

![image-20250420000015458](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250420000015458.png)

Windows PowerShell 和 PowerShell 是不一样的，两者间具体的差异可以查阅文档: [Differences between Windows PowerShell 5.1 and PowerShell 7.x - PowerShell | Microsoft Learn](https://learn.microsoft.com/en-us/powershell/scripting/whats-new/differences-from-windows-powershell?view=powershell-7.5)

> Windows PowerShell 5.1 基于 .NET Framework v4.5 构建。 随着 PowerShell 6.0 的发布，PowerShell 成为基于 .NET Core 2.0 构建的开源项目。 从 .NET Framework 迁移到 .NET Core 允许 PowerShell 成为跨平台解决方案。 PowerShell 在 Windows、macOS 和 Linux 上运行。
>
> 在 PowerShell 语言方面，Windows PowerShell 和 PowerShell 之间几乎没有差异。 最显著的差异在于 Windows 和非 Windows 平台之间的 PowerShell cmdlet 的可用性和行为，以及源于 .NET Framework 和 .NET Core 之间的差异的更改。



使用 WinGet 安装 PowerShell，参考文档: [Link](https://learn.microsoft.com/en-us/powershell/scripting/install/installing-powershell-on-windows?view=powershell-7.5#install-powershell-using-winget-recommended)

```bash
winget search Microsoft.PowerShell
winget install --id Microsoft.PowerShell --source winget
```

![image-20250420001345022](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250420001345022.png)

## 1.2 安装 Hugo

> 参考文档: [Link](https://gohugo.io/installation/windows/#prebuilt-binaries)

1. 下载可执行文件: [Link]([Releases · gohugoio/hugo](https://github.com/gohugoio/hugo/releases))

![image-20250419234444603](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250419234444603.png)

2. 解压后设置环境变量

此电脑 --- 属性 --- 高级系统设置 --- 环境变量 --- 用户变量 Path --- 新建，输入解压后 `hugo.exe` 的路径

![image-20250419234932898](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250419234932898.png)

3. 测试

![image-20250419235305484](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250419235305484.png)

## 1.3 安装 Git

下载安装包: [Link](https://git-scm.com/downloads/win)，安装即可，这里不再赘述。

# 2 选择主题模板

在 [Hugo Themes](https://themes.gohugo.io/) 选择自己喜欢的主题，下面我将以 Saral 为例，搭建自己的网站。

![image-20250420002546406](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250420002546406.png)

# 3 搭建本地站点

> 参考文档: [Link](https://gohugo.io/getting-started/quick-start/)

1. 创建新站点

```bash
hugo new site homepage --format yaml
```

![image-20250420010640091](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250420010640091.png)

2. 下载 Saral 主题

```bash
cd homepage
git init
git submodule add --depth=1 git@github.com:dipeshsingh253/saral.git themes/Saral
git submodule update --init --recursive # needed when you reclone your repo (submodules may not get cloned automatically)

# 后续更新内容
git submodule update --remote --merge
```

3. 将主题设置为 Saral

在 `hugo.yml` 中加入:

```
theme: ["Saral"]
```

4. 本地启动测试

```bash
hugo server
```

![image-20250420011741372](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250420011741372.png)

# 4 Host on GitHub Pages

> 参考文档: [Link](https://gohugo.io/host-and-deploy/host-on-github-pages/)

1. 创建空仓库

Note: 仓库名称最好与 username 一致，可以大小写不同，如 Guochh01 / guochh01.github.io，这样之后创建好的 site URL 中就不会包含 username。

![image-20250421011301258](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250421011301258.png)

2. 创建 `.gitignore` 文件

```bash
cd homepage
touch .gitignore
```

复制下面的内容到 `.gitignore` 文件，内容来自 PaperMod 主题的 example 仓库: [adityatelange/hugo-PaperMod at exampleSite](https://github.com/adityatelange/hugo-PaperMod/tree/exampleSite)

```txt
# Compiled Object files, Static and Dynamic libs (Shared Objects)
*.o
*.a
*.so

# Folders
_obj
_test

# Architecture specific extensions/prefixes
*.[568vq]
[568vq].out

*.cgo1.go
*.cgo2.c
_cgo_defun.c
_cgo_gotypes.go
_cgo_export.*

_testmain.go

*.exe
*.test

/public
.DS_Store
.hugo_build.lock
resources/_gen/
```

3. 将本地站点 push 进仓库

```bash
git remote add origin [ssh url]
git add -A
git commit -m "first commit"
git push -u origin main
```

4. 在 `.github/workflows` 路径下创建 `hugo.yaml` 文件

```bash
mkdir -p .github/workflows
touch .github/workflows/hugo.yaml
```

复制下面的内容到 `hugo.yaml` 文件，只需修改下面两项：

- branch name: on --- push --- branches
- Hugo version: jobs --- build --- env --- HUGO_VERSION，我的是 0.146.1

```yaml
# Sample workflow for building and deploying a Hugo site to GitHub Pages
name: Deploy Hugo site to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches:
      - main

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
concurrency:
  group: "pages"
  cancel-in-progress: false

# Default to bash
defaults:
  run:
    shell: bash

jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.146.1
      HUGO_ENVIRONMENT: production
      TZ: America/Los_Angeles
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb \
          && sudo dpkg -i ${{ runner.temp }}/hugo.deb
      - name: Install Dart Sass
        run: sudo snap install dart-sass
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive
          fetch-depth: 0
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v5
      - name: Install Node.js dependencies
        run: "[[ -f package-lock.json || -f npm-shrinkwrap.json ]] && npm ci || true"
      - name: Cache Restore
        id: cache-restore
        uses: actions/cache/restore@v4
        with:
          path: |
            ${{ runner.temp }}/hugo_cache
          key: hugo-${{ github.run_id }}
          restore-keys:
            hugo-
      - name: Build with Hugo
        run: |
          hugo \
            --gc \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/" \
            --cacheDir "${{ runner.temp }}/hugo_cache"
      - name: Cache Save
        id: cache-save
        uses: actions/cache/save@v4
        with:
          path: |
            ${{ runner.temp }}/hugo_cache
          key: ${{ steps.cache-restore.outputs.cache-primary-key }}
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  # Deployment job
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

5. Commit & Push

```bash
git branch -u origin/main main
git add -A
git commit -m "Create hugo.yaml"
git push
```

push 后可以看到 site 正在构建，如下图:

![image-20250421015317338](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250421015317338.png)

6. 进入 site 查看

![image-20250421015739167](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20250421015739167.png)