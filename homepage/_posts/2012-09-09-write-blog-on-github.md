---
layout: post
title: "write blog on github"
description: ""
category: blog
tags: []
---


### 2012年9月9日,在github创建blog日志  
* 下载模版上传
    1. git clone https://github.com/plusjade/jekyll-bootstrap.git matrix207.github.com
    2. cd matrix207.github.com/
    3. git remote set-url origin git@github.com:matrix207/matrix207.github.com.git
    4. git push origin master
    5. ssh-keygen -t rsa -C "dennis.cpp@gmail.com"
	   作用: 产生ssh-key,上传代码需要
    6. cat ~/.ssh/id_rsa.pub
    7. git push origin master

* 安装jekyll(Ubuntu)(本地预览)
    1. sudo apt-get install ruby  
    2. sudo apt-get install rubygems1.8 rake  
    3. sudo apt-get install ruby1.8-dev  
        如果你在gem的安装过程中遇到了问题，你可能需要安装用于在Ruby 1.8上编译扩展模块的头文件。  
        参考http://www.soimort.org/tech-blog/2011/11/19/introduction-to-jekyll_zh.html  
    4. sudo gem install jekyll  
    5. sudo vim /etc/profile  
         追加如下内容:  
        PATH=$PATH:/var/lib/gems/1.8/bin  
        export PATH  
    6. source /etc/profile  
    7. cd 项目目录  
         如: cd /home/dennis/project/matrix207.github.com  
    8. jekyll --server  

