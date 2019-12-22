## 依赖环境

博客框架基于python， 使用pip安装相关安装包


```python
Python 3.0-3.7.x 	# mkdocs还未支持Python 3.8.0
mkdocs >= 1		 	# 博客框架
mkdocs-material	 	# mkdocs主题
pymdown-extensions	# markdown扩展
```

## 博客结构

```yaml
-.git:
	- # git配置文件夹，勿动
-docs:
	- _static	
	- Base
	- Data_Structure
	- ...				# docs文件夹存放markdown文件，既博客内容
-site:
	- xxxx.html
	- ...	 			# - mkdocs build命令后生成的静态文件，由html组成。
- CNAME 				# DNS域名配置文件，若丢失会造成域名无法解析
- mkdocs.yml 			# mkdocs配置文件，文章目录
- README.md  			# GitHub仓库介绍文件
```

## Git命令简介

需要安装git环境，[下载Git](https://git-scm.com/downloads)

有命令行和GUI两种操作方式，推荐两种结合使用 [下载GitHub Desktop](https://desktop.github.com/)

```bash
start a working area (see also: git help tutorial)
   clone     Clone a repository into a new directory
   init      Create an empty Git repository or reinitialize an existing one

work on the current change (see also: git help everyday)
   add       Add file contents to the index
   mv        Move or rename a file, a directory, or a symlink
   restore   Restore working tree files
   rm        Remove files from the working tree and from the index

examine the history and state (see also: git help revisions)
   bisect    Use binary search to find the commit that introduced a bug
   diff      Show changes between commits, commit and working tree, etc
   grep      Print lines matching a pattern
   log       Show commit logs
   show      Show various types of objects
   status    Show the working tree status

grow, mark and tweak your common history
   branch    List, create, or delete branches
   commit    Record changes to the repository
   merge     Join two or more development histories together
   rebase    Reapply commits on top of another base tip
   reset     Reset current HEAD to the specified state
   switch    Switch branches
   tag       Create, list, delete or verify a tag object signed with GPG

collaborate (see also: git help workflows)
   fetch     Download objects and refs from another repository
   pull      Fetch from and integrate with another repository or a local branch
   push      Update remote refs along with associated objects
```

常用的命令只有几条熟悉即可，我们一般在GUI中操作

1.  git clone url
2.  git add -A
3.  git commit -m “本次提交的说明”
4.  git push

## MkDocs命令

```powershell
MkDocs - Project documentation with Markdown.

Options:
  -V, --version  Show the version and exit.
  -q, --quiet    Silence warnings
  -v, --verbose  Enable verbose output
  -h, --help     Show this message and exit.

Commands:
  build      Build the MkDocs documentation
  gh-deploy  Deploy your documentation to GitHub Pages
  new        Create a new MkDocs project
  serve      Run the builtin development server
```

常用操作流程，在任一终端下

1.  进入wiki的根目录使用mkdocs serve 启动本地服务，此时可以在 127.0.0.1:8000 使用浏览器访问
2.  本地文件修改后，无需重启服务，保存后浏览器自动刷新
3.   修改完成后使用 mkdocs build 生成静态文件`/site`文件夹
4.   无需使用gh-deploy和new命令

## 参与编写

### > 我是萌新

参与Wiki的编写**需要**一个 GitHub 账号，**不需要**高超的 GitHub 技巧。 

 维修中。。。

### > 我是大佬

被迫成为大佬，提交你宝贵的第一次**PR（Pull Requests）**吧

PR五步走流程

1.  fork本项目到自己的仓库

    ![](.\images\forl.png)

    成功后：（注意这里已经是自己的仓库了，用户名relifes，并且图标变成了叉子）

    ![](.\images\fork-successed.png)

2.   clone到本地进行修改，由于是自己的仓库，所以想怎么改都行 

    ![](.\images\clone.png)
    
    这里有4种方式克隆下来
    
    -   git clone `https://github.com/username/wiki.git`
    -   git clone `git@github.com:username/wiki.git`
    -   使用Destop客户端克隆
    -   直接下载项目的源码压缩包




