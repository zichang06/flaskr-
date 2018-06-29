---
title: flask learning
---
# flask learning
## 前言
系分大佬队友说在学习flask，于是我也着手学习flask的内容。
Flask是微型web框架，框架本身十分精简，微型并不代表其功能弱，核心代码基于Werkzeug, Jinja 2 这两个库,它以插件形式的进行功能扩展，且插件易于安装与使用，并且可以自行开发扩展插件。与其他web框架类似，flask中请求(request),路由(route),响应(response)构成其完整的一个基本http流程。作为入门，flask框架非常易于使用，了解其基本结构后，可以迅速进行MVC开发，或者将其当作后端restfulApi来响应数据。
- 参考资料
[官方中文文档——欢迎来到 Flask 的世界](http://dormousehole.readthedocs.io/en/latest/index.html)
[知乎——flask的学习顺序](https://www.zhihu.com/question/20135205)
![后端学习路线](http://www.runoob.com/wp-content/uploads/2018/01/weblearnpath3.png)
目前我主要参考的是官方文档中文版，按照教程搭了最简单的博客模型。
[官方中文文档——教程](http://dormousehole.readthedocs.io/en/latest/tutorial/index.html)
该教程引领创建一个名为 Flaskr 的具备基本功能的博客应用。应用用户可以 注册、登录、发贴和编辑或者删除自己的帖子。我在此记录一下学习过程。
- 登陆/注册界面
![登录界面](https://github.com/zichang06/markdownPics/blob/master/flask1.png?raw=true)
- 博客界面
![博客界面](https://github.com/zichang06/markdownPics/blob/master/flask2.png?raw=true)

## 准备
我打开了Anaconda Prompt，激活了之前已经安装好的python36虚拟环境，执行安装flask语句即可。项目的构建用的是VScode。
安装flask以及在本地运行文件如下图所示：
我的github仓库地址：[flaskr_blog](https://github.com/zichang06/flaskr_blog)
除了测试部分，总体文件布局如下图所示
![](https://github.com/zichang06/markdownPics/blob/master/flas31.png?raw=true)

## 一、[应用工厂](http://dormousehole.readthedocs.io/en/latest/tutorial/factory.html)

一个 Flask 应用是一个 [`Flask`](http://dormousehole.readthedocs.io/en/latest/api.html#flask.Flask) 类的实例。应用的所有东西（例如配置 和 URL ）都会和这个实例一起注册。
创建一个 Flask 应用最粗暴直接的方法是在代码的最开始创建一个全局 [`Flask`](http://dormousehole.readthedocs.io/en/latest/api.html#flask.Flask) 实例。前面的 “Hello, World!” 示例就是这样做的。有的情况下这 样做是简单和有效的，但是当项目越来越大的时候就会有些力不从心了。
可以在一个函数内部创建 [`Flask`](http://dormousehole.readthedocs.io/en/latest/api.html#flask.Flask) 实例来代替创建全局实例。这个函数被 称为 **应用工厂application factory** 。所有应用相关的配置、注册和其他设置都会在函数内部完成， 然后返回这个应用。
`create_app` 是一个应用工厂函数。
完成该步骤后，可以在浏览器看到下图：
![](https://github.com/zichang06/markdownPics/blob/master/flask5.png?raw=true)
## 二、[定义和操作数据库](http://dormousehole.readthedocs.io/en/latest/tutorial/database.html)
应用使用一个 SQLite 数据库来储存用户和博客内容。 Python 内置了 SQLite 数据库支持，相应的模块为 sqlite3 
- 连接数据库
	创建flaskr/db.py文件，创建一个数据库的连接，所有查询和操作都要通过该连接来执行，完事后需要关闭连接。
- 创建表
	创建flaskr/schema.sql文件， Flaskr 会把用户数据储存在 user 表中，把博客内容储存在 post 表中。
- 创建表
	首先在flaskr/schema.sql中创建数据存储。之后在flaskr/db.py中添加python函数，以运行该SQL命令。
- 在应用中注册
	close_db 和 init_db_command 函数需要在应用实例中注册，否则无法使用。 然而，既然我们使用了工厂函数，那么在写函数的时候应用实例还无法使用。代替地， 我们写一个函数，把应用作为参数，在函数中进行注册
- 初始化数据库文件
	现在init-db已经在应用中注册好了。先停止正在运行的服务器，运行init-db命令，如下图所示，现在会有一个flaskr.sqlite文件出现在项目所在文件夹的 instance 文件夹中。
	![](https://github.com/zichang06/markdownPics/blob/master/flask8.png?raw=true)

## 三、[蓝图Blueprints 和视图Views](http://dormousehole.readthedocs.io/en/latest/tutorial/views.html)
视图是一个应用对请求进行响应的函数。 Flask 通过模型把进来的请求 URL 匹配到 对应的处理视图。视图返回数据，Flask 把数据变成出去的响应。Flask 也可以反过来，根据视图的名称和参数生成 URL 。
Blueprint 是一种组织一组相关视图及其他代码的方式。与把视图及其他代码直接注册到应用的方式不同，蓝图方式是把它们注册到蓝图，然后在工厂函数中 把蓝图注册到应用。
Flaskr 有两个蓝图，一个用于认证功能，另一个用于博客帖子管理。每个蓝图的代码都在一个单独的模块中。使用博客首先需要认证，因此我们先写认证蓝图。
- 创建蓝图
	首先创建名为auth.py的文件，在其中创建名为auth的蓝图，再在__init__.py中，导入并注册蓝图。
- 视图一：注册
	当用访问 /auth/register URL 时， register 视图会返回用于填写注册 内容的表单的 HTML 。当用户提交表单时，视图会验证表单内容，然后要么再次 显示表单并显示一个出错信息，要么创建新用户并显示登录页面
- 视图二：登陆
	类似于注册。之后用户的 id 被储存在 session 中，可以被后续的请求使用。在每个请求的开头，如果用户已登录，那么其用户信息应当被载入，以使其可用于其他视图
- 是图三：注销
	注销的时候需要把用户 id 从 session 中移除。 然后 load_logged_in_user 就不会在后继请求中载入用户了。

## 四、[模板](http://dormousehole.readthedocs.io/en/latest/tutorial/templates.html)
模板文件会储存在 flaskr 包内的 templates 文件夹内。模板是包含静态数据和动态数据占位符的文件。模板使用指定的数据生成最终的文档。 Flask 使用 Jinja 模板库来渲染模板。值得注意的是，应用中的每一个页面主体不同，但是基本布局是相同的。每个模板会 扩展 同一个 基础模板并重载相应的小节，而不是重写整个 HTML 结构。在flaskr/templates/路径下有个base.html的文件，之后的注册、登陆界面都可以根据该文件进行拓展。

## 五、[静态文件](http://dormousehole.readthedocs.io/en/latest/tutorial/static.html)
可以使用一些 CSS 给 HTML 添加点样式。样式不会改变，所以应当使用 静态文件 ，而不是模板。Flask 自动添加一个 static 视图，视图使用相对于 flaskr/static 的相对路径。除了 CSS ，其他类型的静态文件可以是 JavaScript 函数文件或者 logo 图片。它们 都放置于 flaskr/static 文件夹中，并使用 url_for('static', filename='...') 引用。
此时，登陆、注册界面已经基本完成了，注册一个账号后，自动跳转置登录界面，此时若账号密码发生错误，会自动提示错误。截取登陆界面如下图所示：
![](https://github.com/zichang06/markdownPics/blob/master/flask9.png?raw=true)

## 六、[博客蓝图](http://dormousehole.readthedocs.io/en/latest/tutorial/blog.html)
博客蓝图与验证蓝图所使用的技术一样。博客页面应当列出所有的帖子，允许已登录 用户创建帖子，并允许帖子作者修改和删除帖子。
- 定义蓝图并注册到应用工厂
	首先创建名为blog.py的文件，在其中创建名为blog的蓝图，再在__init__.py中，导入并注册蓝图。
- 视图一：索引页
	索引会显示所有帖子，最新的会排在最前面。在flaskr/blog.py中添加相关路由和函数，并在flaskr/templates/blog/下添加index.html
- 视图二：创建新博文
	create 视图与 register 视图原理相同。要么显示表单，要么发送内容 已通过验证且内容已加入数据库，或者显示一个出错信息。先前写的 login_required 装饰器用在了博客视图中，这样用户必须登录以后 才能访问这些视图，否则会被重定向到登录页面。
	在flaskr/blog.py中添加相关路由和函数，并在flaskr/templates/blog/下添加create.html
	![](https://github.com/zichang06/markdownPics/blob/master/flask11.png?raw=true)
- 视图三：修改博文
	在flaskr/blog.py中添加相关路由和函数，并在flaskr/templates/blog/下添加update.html
	![](https://github.com/zichang06/markdownPics/blob/master/flask10.png?raw=true)
- 视图四：删除博文
	删除视图没有自己的模板。删除按钮已包含于 update.html 之中，该按钮指向 /<id>/delete URL 。既然没有模板，该视图只处理 POST 方法并重定向到 index 视图。
	

## 后记
至此，第一个简单的flask应用已经完成。flask是个轻便的后端框架，可以嵌入web网页开发，也可以开发安卓后端。这篇博文简单记述了我第一次成功搭建的过程，并梳理了一个小型博客应用的思路。其中有很多细节、原理，我还没有弄懂，因为自己没学过安卓开发，暂时也不清楚如何应用到安卓后端。之后的学习之路还很漫长，一步一个脚印，逐步开始吧！
