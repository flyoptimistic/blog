---
title: GitPages和Hexo搭建个人博客
date: 2020-06-17 11:28:50
tags: 
	- Hexo
	- GitPages
	- 个人博客
categories:
	- 博客
comments: false


---

## 为什么要写个人博客？

- **个人空间**：科技发展迅猛的情况下，网民日益增加，在这个流量为王的时代，通过个人博客来提高自己的知名度，打造个人品牌能为自己带来更多的益处。
- **学无止境**：学如逆水行舟，不进则退。在行业竞争力日益激烈的情况下，通过运营个人博客来让自己的知识不断增长，避免成为时代落伍，淘汰的那部分人。通过记录博客让自己对知识的理解更加深刻,同时可以当做备忘录的存在，好记性不如写博客。

<!-- more -->

- **增强文采**：作为IT行业的猿类已经日益被社会绝大多数人冠上”人傻“，”直男“的称号，通过文字记录博客，能加强自己的文采，同时一定会有各行各业大佬提出不同的质疑，正是这个不断沟通交流思考的过程增强自己的沟通能力，有可能让自己成为所谓的”暖(zha)男“.
- **工作机会**：求职前一定会准备一份由自己精心打造的简历，简历决定了第一印象，简历中加入自己的博客无疑会为自己加分不少。



## 个人博客到底是什么？

- **域名**：地址，因为ip地址是一长串数字且不直观所以为了方便记忆发明一套字符型地址方案。描述网站具体位置。

- **服务器**：存储网站资源的容器。