* Reference:  
    + [像极客一样写博客](http://zyzhang.github.com/blog/2012/08/29/%E5%83%8F%E6%9E%81%E5%AE%A2%E4%B8%80%E6%A0%B7%E5%86%99%E5%8D%9A%E5%AE%A2/)
    + [博客源码](https://github.com/zyzhang/zyzhang.github.com)
    + [用git维护博客,酷.](http://www.worldhello.net/2011/11/29/jekyll-based-blog-setup.html)
    + [在github上写博客](http://www.cnblogs.com/Lvkun/archive/2012/02/08/write-blog-on-github.html)
    + [为github帐号添加SSH keys](http://blog.csdn.net/keyboardota/article/details/7603630)
    + [ubuntu 搭建 Jekyll环境](http://blog.csdn.net/liumengxinfly/article/details/7419144)
    + [像黑客一样写博客——Jekyll入门](http://www.soimort.org/tech-blog/2011/11/19/introduction-to-jekyll_zh.html)
    + [Jekyll Quick Start ](http://jekyllbootstrap.com/usage/jekyll-quick-start.html)
    + [基于github的jekyll模板开发的网站/博客](https://github.com/mojombo/jekyll/wiki/sites)

* 基本操纵:  

	产生新的文章  
	rake post title="MyNewPage"  
	rake page name="MyNewPage"  
	rake preview  

	更新:
	git push origin master
	git add .
	git commit -m 'create my site'
	git push
	
	例子:
	[root@localhost matrix207.github.com]# rake post title="MyFisrtArticle"
	(in /home/dennis/project/matrix207.github.com.tmp)
	Creating new post: ./_posts/2012-09-14-myfisrtarticle.md
	[root@localhost matrix207.github.com]#  rake page name="MyFisrtArticle"
	(in /home/dennis/project/matrix207.github.com.tmp)
	mkdir -p ./MyFisrtArticle
	Creating new page: ./MyFisrtArticle/index.html
	
	// git 删除文件,目录
	git rm abc.md
	git rm -r myfolder
	
### 2012年9月14日 
* [Github+Jekyll构建个人博客](http://equation85.github.com/blog/blog-with-github-and-jekyll/)
* [使用github+jekyll写博客](http://hjkl.me/git/2012/05/29/git-jekyll-blogging.html)
* [为 Jekyll 博客添加 category 分类](http://www.pizn.me/2012/02/23/use-category-plugin-for-jekyll-blog.html)
* [使用jekyll写博客](http://www.brucebot.com/2012/03/blog_with_jekyll_and_markdown/)
* [使用 Git 管理源代码](http://www.ibm.com/developerworks/cn/linux/l-git/)

### 2012年9月16日  
* [Markdown 语法等介绍](http://zh.wikipedia.org/wiki/Markdown)  
* [Markdown 语法说明  ](http://wowubuntu.com/markdown/)  
Markdown中的转义字符为`\`

### 2012年9月24日
	配置git环境 用户名和邮件(配置文件目录~/.gitconfig)
	git config --global user.name "Your Name"
    git config --global user.email you@example.com
	如:
	  dennis@dennis-VirtualBox:~git config --global user.name "Dennis"
	  dennis@dennis-VirtualBox:~git config --global user.emal "dennis.cpp@gmail.com"
	确认配置是否成功:
	git config user.name
	git config user.email
	也可以直接编辑文件:
	$ cat ~/.gitconfig

### 2012年12月14日
	环境windows xp，git
	git status 查看状态
	git pull 更新
	在_post目录中加入新的文章后如:2012-12-14-visual-studio-project-clean.md 
	执行下面的命令更新上传
	git push origin master
	下面这三行是执行上一个命令要求输入用户名和密码
		Username for 'github.com':
		Password for 'github.com':
		Everything up-to-date
	git add .
	git commit -m "create my site"
	git push

### 2013年01月04日
	Fedora17安装Jekyll预览github博客文章
	1.安装工具
		yum install ruby-devel
		gem install jekyll
	2.在blog目录下运行jekyll服务
		jekyll --server
	3.预览blog
		打开http://localhost:4000
	4.rake产生post文章
		gem install rake
		rake post title="***"

### 2013年01月25日  
* [轻量级标记语言](http://www.worldhello.net/gotgithub/appendix/markups.html)

### 2013年01月26日
* [像黑客一样写博客](http://kyle.xlau.org/posts/blogging-like-a-hacker.html)
* [Blogging Like a Hacker](http://tom.preston-werner.com/2008/11/17/blogging-like-a-hacker.html)
* [Blog Site use Jekyll](https://github.com/mojombo/jekyll/wiki/Sites)
* [TechTinkering](http://techtinkering.com/)\([source](https://github.com/lawrencewoodman/techtinkering.com)\)

### 2014年05月23日
* [How I built my blog in one day](http://erjjones.github.io/blog/How-I-built-my-blog-in-one-day/)
* [Part two on how I built my blog](http://erjjones.github.io/blog/Part-two-how-I-built-my-blog/)
* [Github pages](https://pages.github.com/)
* [jekll quick start](http://jekyllbootstrap.com/usage/jekyll-quick-start.html)

### 2014年06月05日
* [一步步在GitHub上创建博客主页:评论功能,站点统计](http://pchou.info/web-build/2013/01/09/build-github-blog-page-06.html)
* [你好！github页面: 域名解析](http://beyondvincent.com/blog/2013/07/27/107-hello-page-of-github/)

### 2014年06月21日
* [How do you uninstall Ruby 1.8.7 and install Ruby 1.9.2?](http://askubuntu.com/questions/91693/how-do-you-uninstall-ruby-1-8-7-and-install-ruby-1-9-2)

* **Install jekyll on Ubuntu**  

        dennis@dennis:~/work/git/matrix207.github.com$ sudo gem install jekyll
        ERROR:  Error installing jekyll:
            celluloid requires Ruby version >= 1.9.2.
        dennis@dennis:~/work/git/matrix207.github.com$ ruby --version
        ruby 1.8.7 (2011-06-30 patchlevel 352) [x86_64-linux]
        dennis@dennis:~/work/git/matrix207.github.com$ sudo apt-get install ruby1.9.3
        Reading package lists... Done
        Building dependency tree       
        ......
        dennis@dennis:~/work/git/matrix207.github.com$ sudo update-alternatives --config ruby
        There are 2 choices for the alternative ruby (providing /usr/bin/ruby).

          Selection    Path                Priority   Status
        ------------------------------------------------------------
        * 0            /usr/bin/ruby1.8     50        auto mode
          1            /usr/bin/ruby1.8     50        manual mode
          2            /usr/bin/ruby1.9.1   10        manual mode

        Press enter to keep the current choice[*], or type selection number: 2
        update-alternatives: using /usr/bin/ruby1.9.1 to provide /usr/bin/ruby (ruby) in manual mode.
        dennis@dennis:~/work/git/matrix207.github.com$ ruby -v
        ruby 1.9.3p0 (2011-10-30 revision 33570) [x86_64-linux]
        dennis@dennis:~/work/git/matrix207.github.com$ sudo update-alternatives --config gem
        There are 2 choices for the alternative gem (providing /usr/bin/gem).

          Selection    Path               Priority   Status
        ------------------------------------------------------------
        * 0            /usr/bin/gem1.8     180       auto mode
          1            /usr/bin/gem1.8     180       manual mode
          2            /usr/bin/gem1.9.1   10        manual mode

        Press enter to keep the current choice[*], or type selection number: 2
        update-alternatives: using /usr/bin/gem1.9.1 to provide /usr/bin/gem (gem) in manual mode.
        dennis@dennis:~/work/git/matrix207.github.com$ gem -v
        1.8.11

* install nodejs for JavaScript runtime environment  
  `sudo apt-get install nodejs`

* `jekyll --server` change to `jekyll server`
