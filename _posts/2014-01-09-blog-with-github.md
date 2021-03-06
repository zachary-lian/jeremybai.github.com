---
layout: post
title: "使用Github搭建自己的博客"
description: ""
category:  Jekyll
tags: [工具使用]
---

##1 搭建环境
操作系统：Windows 7旗舰版64位

##2 基础知识和准备工作
![](http://git-scm.com/images/logo@2x.png)    

　　**Git**  是一个开源的分布式版本控制系统，用以有效、高速的处理从很小到非常大的项目版本管理。Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。使用Git可以帮助我们更有效的进行代码版本控制。  

![](http://jekyllcn.com/img/logo-2x.png)    
  
　　**Jekyll**是一个简单的博客形态的静态站点生产机器。它有一个模版目录，其中包含原始文本格式的文档，通过 [Markdown](http://daringfireball.net/projects/markdown/) （或者 [Textile](http://textile.sitemonks.com/)） 以及 [Liquid](http://docs.shopify.com/themes/liquid-basics) 转化成一个完整的可发布的静态网站，你可以发布在任何你喜爱的服务器上。它会根据网页源码生成静态文件。它提供了模板、变量、插件等功能，所以实际上可以用来编写整个网站。

　　Jekyll 的核心其实是一个文本转换引擎。它的概念其实就是： 你用你最喜欢的标记语言来写文章，可以是 Markdown，也可以是 Textile,或者就是简单的 HTML, 然后 Jekyll 就会帮你套入一个或一系列的布局中。在整个过程中你可以设置URL路径, 你的文本在布局中的显示样式等等。这些都可以通过纯文本编辑来实现，最终生成的静态页面就是你的成品了。

　　首先你在本地编写符合Jekyll规范的网站源码，然后上传到Github，因为Github标配有Jekyll解析引擎，上传的站点可以由Github的Jekyll解析,生成静态页面，从而在Github上生成并托管整个网站。有人会疑惑为什么我们要安装Ruby呢？原因是Jekyll是用ruby制造的静态站点的ruby解析引擎,不用担心，搭建博客时ruby不是必会的，只要安装上就可以了。

　　**确保你的电脑上已经安装了[Git](http://msysgit.github.io/ "msysGit"),[Ruby](http://rubyinstaller.org/downloads/ "Ruby Installer")和Jekyll，并且你已经拥有了一个github账户。**   

###注意：
　　在安装ruby的时候选择Ruby1.9.3这个版本，因为这个版本比较稳定并且提供给了比较全的包的支持，在安装过程中将是否添加系统路径打上勾，装完Ruby再把DEVELOPMENT KIT也装下，记得选择与Ruby1.9.3相匹配的开发包下载。
安装完之后打开cmd，输入命令：

    ruby -v
　　如果能显示版本号，就说明安装成功。

　　安装Jekyll：在git bash中运行命令：

    gem install jekyll
　　耐心等待一段时间就可以了。安装成功后输入命令：

    jekyll -v
　　和上面一样，显示版本号就表明安装成功了。这个时候，我们的准备工作已经完成，接下来我们会在三分钟制备搭建一个bootstrap风格的个人博客，如果安装中出现问题可以参考最后一节，给出了一些常见的问题和解决方法。  
##3 搭建博客
　　我们使用github创建页面时，有两种方式，一种是用户或者组织的页面，另外一种就是项目页面。
###3.1 创建新的代码库。 
　　在你自己的[github](https://github.com "Github")主页上创建一个新的代码库，取名为：USERNAME.github.com，其中USERNAME为你自己的用户名。
###3.2 安装Jekyll-Bootstrap
　　找一个合适的位置，使用Git bash输入以下命令：

    git clone https://github.com/plusjade/jekyll-bootstrap.git USERNAME.github.com
    cd USERNAME.github.com
    git remote set-url origin git@github.com:USERNAME/USERNAME.github.com.git
    git push origin master
###3.3 结果
　　等待几分钟之后，在浏览器中输入 http://USERNAME.github.com ，你就会看到Jekyll-Bootstrap的样例博客了,gorgeous!如果你希望在本地来调试你的博客的话，只要在在Git bash中运行：

    $ cd USERNAME.github.com 
    $ jekyll build
　　build完之后再运行`jekyll server`这时候服务器就在运行了。打开 [http://localhost:4000/](http://localhost:4000/) 上就可以看到效果了，这样你就可以在本地调试好之后push到github上。  
##4 修改  
###4.1 基本结构
　　接下来我们要做的就是在这个基础上把它修改成我们自己的博客了。我们介绍一个最基础的Jekyll博客的目录结构：     

		.
		├── _config.yml
		├── _drafts
		|   ├── begin-with-the-crazy-ideas.textile
		|   └── on-simplicity-in-technology.markdown
		├── _includes
		|   ├── footer.html
		|   └── header.html
		├── _layouts
		|   ├── default.html
		|   └── post.html
		├── _posts
		|   ├── 2007-10-29-why-every-programmer-should-play-nethack.textile
		|   └── 2009-04-26-barcamp-boston-4-roundup.textile
		├── _site
		└── index.html
　　这些目录的介绍如下：  

`_config.yml`：  
　　保存配置数据。很多配置选项都会直接从命令行中进行设置，但是如果你把那些配置写在这儿，你就不用非要去记住那些命令了。  
`_drafts`：  
　　drafts 是未发布的文章。这些文件的格式中都没有 title.MARKUP 数据。学习如何使用 drafts.  
`_includes`：  
　　你可以加载这些包含部分到你的布局或者文章中以方便重用。可以用这个标签  \{\% include file.ext \%\} 来把文件_includes/file.ext 包含进来。  
`_layouts`：  
　　layouts 是包裹在文章外部的模板。布局可以在 YAML 头信息中根据不同文章进行选择。 这将在下一个部分进行介绍。标签  {{ content }} 可以将content插入页面中。  
`_posts`：  
　　这里放的就是你的文章了。文件格式很重要，必须要符合: YEAR-MONTH-DAY-title.MARKUP。 The permalinks 可以在文章中自己定制，但是数据和标记语言都是根据文件名来确定的。  
`_site`：  
　　一旦 Jekyll 完成转换，就会将生成的页面放在这里（默认）。最好将这个目录放进你的 .gitignore 文件中。  
`index.html`：  
　　如果这些文件中包含 YAML 头信息 部分，Jekyll 就会自动将它们进行转换。当然，其他的如 .html， .markdown，  .md，或者 .textile 等在你的站点根目录下或者不是以上提到的目录中的文件也会被转换。  
`其他文件和文件夹`：  
　　其他一些未被提及的目录和文件如  css 还有 images 文件夹， favicon.ico 等文件都将被完全拷贝到生成的 site 中。  
###4.2 开始修改
　　首先我们编辑_config.yml文件，将页面的一些基本参数改掉：

    title : My Blog =)
    
    author :
      name : Name Lastname
      email : blah@email.test
      github : username
      twitter : username

　　比如页眉显示的文字，以及页脚显示的作者，邮件等等。在_posts文件夹下有个core-samples文件夹存放的简单样例，我们可以把它删掉：

    rm -rf _posts/core-samples
　　这时候我们运行目录jekyll build一下，发现主页并没有被改变还是显示的关于Jekyll-Bootstrap的，于是我们打开index.md，修改里面的内容，将与Bootstrap相关的东西全部删除，只留下posts list用来显示我们的博客列表。再打开README.md修改下readme，这样我们的博客框架就修改好了。  
　　接下来做的就是将代码push到github就可以了。

    git add .
    git commit -m "change the content"
    git push origin master
###4.3 创建文章
　　发表一篇新文章，你所需要做的就是在_posts文件夹中创建一个新的文件。 文件名的命名非常重要。Jekyll 要求一篇文章的文件名遵循下面的格式：

	年-月-日-标题.MARKUP
　　在这里，年是4位数字，月和日都是2位数字。MARKUP扩展名代表了这篇文章 是用什么格式写的。  
　　我们也可以通过命令来创建文章：

    rake post title="Hello World"
　　这个命令会在_posts文件夹下创建一个year-month-day-hello-world.md文件,　我们用notepad++打开这个文件，发现前几行的格式比较特殊，这就是[YAML 头信息](http://yaml.org/)，正是头信息开始让 Jekyll 变的很酷。任何只要包含 YAML 头信息的文件在 Jekyll 中都能被当做一个特殊的文件来处理。头信息必须在文件的开始部分，并且需要按照 YAML 的格式写在两行三虚线之间。**如果你使用UTF-8编码，那么在你的文件中一定不要出现 BOM 头字符，否则你会碰上非常糟糕的事情，尤其当你在Windows上使用Jekyll的时候。**下面是一个基本的例子：  

	---
	layout: post
	title: hello world
	description: ""
	category:
	tags: 
	---
　　在这两行的三虚线之间，你可以设置一些预定义的变量（下面这个例子可以作为参考）或者甚至创建一个你自己定义的变量。这样在接下来的文件和任意模板中或者在包含这些页面或博客的模板中都可以通过使用 Liquid 标签来访问这些变量。一个文章中多个类别可以通过 YAML list来指定，或者用空格隔开。类似分类，一篇文章也可以给它增加一个或者多个标签。同样多个标签之间可以通过 YAML 列表或者空格隔开。在头信息中你设置一个 title，然后就可以在你的模板中使用这个 title 变量来设置页面的 title属性 ：  

	<!DOCTYPE HTML>
	<html>
	  <head>
	    <title>{{ page.title }}</title>
	  </head>
	  <body>
	    ...  
　　也可以使用jekyll的草稿（Jekyll v1.x 新增功能）功能，,草稿是没有日期的文章。它们是你还在创作中而暂时不想发表的文章。草稿创建在名为 _drafts 的文件夹（如在4.1 基本结构章节里描述的）：

	rake post '文章标题' --drafts

　　为了预览你拥有草稿的网站，运行带有 --drafts 配置选项的 jekyll serve 或者 jekyll build。此两种方法皆会将 Time.now 的值赋予草稿文章，作为其发布日期，所以你将看到草稿文章作为最新文章被生成。  
  

###4.4 创建页面
　　除了可以创建文章，我们还可以创建一些其他的页面。

    rake page name="about.md"
    rake page name="pages/about.md"

##5 问题
　　在过程中，也遇到了一些问题，整理如下：
###5.1 关于windows下git bash的中文显示。
　　解决方法：  
　　1、修改C:\Program Files(x86)\Git\etc\git-completion.bash,添加  

    	alias ls='ls --show-control-chars --color=auto'  
　　说明：使得在 Git Bash 中输入 ls 命令，可以正常显示中文文件名。  
　　2、修改C:\Program Files(x86)\Git\etc\inputrc，添加

    set output-meta on
    set convert-meta off
　　说明：使得在 Git Bash 中可以正常输入中文，比如中文的 commit log。
　　3、修改C:\Program Files(x86)\Git\etc\profile，添加

    export LESSCHARSET=utf-8
　　说明：$ git log 命令不像其它 vcs 一样，n 条 log 从头滚到底，它会恰当地停在第一页，按 space 键再往后翻页。这是通过将 log 送给 less 处理实现的。以上即是设置 less 的字符编码，使得 $ git log 可以正常显示中文。其实，它的值不一定要设置为 utf-8，比如 latin1 也可以……。还有个办法是 $ git –no-pager log，在选项里禁止分页，则无需设置上面的选项。  
　　4、修改C:\Program Files\Git\etc\gitconfig，添加

    [gui]
    	encoding = utf-8
　　说明：我们的代码库是统一用的 utf-8，这样设置可以在 git gui 中正常显示代码中的中文。

    [i18n]
 	    commitencoding = GB2312
　　说明：如果没有这一条，虽然我们在本地用 $ git log 看自己的中文修订没问题，但是，一、我们的 log 推到服务器后会变成乱码；二、别人在 Linux 下推的中文 log 我们 pull 过来之后看起来也是乱码。这是因为，我们的 commit log 会被先存放在项目的 .git/COMMIT_EDITMSG 文件中；在中文 Windows 里，新建文件用的是 GB2312 的编码；但是 Git 不知道，当成默认的 utf-8 的送出去了，所以就乱码了。有了这条之后，Git 会先将其转换成 utf-8，再发出去，于是就没问题了。
###5.2 修改jekyll_bootstrap主页的yaml头时出现中文字符不能build成功
　　这是估计因为字符集不兼容的问题，解决方法：  
　　修改C:\Program Files (x86)\Git\etc路径下的git-completion.bash文件： 添加  

    export LC_ALL=zh_CN.UTF-8
    export LANG=zh_CN.UTF-8

### 5.3 jekyll build 出现 invalid argument 错误

　　出现这个问题可能是由于你的jekyll版本的问题造成的，1.4.3的版本刚刚出来可能不太稳定，改成1.4.2就行了。

    gem uninstall jekyll
    gem install jekyll --version "=1.4.2"

### 5.4 cannot close fd before spawn
　　`Jekyll build`时产生的警告"cannot close fd before spawn"而导致`jekyll build`失败（反馈消息：`"Conversion error: There was an error converting _posts/xxx.md"`），因为这个问题实在困扰了我好久。其实是pygments.rb版本导致。产生这个警告的大多数原因是pygments.rb版本不正确。首先查看pygments版本，简单的方法，可以先在ruby文件夹中搜索pygments.rb 会看到以版本名称命名的文件夹，如pygments.rb-0.5.4，然后再运行：
  
    gem uninstall pygments.rb --version "=0.5.4"
    gem install pygments.rb --version "=0.5.0"
　　然后再`jekyll build`，应该就会成功了。  

### 5.5 jekyll中的不能嵌套html代码  
　　之前不能在markdown文件中嵌入html代码，在build的时候总会出问题，貌似是转义字符的问题，现在可以了，解决方法是使用RDiscount模板解释器替代Maruku。这个解释器到底是干嘛用的呢？和其它轻量标记语言一样，Markdown并不能也不旨在替代HTML；因为所有的网页最终都要交给浏览器来解析的，而浏览器只认识HTML，因此，单独使用Markdown编写的文本并不能为浏览器所认识；所以，在浏览器和Markdown之间还要有一个东西，那就是翻译器。它将已完成的Markdown格式的文本转换成HTML文本，然后才能交由浏览器来解析和排版。Markdown官方发布的翻译器是一个Perl脚本，在命令行执行如下命令即可转换Markdown文本到HTML文本：

    perl Markdown.pl --html4tags sample.txt > sample.html  

　　Maruku是纯ruby的Markdown模版解释器；rdiscount 则是c写的模版解释器，两者的效率显然不同。rdiscount 的安装很简单：

    gem install rdiscount
　　再编辑_config.yml文件，加上：

    markdown: rdiscount

　　再运行jekyll build和jekyll server就不会有错误了。再补充一点，如果使用使 rdiscount，那么这种格式的代码就不能被高亮。

    ``` ruby
    require 'rubygems'
    
    def foo
    puts 'foo'
    end
    
    #comment
    ```
　　如果使用的是redcarpet解释器，上面的那种格式的代码高亮能够被识别，但是有个缺点，不能识别粗体。
### 5.6 设置用户名和邮箱
　　当你提交你的修改之后在你的github上并没有看到你的提交纪录时，有可能你没有设置你本地的用户名，使用下面的命令进行设置。  

	$ git config --global user.name author #将用户名设为author
	$ git config --global user.email author@corpmail.com #将用户邮箱设为author@corpmail.com
　　git-config命令带--global选项是对所有用户信息进行配置，默认只针对对当前用户。 git-config中写入配置信息。 如果git-config加了--global选项，配置信息就会写入到~/.gitconfig文件中。 因为你可能用不同的身份参与不同的项目，而多个项目都用git管理，所以建议不用global配置。  

	git config -–list              #查看用户信息
### 5.7 gem安装失败
　　如果你在使用gem准备安装jekyll时发现错误：  

	ERROR:  While executing gem ... (Gem::RemoteFetcher::FetchError)
	    Errno::ECONNRESET: An existing connection was forcibly closed by the remote
	host. - SSL_connect (https://api.rubygems.org/specs.4.8.gz)
　　这个错误是因为gem使用的默认的源中有些地址是被屏蔽掉了的，所以你无法从[https://rubygems.org/](https://rubygems.org/)获取相关的包，解决方法就是替换掉原有的源，替换方法如下： 

	$ gem sources --remove https://rubygems.org/
	$ gem sources -a https://ruby.taobao.org/
	$ gem sources -l
	*** CURRENT SOURCES ***
	
	https://ruby.taobao.org
	# 请确保只有 ruby.taobao.org
### 5.8 DevKit安装
　　windows下安装完ruby之后，使用gem安装jekyll时可能还会出现这样的错误： 
 
	Please update your PATH to include build tools or download the DevKit
	from 'http://rubyinstaller.org/downloads' and follow the instructions
	at 'http://github.com/oneclick/rubyinstaller/wiki/Development-Kit'
　　这是因为没有安装DevKit。DevKit是windows平台下编译和使用本地C/C++扩展包的工具，用来模拟Linux平台下的make, gcc等工具进行编译。下载与你ruby相匹配的[DevKit](http://rubyinstaller.org/downloads/)，指定解压路径，路径中不能有空格。如C:\DevKit，在这个路径下打开命令行：  

	ruby dk.rb init
	# 查找RubyInstaller安装的位置，并且生成config.yml，
	# 如果机器上有多个ruby版本，而且列出的Ruby与你希望的的不符，可以手动修改
	ruby dk.rb review 
	ruby dk.rb install
　　再次使用gem安装jekyll即可。
### 5.9 找不到mentos.py文件
　　在执行`jekyll build`时出现的错误：  

	Liquid Exception: No such file or directory - python c:/Ruby200-x64/lib/ruby/gems/2.0.0/gems/pygments.rb-0.4.2/lib/pygments/mentos.py in 2014-01-01-const-volatile.md
　　这个错误是由于pygments没有被正确安装或者python未被添加到系统路径。使用目录`gem list`查看pygments是否被安装成功，因为pygments依赖于python，所以还需要安装python，前往[官网](https://www.python.org/downloads/)下载python进行安装，安装时选择将python添加到系统路径中，如果没有添加的话在安装完成之后自己手动添加，添加完成之后重启电脑，在命令行中输入`python --version`检查路径添加是否成功。成功之后再次执行`jekyll build`错误就没有了。  
### 5.10 找不到mentos.py文件  
　　使用bundle时出现错误信息：

	[!] There was an error parsing `Gemfile`: SSL_connect returned=1 errno=0 state=S SLv3 read server certificate B: certificate verify failed. Bundler cannot continue.
　　这个问题是由于验证失败导致的，可以参考[https://gist.github.com/fnichol/867550](https://gist.github.com/fnichol/867550)这篇文章，我使用的是第二种方法，下载[cacert.pem](http://curl.haxx.se/ca/cacert.pem)文件并且保存，保存的位置由你决定，我保存在了ruby的安装位置，接着在命令行输入让ruby意识到这个证书文件的存在:

	set SSL_CERT_FILE=C:\RailsInstaller\cacert.pem
　　为了不用每一次都输入这个命令，添加环境变量SSL_CERT_FILE，值为C:\RailsInstaller\cacert.pem。
### 5.10 Liquid Exception: highlight tag was never closed...
　　[修复这个错误](http://blog.slaks.net/2013-08-09/jekyll-tag-was-never-closed)，你需要在你的`_config.yml`文件中增加下面一行:  

	excerpt_separator: ""   
	