- **博客系统**:让用户在互联网上建立个人博客的系统，包含了构建，发布，管理等一系列操作。

  博客本质是一个web网站，大致流程为：客户端通过浏览器中输入地址发起请求，根据地址去请求资源，并在获取到资源后展示在当前浏览器。

  1. **DNS域名解析**：客户端去解析域名，确认对应服务器。
  2. **建立连接**：客户端与服务器连接。[三次握手](https://baike.baidu.com/item/%E4%B8%89%E6%AC%A1%E6%8F%A1%E6%89%8B/5111559?fr=aladdin)
  3. **发送请求**：客户端发送请求报文。
  4. **服务器处理请求**：服务器处理请求，查询数据，获取到相应资源。
  5. **返回响应结果**：服务器返回响应结果给客户端。
  6. **关闭连接**
  7. **浏览器解析HTML**：浏览器解析html，css，js。加载静态资源（图片，视频等）。
  8. **浏览器布局渲染**：展示给用户。

​     具体流程请自行查看 [细说浏览器输入URL后发生了什么](https://zhuanlan.zhihu.com/p/31311964)，此处不过多赘述。

## 怎么搭建个人博客？

### 搭建环境

#### **Git**：[下载地址](https://git-scm.com/download/win)

#### **NodeJs**：[下载地址](http://nodejs.cn/download/)

~~~shell
#检查是否安装成功
$ node —version
~~~

#### **Hexo**：[中文文档](https://hexo.io/zh-cn/docs/)

~~~shell
#安装
$ npm install -g hexo-cli
# 查看是否安装成功，查看版本号
$ hexo -v
# 安装 Hexo 完成后，请执行下列命令，Hexo 将会在指定文件夹中新建所需要的文件。
$ hexo init <folder>
$ cd <folder>
$ npm install
# 清除缓存，和已生成的静态文件
$ hexo clean
#生成静态文件
$ hexo generate
# 启动,默认访问：http://localhost:4000/
$ hexo server
~~~

#### GithubPages：Github提供的免费站点服务，自带域名和1G免费空间

##### **新建博客仓库**：github新建一个名为`你的用户名.github.io`的repository，这样做的好处是避免生成的域名太长,具体可执行：![image-20200610180036253](新建GitPages仓库.png)

##### **部署到GitPages**

~~~properties
#修改hexo子目录中config.yml
deploy:
   type: git
   repository: https://github.com/flyoptimistic/.github.io.git
   branch: master   
#安装插件
$ npm install hexo-deployer-git —save
#执行以下命令发布本地博客到远程仓库
$ hexo deploy
~~~

## 怎么优化Hexo博客页面？

此处都以next主题为例，如需别的主题可以点击[这里](https://hexo.io/themes/)

### 了解文件目录

~~~
.
├── _config.yml   	#配置文件
├── package.json	#应用程序的信息
├── scaffolds		#模版 文件夹
├── source  		#资源文件夹是存放用户资源的地方
|   ├── _drafts
|   └── _posts		#生成的文章
└── themes			#主题 文件夹

~~~

### 安装Next主题：[官网](https://theme-next.org/)

~~~shell
#切换到blog文件夹下（就是hexo init的文件夹）
cd [blog]
#通过git安装 注意：此处next版本为8.0,如需别的版本请自行去github上查找。
git clone https://github.com/next-theme/hexo-theme-next.git themes/next
#切换到next主题：hexo根目录下编辑_config.yml文件
theme: next
#切换后，用命令清除下缓存
hexo clean
hexo generate
#执行hexo s本地产看NexT主题效果
hexo server
~~~

### 主题设定

next目录下**_config.yml**文件 ：选择 Scheme

Scheme 是 NexT 提供的一种特性，借助于 Scheme，NexT 为你提供多种不同的外观。

- Muse - 默认 Scheme，这是 NexT 最初的版本，黑白主调，大量留白
- Mist - Muse 的紧凑版本，整洁有序的单栏外观
- Pisces - 双栏 Scheme，小家碧玉似的清新

Scheme 的切换通过更改 **主题配置文件**，搜索 scheme 关键字。 你会看到有三行 scheme 的配置，将你需用启用的 scheme 前面注释 `#` 去除即可。

### 站点配置文件：文件位置：~/_config.yml [官网文档](https://hexo.io/docs/configuration.html)

~~~yaml
# 网站标题、副标题、网站描述、关键词、作者、语言等基本信息的配置
title: 北木南的博客
subtitle: ''
description: '心有猛虎，细嗅蔷薇'
keywords:
author: 北木南
language: zh-CN  
timezone: Asia/Shanghai
# 博客的网址及文章 URL 结构，默认在根目录
# 如果你想要将博客设定在一个子目录，如 'http://yoursite.com/blog'，则将 root 设定为该子目录的名称 '/child/'
# 建议博客的 URL 结构在博客建立初期就规划好，因为当你写的文章被搜索引擎收录以及被读者收藏后，再更改结构，会对你的网站访问造成一定影响
url: https://github.com/flyoptimistic/flyoptimistic.github.io.git
root: /blog
permalink: :year/:month/:day/:title/
permalink_defaults:
pretty_urls:
  trailing_index: true # Set to false to remove trailing 'index.html' from permalinks
  trailing_html: true # Set to false to remove trailing '.html' from permalinks

# 这里是设定一些基本文件夹的名称，如资源文件夹等。
source_dir: source
public_dir: public
tag_dir: tags
archive_dir: archives
category_dir: categories
code_dir: downloads/code
i18n_dir: :lang
## skip_render 是为了避免在执行 'hexo generate' 命令后将一些你不想转义的文件转成 HTML 格式。
## 比如 README.md，你可以将这些文件名填写在括号内，格式为 [README.md, Post1.md, Post2.md]
skip_render: [README.md]

# Writing
new_post_name: :title.md # File name of new posts
default_layout: post
titlecase: false # Transform title into titlecase
external_link:
  enable: true # Open external links in new tab
  field: site # Apply to the whole site
  exclude: ''
filename_case: 0
render_drafts: false
## post_asset_folder 设置为 true 后，当你新建一个 post 的时候，会在同级目录生成一个相同名字的文件夹
post_asset_folder: false
relative_link: false
future: true
## 代码高亮设置
highlight:
  enable: true
  line_number: true
  auto_detect: false
  tab_replace: ''
  wrap: true
  hljs: false

# Home page setting
# path: Root path for your blogs index page. (default = '')
# per_page: Posts displayed per page. (0 = disable pagination)
# order_by: Posts order. (Order by date descending by default)
index_generator:
  path: ''
  per_page: 10
  order_by: -date

# Category & Tag
default_category: uncategorized
## URL 中的分类和标签「翻译」成英文 参见：https://github.com/hexojs/hexo/issues/1162
category_map:
tag_map:

# Metadata elements
## https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta
meta_generator: true

# Date / Time format
## Hexo uses Moment.js to parse and display date
## You can customize the date format as defined in
## http://momentjs.com/docs/#/displaying/format/
date_format: YYYY-MM-DD
time_format: HH:mm:ss
## Use post's date for updated date unless set in front-matter
use_date_for_updated: false

# Pagination
## Set per_page to 0 to disable pagination
per_page: 10
pagination_dir: page

# Include / Exclude file(s)
## include:/exclude: options only apply to the 'source/' folder
include:
exclude:
ignore:

# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
## 切换主题
theme: next

# Deployment
## Docs: https://hexo.io/docs/deployment.html
## 设定执行 'hexo deploy' 命令后提交的代码仓库地址
deploy:
   type: git
   repository: https://github.com/flyoptimistic/flyoptimistic.github.io.git
   branch: master
~~~

### 主题配置文件

#### 页脚信息

在网页的底部显示版权信息，包括年份、图标、作者信息，是否显示 Hexo 及其版本、NexT 及其版本，还有备案信息。图标名称需要填写 [Font Awesome](https://fontawesome.com/) 中提供的图标名称。如果你想要添加任何自定义内容，比如添加一句话，可以考虑在 `~/source/_data/` 路径下建立 `footer.swig` 文件，在该文件下添加内容。

~~~yaml
footer:
    icon:
    # 设置图标，如需要别的请自行查找 https://fontawesome.com/icons
    name: fa fa-heart
    # 设置动画效果
    animated: true
    # 颜色
    color: "#ff0000"
    # If not defined, `author` from Hexo `_config.yml` will be used.
    copyright:
    # 修改为false，去除hexo和next页脚链接
    powered: false
~~~

#### 导航目录

##### 基本配置

你可以在此处设置目录选项的名称和所在文件夹的位置，以及对应的图标，这里的图标同样需要对应 [Font Awesome](https://fontawesome.com/)  中图标的名称。

~~~yaml
menu:
  home: / || fa fa-home
  about: /about/ || fa fa-user
  tags: /tags/ || fa fa-tags
  categories: /categories/ || fa fa-th
  archives: /archives/ || fa fa-archive
  #schedule: /schedule/ || fa fa-calendar
  #sitemap: /sitemap.xml || fa fa-sitemap
  #commonweal: /404/ || fa fa-heartbeat

menu_settings:
  icons: true
  badges: false
~~~

- `home: ` 该目录名称，对应`~/themes/next/languages/zh-CN.yml` 下对应的 `menu:` 下添加对应的中文，格式为 `home: 首页`。
- `/` 即为网站的根目录。
- `||` 后的 `home` 即为图标名称。

##### 创建目录页面

- 建立标签页面和分类页面，首先需要去掉 `tags` 和 `categories` 前的注释，然后在 `~/source/` 文件夹下建立该页面，对应名称为 `tags` 和 `categories`。可以执行以下命令生成：

  ~~~shell
  $ hexo new page tags
  $ hexo new page categories
  ~~~

- 执行上述命令后会在 `~/source/` 文件夹中生成了对应名称的文件夹，在该文件夹下有一个 `index.md` 文件，打开该文件，在如下所示位置添加内容：

  ~~~yaml
  title: about
  date: 2020-06-12 15:36:24
  #添加type
  type: "about"
  ~~~

#### 设置侧栏阅读进度百分比

~~~yaml
back2top:
  enable: true
  #侧边栏回到顶部
  sidebar: false
  #百分比
  scrollpercent: true
~~~

#### 设置头像

~~~yaml
avatar:
  # 替换掉默认路径并设置头像路径
  url: /images/avatar.jpg 
  # 圆形
  rounded: true
  # 旋转
  rotated: false

~~~

#### 设置网页标签图标

[制作icon链接](http://www.ico51.cn/)

~~~yaml
favicon:
  small: /images/favicon_16.ico  #此处放制作好的链接
  medium: /images/favicon_32.ico
  #apple_touch_icon: /images/apple-touch-icon-next.png
  #safari_pinned_tab: /images/logo.svg
  #android_manifest: /images/manifest.json
  #ms_browserconfig: /images/browserconfig.xml

~~~



#### 设置github图标

~~~yaml
github_banner: # 开启github
  enable: true
  permalink: https://github.com/flyoptimistic #跳转自己的仓库链接
  title: Follow me on GitHub

~~~

#### 图片浏览

实现该功能的基础是在文章中插入图片。该项功能的效果是：点击文中插图，图片能够放大，有幻灯片的效果。目前 NexT 提供了两款插件 fancybox 和 mediumzoom，两款插件开启一个即可。

~~~yml
fancybox: false
mediumzoom: true
~~~

#### 数学公式

支持 MathJax 和 KaTeX 两种加载数学公式的方法，使用语法都是 LaTeX 语法。不过 MathJax 的功能比较全面，KaTeX 的加载速度比较快。不过有一点要注意，不论是用哪一个方式，我都推荐替换默认渲染器。

MathJax 使用 hexo-renderer-pandoc 或者 hexo-renderer-kramed；KaTeX 使用 hexo-renderer-markdown-it-plus 或者 hexo-renderer-markdown-it。

默认的 `per_page: true` 的意思是，只用当你在文章设定中添加 `mathjax: ture`，才会在当前页面中加载公式渲染。如果你使用的是 KaTeX，还要注意，过长的公式会超出页面边框，可能需要自行添加 CSS 样式对长公式进行滚动浏览。

~~~yaml
math:
  # 只渲染前面有 `mathjax: true` 的页面.
  per_page: true

  # hexo-renderer-pandoc (or hexo-renderer-kramed) required for full MathJax support.
  mathjax:
    enable: true
    # See: https://mhchem.github.io/MathJax-mhchem/
    mhchem: false
  katex:
    enable: false
    # See: https://github.com/KaTeX/KaTeX/tree/master/contrib/copy-tex
    copy_tex: false
~~~



#### 访客统计

[卜算子统计官网](http://ibruce.info/2015/04/04/busuanzi)

~~~yml
busuanzi_count:
  #开启
  enable: true
  total_visitors: true
  total_visitors_icon: fa fa-user
  total_views: true
  total_views_icon: fa fa-eye
  post_views: true
  post_views_icon: fa fa-eye

~~~

#### 评论功能

##### 注册

[Valine](https://valine.js.org/) 评论系统是我认为的在国内网络环境下最好用的评论系统，可通过 Leancloud 管理评论，无广告，简洁美观。不过缺点就是，Leancloud 平台的不稳定性，在 2019 年夏季的时候，出现了一次域名停止解析的事故，原因是有人利用 Leancloud 进行一些非法行为，而平台管理人员并没有监管到位。在那次事故之后，Leancloud 加强了监管，国内用户必须进行实名注册，每一个服务器必须绑定一个备案的域名。如果你不想备案，可以选择使用 Leancloud 国际版。但谁也无法确保 Leancloud 国际版会发生什么事情。

Leancloud 国内版和国际版的配置相同，这里以国际版为例进行说明。首先进入[官网](https://leancloud.app/)进行用户注册，注册完成后点击「创建应用」，填写应用的名称，选择「开发版」进行创建。

###### 创建应用

![创建leancloud应用](创建leancloud应用.png)

###### 创建class

![leancloud_创建class](leancloud_创建class.png)

###### 获取appid和appkey

![leancloud_获取信息](leancloud_获取信息.png)

##### 修改主题配置文件

~~~yaml
# Valine
# 更多信息: https://valine.js.org, https://github.com/xCss/Valine
valine:
  enable: true
  appId:  # leancloud 应用 appid
  appKey: # leancloud 应用 appkey
  placeholder: 来都来了，留下点宝贵建议吧！ # Comment box placeholder
  avatar: mm # Gravatar style
  meta: [nick, mail, link] # Custom comment header
  pageSize: 10 # Pagination size
  language: # Language, available values: en, zh-cn
  visitor: false # Article reading statistic
  comment_count: true # If false, comment count will only be displayed in post page, not in home page
  recordIP: false # Whether to record the commenter IP
  serverURLs: # When the custom domain name is enabled, fill it in here (it will be detected automatically by default, no need to fill in)
  #post_meta_order: 0
~~~

#### 本地搜索

##### 安装插件

```shell
npm install hexo-generator-searchdb --save
```

##### 站点配置文件`_config.yml`添加

~~~yaml
search:
    path: search.xml
    field: post
    format: html
    limit: 5000
~~~

##### 主题配置文件

~~~yaml
local_search:
    enable: true
~~~



#### 添加分享

##### 打开[addthis官网](https://www.addthis.com/dashboard#tool-config/pub/ra-5eeb31b88c2ca90e)

##### 注册

##### 选择分享工具

{% asset_img 分享选择按钮.png 分享选择按钮 %}

##### 生成代码

##### 获取pubid

{% asset_img 生成分享id.png 生成分享id %}

##### 修改主题配置文件 `_config.yml`_

~~~
add_this_id:  #设置为你的id
~~~

### 网页样式布局

在对 NexT 主题的个性优化中，如果想要添加一些个性化的内容，就需要对内部代码进行修改。主题提供了许多注入点，可以通过注入点插入自己想要的东西，而不会对原有的主题内部文件进行大量的修改。这样便于以后主题的升级，避免一系列的错误发生。NexT 主题更新到 v7.2.0 后，[PR #868](https://github.com/theme-next/hexo-theme-next/pull/868) 简化了自定义内容的添加方法，删除了以前版本中所用的 `css/_custom.styl` 自定义 CSS 样式文件。如果想要修改样式或者在 HTML 中的 ``、`` 等部位插入代码。即直接在博客 `sourse` 资源文件夹下新建自定义文件 `_data/xxx` 实现该功能。

主题配置文件 `_config.yml` 中：

~~~yaml
# 自定义文件路径
# 在`source/_data` 创建自定义文件，并打开需要的注释.
custom_file_path:
  #head: source/_data/head.njk
  #header: source/_data/header.njk
  #sidebar: source/_data/sidebar.njk
  #postMeta: source/_data/post-meta.njk
  #postBodyEnd: source/_data/post-body-end.njk
  #footer: source/_data/footer.njk
  #bodyEnd: source/_data/body-end.njk
  #variable: source/_data/variables.styl
  #mixin: source/_data/mixins.styl
  #style: source/_data/styles.styl

~~~

#### 基本修改方法

NexT 主题最大的特点就是主题基础颜色简单，白加黑的简单组合为我们提供了能够随心所欲进行自定义的空间。修改颜色、修改图形、修改动画……这些都能够实现。控制这些自定义样式布局的文件即为 `styles.styl`。该文件在主题安装后并不存在，需要你自己建立。首先，你需要在主题配置文件 `_config.yml` 中，将下面一栏的注释删除：

~~~yaml
custom_file_path:
    style: source/_data/styles.styl

~~~

然后在你的在博客根目录下的 `source` 文件夹下新建 `_data` 文件夹，在该文件夹下创建名为 `styles.styl` 的文件。这里需要注意，不要将 `source` 文件夹创建在主题文件夹中。

该文件有什么作用呢？你可以将自定义的 CSS 样式写入该文件中，这些自定义样式会覆盖主题原有的样式设定。那么该如何自定义样式呢？你只需要一个工具——浏览器。这里我推荐使用 Google 的 Chrome 浏览器，通过浏览器打开你的网站，右键，点击「检查」或者按键盘上的「F12」键，进入[调试模式](https://developers.google.com/web/tools/chrome-devtools/)。然后点击窗口右上角的小箭头定位元素，定位到你需要修改的元素区域，调试台就会显示这一元素的 CSS 代码，在这里修改你想要的样式，再将其 Copy 到 `styles.styl` 中就可以了。

#### 添加顶部加载

在`~\themes\next\layout\_partials\head\head.njk`文件中添加如下代码

~~~js
<script src="//cdn.bootcss.com/pace/1.0.2/pace.min.js"></script>
<link href="//cdn.bootcss.com/pace/1.0.2/themes/pink/pace-theme-flash.css" rel="stylesheet">
<style>
  .pace .pace-progress {
	  background: #1E92FB; /*进度条颜色*/
	  height: 3px;
  }
  .pace .pace-progress-inner {
	   box-shadow: 0 0 10px #1E92FB, 0 0 5px     #1E92FB; /*阴影颜色*/
  }
  .pace .pace-activity {
	  border-top-color: #1E92FB;    /*上边框颜色*/
	  border-left-color: #1E92FB;    /*左边框颜色*/
  }
</style>
~~~

#### 修改标签样式

修改`~\themes\next\_config.yml`

~~~yaml
# 使用图标代替#
tag_icon: true
~~~

#### 添加球形标签云样式

![标签云](标签云.gif)

首先要确保你已经开启标签功能。目前有一个标签云插件可以提供这样的效果：[hexo-tag-cloud](https://github.com/MikeCoder/hexo-tag-cloud/blob/master/README.ZH.md)。执行 `npm install hexo-tag-cloud --save` 进行安装。插件安装完成后，你可以自定义标签云的位置，比如显示在侧栏，或者显示在标签页面。比如选择显示在标签页面，则在 `~/themes/next/layout/page.njk` 中，添加如下所示代码：

~~~js
{% if site.tags.length > 1 %}
<script type="text/javascript" charset="utf-8" src="{{ url_for('/js/tagcloud.js') }}"></script>
<script type="text/javascript" charset="utf-8" src="{{ url_for('/js/tagcanvas.js') }}"></script>
<div class="widget-wrap">
    <h3 class="widget-title">Tag Cloud</h3>
    <div id="myCanvasContainer" class="widget tagcloud">
        <canvas width="250" height="250" id="resCanvas" style="width:100%">
            {{ list_tags() }}
        </canvas>
    </div>
</div>
{% endif %}
~~~

最后，你可以选择在博客根目录配置文件 `_config.yml` 中添加如下的配置项进行更细致的设定:

~~~yaml
#球形标签云
tag_cloud:
    textFont: Trebuchet MS, Helvetica
    textColor: '#333'
    textHeight: 25
    outlineColor: '#E2E1D1'
    maxSpeed: 0.5
    pauseOnSelected: true # true 意味着当选中对应 tag 时,停止转动

~~~

#### 添加线状动态背景

![线性动态背景](线性动态背景.gif)

- 修改`next/layout/_layout.njk`_,添加到末尾

  ~~~js
  {% if theme.canvas_nest %}
  <script type="text/javascript" src="//cdn.bootcss.com/canvas-nest.js/1.0.0/canvas-nest.min.js"></script>
  {% endif %}
  
  ~~~

- `/next/_config.yml`添加

  ~~~yml
  # --------------------------------------------------------------
  # background settings
  # --------------------------------------------------------------
  # add canvas-nest effect
  # see detail from https://github.com/hustcc/canvas-nest.js
  canvas_nest: true
  
  ~~~

#### 添加点击爱心特效

首先在 `~/themes/next/source/js/` 下新建文件 `clicklove.js`，接着把以下的代码拷贝粘贴到该文件中：

~~~js
/* 所在目录：~/themes/next/source/js/ */
!function(e,t,a){function n(){c(".heart{width: 10px;height: 10px;position: fixed;background: #f00;transform: rotate(45deg);-webkit-transform: rotate(45deg);-moz-transform: rotate(45deg);}.heart:after,.heart:before{content: '';width: inherit;height: inherit;background: inherit;border-radius: 50%;-webkit-border-radius: 50%;-moz-border-radius: 50%;position: fixed;}.heart:after{top: -5px;}.heart:before{left: -5px;}"),o(),r()}function r(){for(var e=0;e<d.length;e++)d[e].alpha<=0?(t.body.removeChild(d[e].el),d.splice(e,1)):(d[e].y--,d[e].scale+=.004,d[e].alpha-=.013,d[e].el.style.cssText="left:"+d[e].x+"px;top:"+d[e].y+"px;opacity:"+d[e].alpha+";transform:scale("+d[e].scale+","+d[e].scale+") rotate(45deg);background:"+d[e].color+";z-index:99999");requestAnimationFrame(r)}function o(){var t="function"==typeof e.onclick&&e.onclick;e.onclick=function(e){t&&t(),i(e)}}function i(e){var a=t.createElement("div");a.className="heart",d.push({el:a,x:e.clientX-5,y:e.clientY-5,scale:1,alpha:1,color:s()}),t.body.appendChild(a)}function c(e){var a=t.createElement("style");a.type="text/css";try{a.appendChild(t.createTextNode(e))}catch(t){a.styleSheet.cssText=e}t.getElementsByTagName("head")[0].appendChild(a)}function s(){return"rgb("+~~(255*Math.random())+","+~~(255*Math.random())+","+~~(255*Math.random())+")"}var d=[];e.requestAnimationFrame=function(){return e.requestAnimationFrame||e.webkitRequestAnimationFrame||e.mozRequestAnimationFrame||e.oRequestAnimationFrame||e.msRequestAnimationFrame||function(e){setTimeout(e,1e3/60)}}(),n()}(window,document);
~~~

然后在 `~/theme/next/layout/_layout.njk` 文件的末尾添加引用：

~~~js
<!-- 文件位置：~/theme/next/layout/_layout.njk -->
<script type="text/javascript" src="/js/clicklove.js"></script>
~~~

#### 添加图片

##### [阿里云储存对象 OSS 服务](https://www.aliyun.com/product/oss/)：可以放在阿里云上

##### [hexo-asset-image](https://github.com/xcodebuild/hexo-asset-image)插件

###### 安装插件

~~~shell
npm install hexo-asset-image --save
~~~

###### 修改博客配置文件 `_config.yml`：

~~~yaml
#post_asset_folder 设置为 true 后，当你新建一个 post 的时候，会在同级目录生成一个相同名字的文件夹
post_asset_folder: true
~~~

###### 解决插件自身bug

~~~js
# 文件位置：~/node_modules/hexo-asset-image/index.js
else {
    #var endPos = link.length-1
	var endPos = link.lastIndexOf('.');
    }
~~~

###### 引入图片

~~~
![Alt text](/path/to/img.jpg)
![Alt text](/path/to/img.jpg "Optional title")
~~~

对于 Markdown 图片引用的代码，主要有三个部分：

1. `Alt text`，替代文本，图片无法显示时读者看到的就是它
2. `/path/to/img.jpg`，URL，即图片的链接
3. `Optional title`，图片的标题

##### hexo 自带（新版推荐使用）

###### 修改博客配置文件 `_config.yml`：

~~~yaml
#post_asset_folder 设置为 true 后，当你新建一个 post 的时候，会在同级目录生成一个相同名字的文件夹
post_asset_folder: true
~~~

###### markdown语法插入图片

~~~yaml
#图片相对路径
![Alt text](img.jpg)
~~~

###### 用于相对路径引用的标签插件

Hexo 3之后可以使用[标签插件](https://hexo.io/zh-cn/docs/tag-plugins.html)来引入图片

~~~yaml
# example.jpg 路径
{% asset_img example.jpg 这是一个实例图片 %}
{% asset_img "spaced asset.jpg" "spaced title" %}
~~~

#### 添加动态模型插件

[中文文档](https://github.com/EYHN/hexo-helper-live2d/blob/master/README.zh-CN.md)

##### 安装依赖

~~~shell
#安装插件
npm install --save hexo-helper-live2d
#安装动态模型，此处可以选择自己喜欢的模型 https://huaji8.top/post/live2d-plugin-2.0/
npm install --save live2d-widget-model-z16
~~~

##### 在 `~/themes/next/layout/layout.njk`添加

~~~
#末尾添加就行
{{ live2d() }}
~~~

##### 配置`~/_config.yml`

~~~yaml
# 添加动态模型插件
live2d:
  enable: true
  scriptFrom: local
  pluginRootPath: live2dw/
  pluginJsPath: lib/
  pluginModelPath: assets/
  tagMode: false
  debug: false
  model:
    use: live2d-widget-model-z16 ## 对应安装的宠物依赖包
  display:
    position: right  ##位置
    width: 150
    height: 300
  mobile:
    show: true
  react:
    opacity: 0.7
~~~

#### 添加音乐

##### 直接使用 HTML 的标签，格式如下，其中 `music-url` 替换为你需要加载的音乐即可：

~~~html
<audio src="music-url" style="max-height :100%; max-width: 100%; display: block; margin-left: auto; margin-right: auto;" controls="controls" loop="loop" preload="meta">Your browser does not support the audio tag.</audio>
~~~

##### 使用[hexo-tag-aplayer](https://github.com/MoePlayer/hexo-tag-aplayer)插件

###### 安装插件

```shell
npm install hexo-tag-aplayer --save
```

###### 载入标签格式如下：

~~~js
{% aplayer "歌曲名" "歌手名" "https://什么什么什么.mp3" "https://封面图.jpg" "lrc:https://歌词.lrc" %}
~~~

###### 歌单：

~~~js
{% aplayerlist %}
{
    "autoplay": false,
    "showlrc": 3,
    "mutex": true,
    "music": [
        {
            "title": "歌曲名",
            "author": "歌手名",
            "url": "https://什么什么什么.mp3",
            "pic": "https://封面图.jpg",
            "lrc": "https://歌词.lrc"
        },
        {
            "title": "歌曲名",
            "author": "歌手名",
            "url": "https://什么什么什么.mp3",
            "pic": "https://封面图.jpg",
            "lrc": "https://歌词.lrc"
        }
    ]
}
{% endaplayerlist %}
~~~

###### 网易云音乐生成外链

网页中打开[网易云官网](https://music.163.com/)

![网易云音乐](网易云音乐.png)

生成外链代码

![生成音乐外链代码](生成音乐外链代码.png)

将外链代码插入到侧边栏文件`~/themes/next/layout/_macro/sidebar.swig`

#### 添加视频

##### HTML 的标签

~~~html
<video poster="https://封面图.jpg" src="https://什么什么什么.mp4" style="max-height :100%; max-width: 100%; display: block; margin-left: auto; margin-right: auto;" controls="controls" loop="loop" preload="meta">Your browser does not support the video tag.</video>
~~~

##### [hexo-tag-dplayer](https://github.com/MoePlayer/hexo-tag-dplayer)插件

###### 安装

~~~shell
npm install hexo-tag-dplayer --save
~~~

###### 插入文章

~~~js
{% dplayer "url=https://什么什么什么.mp4" "https://封面图.jpg" "api=https://api.prprpr.me/dplayer/" "id=" "loop=false" %}
~~~

#### 文末添加相关文章

##### [hexo-recommended-posts](https://github.com/huiwang/hexo-recommended-posts)插件

```shell
npm install hexo-recommended-posts --save
```

##### 在编辑完新的文章之后，使用如下命令获取推荐列表

```
hexo recommend
```

##### 如果你对默认显示的位置和样式不满意，可以进行自定义设定。在博客根目录的 `_config.yml` 添加：

~~~yaml
# Dependency: https://github.com/huiwang/hexo-recommended-posts
recommended_posts:
  server: https://api.truelaurel.com #后端推荐服务器地址
  timeoutInMillis: 10000 #服务时长，超过此时长，则使用离线推荐模式
  internalLinks: 3 #内部文章数量
  externalLinks: 1 #外部文章数量
  fixedNumber: false
  autoDisplay: true #自动在文章底部显示推荐文章
  excludePattern: []
  titleHtml: <h1>推荐文章<span style="font-size:0.45em; color:gray">（由<a href="https://github.com/huiwang/hexo-recommended-posts">hexo文章推荐插件</a>驱动）</span></h1> #自定义标题
~~~

其中 `excludePattern` 可以添加想要被过滤的链接的正则表达式, 如配置为 `["example.com"]`, 则所有包含 `example.com` 的链接都会从推荐文章中过滤掉.

`fixedNumber` 字段用来控制是否返回固定数量的推荐文章, 如果默认推荐文章不够的话会填充当前文章的前后文章作为推荐文章.

##### 在主题根目录的 `_config.yml` 添加：

~~~yaml
# 推荐文章
recommended_posts:
  enabled: true
~~~

##### 在主题语言包中添加：

~~~yaml
# 文件位置：~/theme/next/languages/zh-CN.yml
copyright:
    author: 本文作者
    link: 本文链接
    license_title: 版权声明
    license_content: "本博客所有文章除特别声明外，均采用 %s 许可协议。转载请注明出处！"
    recommended_posts: 推荐文章
~~~

##### 主题配置文件中开启 `post-body` 后的自定义文件

~~~yaml
custom_file_path:
  postBodyEnd: source/_data/post-body-end.njk
~~~

##### 在 `~/source/_data/` 下新建文件 `post-body-end.njk`：

~~~js
<!-- 文件位置：~/source/_data/post-body-end.swig -->

{% if theme.recommended_posts.enabled and not is_index %}
<div class="post-body">
  <div class="note primary">
    <div class="recommended_posts">
      {% set recommended_posts = recommended_posts(post, site) %}
      {% if recommended_posts.length > 0 %}
        <h4>{{ __('推荐文章') }}</h4>
        <ul>
          {% for link in recommended_posts  %}
            <li><a href="{{ link.permalink }}">{{ link.title }}</a></li>
          {% endfor %}
        </ul>
      {% endif %} 
    </div> 
  </div>
</div>
{% endif %}
~~~

#### 添加结束标语

##### 同样需要在 `post-body-end.swig` 文件中添加内容，开启自定义文件的功能参考[上文](https://guanqr.com/tech/website/hexo-theme-next-customization/#文末添加相关文章)。

~~~html
<div>
    {% if not is_index %}
        <div class="end-slogan" style="text-align:center;font-size:13px;letter-spacing:10px;user-select:none;color:#bbb;"><br/>本文结束啦<i class="fa fa-star"></i>感谢您阅读<br/><br/></div>
    {% endif %}
</div> 
~~~

##### 在博客根目录的 `_config.yml` 添加：

~~~yaml
# 文章末尾添加「本文结束」标记
passage_end_tag:
  enabled: true
~~~

## 总结