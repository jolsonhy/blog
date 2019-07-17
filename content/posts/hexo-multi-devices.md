---
title: "多设备使用 Hexo 写博客的解决方案"
date: 2019-06-16T15:39:47+08:00
# draft: true
tags: [hexo, blog, 技术]
categories: [Tutorial]
---


最近弄了一台 XPS 13，但同时原来的 MacBook Pro也在使用，就涉及到两台设备需要同时使用 Hexo 编辑、上传 Blog 文件的问题，所以想到把源文件放到服务器上，以下解决方案的记录。

## 了解 Hexo 机制
首先，要搞清楚使用 Hexo 搭建博客的基本机制，本地 git clone 自 Hexo 的并非我们要部署的文件，而通过 `hexo d` 上传部署到 GitHub 的其实是它编译之后的文件，基本就是一个纯静态的 html + css + js 页面，不包括源文件，实际体现在我们本地就是 `.deploy_git` 这个文件夹里的。

了解其机制之后就好办了，一般情况通过 GitHub 作为中转站有两种方式，一种是在你 name.github.io 基础上直接新建一个 hexo 分支，并将它设置为默认分支，这样每次先将源文件推到 hexo 默认分支上，再编译部署实际内容到 master 分支，从而实现一个仓库同时管理源文件与博客文件。

不过我比较喜欢单独建一个私有仓库来存储这些内容，以下是我的步骤：

## 新建仓库存储源文件

1、先定义原博客文件夹目录为 blog，blog 下新建一个 .gitignore，将一些不需要上传保存的内容忽略掉：

``` txt
.DS_Store
Thumbs.db
db.json
*.log
node_modules/
public/
.deploy*/
```

2、Github 新建一个用于保存源文件的仓库，这里设置为 blog_backup（不需要添加 README 文件，直接空白的），然后将本地与远程仓库关联，并推上去。
``` bash
git init
git commit -m “first commit”
git remote add origin git@github.com:<github_name>/blog_backup.git
git push -u origin master
```

3、完成之后，可以检查下是否成功，是否一些主动忽略的文件夹没有上传上去，确认没有问题之后，再去其它机器操作。


## 服务器操作

解决了源文件上传同步问题，接下来就是 ssh 到服务器把文件 down 下载，然后安装好本地部署需要的环境，再编译上传即可，这里以 GCP 为例：

1、直接在 `VM 实例` 页面点击 SSH 连接，登陆到终端，然后搭建 git 环境

- 安装 Git

``` bash
sudo apt-get install git
```

- 设置 Git 全局邮箱和用户名

``` bash
git config --global user.name "yourgithubname"
git config --global user.email "yourgithubemail"
```

- 设置ssh 

``` bash
keyssh-keygen -t rsa -C "youremail"
```

- 将 id_rsa 公钥粘贴到 Github

``` bash
cat ~/.ssh/id_rsa.pub
```

- 安装 Node.js

``` bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
```

- 重启终端，然后通过 nvm 安装

``` bash
nvm install stable
```

- 全局安装 Hexo

``` bash
npm install -g hexo-cli
```

至此，本地环境已经安装完毕，接着下载源文件

``` bash
git clone git@github.com:<github_name>/blog_backup.git
cd blog_backup
npm install
npm install hexo-deployer-git --save
```

#### 注意，这里由于原有的 Git 源文件仓库里的 themes 还包含了主题的 git 仓库，所以实际主题的东西没有被下载下来，需要手动再 clone 一次。

``` bash
cd your-hexo-site
git clone https://github.com/iissnan/hexo-theme-next themes/next
```

至此，新机器就与旧的环境保持一致了，每次做了任何修改，先把源文件 Git 上传一下，然后再通过 `hexo g -d` 部署编译。

而如果在另一个电脑上操作，就先 `git pull` 一下，保证与线上内容一致。


## 以上内容参考资料
- https://www.zhihu.com/question/21193762
- https://hexo.io/docs/index.html
- https://theme-next.iissnan.com
