GitHub Page 和 Jekyll 搭建个人Blog
-------
你也许不想使用别人指定的方式创建网站博客,你也许好好的体验DIY式的创建方法,例如能设计自己的网页布局,主题,格式,又或者你只是想看看Github page 和 jekyll是做什么用的.不过,你需要懂一点git的用法和网页生成的方式.

##1) GitHub Page 生成方法:
###什么是 GitHub Pages(Git for Websites)
>Github Pages是面向用户、组织和项目开放的公共静态页面搭建托管服务，站点可以被免费托管在Github上,你可以选择使用Github Pages默认提供的域名github.io或者自定义域名来发布站点。

>Github Pages依靠Github上项目的某些特定分支(master/gh-pages)来工作。Github Pages分为两种基本类型：

*  用户/组织的站点(User or organization site)
*  项目的站点(Project site)。

###生成用户和组织的站点

>	用户和组织的站点被放置在一个特殊的专用仓库中，在该仓库中只存在 Github Pages 的相关文件。这个仓库必须根据用户/组织的名称来命名，仓库中master分支里的文件将会被用来生成 Github Pages站点,所以请确保你的文件储存在该分支上.


*	首先在GitHub上创建库

>   登陆[github](https://github.com/login)创建库名为*username*.github.io的专用仓库,**注意,username必须匹配你自己的Github帐号用户名**. 例如,我的Git用户名为:*feyman-li*,则仓库名需要命名为*feyman-li*.git.io

*  Clone刚创建的仓库到本地

>   Linux系统下,打开终端(termianal), 进入你想保存专用库的文件夹目录下,你也可以使用mkdir创建一个新目录.终端中使用下面命令clone刚在Github上创建的专用库:

>   git clone https://github.com/*username*/*username*.github.io


*  添加index.html
	进入你刚从GitHub上clone的仓库目录(*username*.github.io):
	
	cd *username*.github.io
	
	然后使用下面命令创建简单的index.html,用于在实现网页版的"Hello World!":
	
	echo "Hello World!" > index.html
	
*  Push本地改动到Github

    终端下使用下面命令将本地的改动提交到Github:
    
    *   git add --all
    *	git commit -m "Initial commit"
    *	git push -u origin master
	
	你可能需要根据终端提示输入你的用户名和密码.
	
*  打开浏览器,前往站点http://*username*.github.io即可看到你个人的"Hello World!"页面.

    这里的说明是基于Linux终端命令行格式.其他方法和系统更多的信息可以参考官方网站 https://pages.github.com 的说明.
	
###生成项目的站点

不同于用户和组织的站点，你需要创建一个名为"gh-pages"的"orphan"类型分支(不继承任何父分支的根分支),生成的站点会被部署到你的用户站点的子目录上，如 *username*.github.io/*projectname*（除非指定了一个自定义的域名）。

1.   Github 上创建一个任意命名的库
2.   Clone存在的库到本地电脑:
	 git clone github.com/*username*/*repository*.git
3.   创建名为gh-pages的"orphan"分支:
     cd repository
	 git checkout --orphan gh-pages
	 git rm -rf .
	 
4.   添加index.html文件
     echo "Hello,World!" > index.html
 
5.   Push本地改动到Github
     git add index.html
	 git commit -a -m "First pages commit"
	 git push origin gh-pages
6.   打开浏览器,前往站点 http(s)://_username_.github.io/*projectname*.

更多信息参考访问https://help.github.com/articles/creating-project-pages-manually

##Jekyll的使用
###Jekyll 究竟是什么？

Jekyll 是一个简单的博客形态的静态站点生产机器。它有一个模版目录，其中包含原始文本格式的文档，通过 Markdown （或者 Textile） 以及 Liquid 转化成一个完整的可发布的静态网站，你可以发布在任何你喜爱的服务器上。Jekyll也可以运行在 GitHub Page 上，也就是说，你可以使用 GitHub 的服务来搭建你的项目页面、博客或者网站，而且是完全免费的。

##事先准备

安装 Jekyll 相当简单，但是你得先做好一些准备工作 开始前你需要确保你在系统里已经有如下配置:	
	Ruby
	RubyGems
安装 Jekyll 的最好方式就是使用 RubyGems. 你只需要打开终端输入以下命令就可以安装了：
gem install jekyll (Ubuntun 下也可以使用:sudo apt-get install jekyll)的方式
###以下是一个获取最简单 Jekyll 模板并生成静态页面的方法:
	gem install jekyll
	jekyll new myblog
	cd myblog
	jekyll serve
然后前往http://localhost:4000就可以看到一个生成的本地网页.它有利于你通过修改快速查看生成的效果.

##Jekyll 部署在Git Page上:
项目的站点文件存放在项目本身仓库的 gh-pages分支中。该分支下的文件将会被 Jekyll 处理，你可以在生成Git项目站点后,在gh-pages分支下使用Jekyll生成网页,你可以通过修改_config.yml文件进行配置,如果想尽快在你的Git站点上生成类似于Jekyll本地生成的网页,你只需要将文件中"url"和"baseurl"配置为你自己的站点地址,类似于:

>    url: "http://*username*.github.io" (the base hostname & protocol for your site)

>    baseurl: "/*projectname*" (the subpath of your site, e.g. /blog) 
**注意'/'是必需的**


然后Push到GitHub上,访问http:*username*.github.io/*projectname*


较容易的方法是使用http://jekyllthemes.org下载主题后,修改配置文件的信息
然后以 git push -u https://github.com/*username*/*projectname*.git的方式 push到自己的工程.你可以通过git branch -a的方式查看远程分支是否创建成功.
