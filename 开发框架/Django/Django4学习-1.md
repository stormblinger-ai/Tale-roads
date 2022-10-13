# Django4学习-1

## 使用pycharm创建新项目



- 在pycharm终端里，输入命令

  ` python -m django --version ` //该命令输出Django版本号

  ` python manage.py runserver` // 该命令为运行该项目

  ` python manage.py startapp polls` //创建polls应用，即当前目录下生成一个APP应用

  1，创建项目可直接利用pycharm文件新建创建，或者利用终端命令

  ` django-admin startproject 项目名`  //该命令会在当前目录下创建一个项目

  项目目录里主要有几个默认创建文件

  ```
  manage.py	//管理Django项目的命令行工具
  项目名文件夹：	//纯Python包，相当于库或者API，用于引用内部时的Python包名
  __init__.py	//空文件2，告诉Python解释权，这个目录应该被认为时一个Python包
  settins.py	// Django项目的配置文件
  urls.py		// URL声明，就像网站的目录，总路由
  asgi.py和wsgi.py		//	作为项目的运行在相关协议兼容的web服务器入口
  ```

  2， 创建应用，命令行用相应命令即可，映射路由

  每一个应用都是一个Python包，应用是一个专门做某件事的网络应用程序，比如博客系统，投票系统，公共记录的数据库等。

  项目，项目则是一个网站使用的配置和应用的集合。项目可以包含很多个应用，应用可以被很多个项目使用。

![image-20221013173837213](C:\Users\stormblinger\AppData\Roaming\Typora\typora-user-images\image-20221013173837213.png)

2.1， 编写视图，即views.py

主要是为了网站请求request时，响应操作，给出想要的效果，输出或数据，网页内容等

2.2， 视图编写后，要在项目中使用这个应用，需要用到映射到项目总路由，即URL声明里，也是我们需要URLconf的原因

首先，现在应用总目录下新建一个urls.py文件，并且导入刚刚编写视图文件，需要用到里面的响应内容。并在里面写入一个列表

` urlpatterns = [ path('', view.index, name='index'),]`

这样子路由就写好了，之后需要映射到项目总路由里:

` urlspatterns = [ path('polls/', include('polls.urls')),]`

这个时候，在跑起来项目，用本地8000端口加polls，就可以看到在index视图里定义的效果了

2.3，函数：鼠标放函数上，Ctrl + 左键，或右键选go to 可以看到函数说明

` include()`	//函数include（），允许引用其他URLconf，每当Django遇到include时，会截断与此项匹配的URL的部分，并将剩余的字符串发送到URLconf以供进一步处理。

-该函数，即插即用，polls应用有自己的路由，即子路由，无所谓在任何路径下，设置好都能正常工作。

-何时使用include，当包括其他URL模式时，应该总是使用include，其中` admin.site.urls`是唯一例外

` path()`函数：有四个参数，其中kwargs是指任意个关键字参数可以作为一个字典传递给目标视图函数

* route，是一个匹配URL的准则（类似正则表达式）,当Django响应一个请求时，它会从urlpatterns的第一项开始，按顺序依次匹配到列表中的项，直到找到匹配的项。但是，其不会匹配get和post参数或域名。比如：

  ```HTML
  https://.../myapp/,    //尝试匹配myapp/，处理请求
  https://.../myapp/?page=3, 	//还是只匹配到myapp/，处理请求
  ```

* view ，当Django找到了一个匹配的准则，就会调用这个特定的视图函数，并传入httprequest对象作为第一个参数，被响应的参数以关键字参数的形式传入
* name，为你的URL取名，能使你在Django的任意地方唯一地引用它，这个有用的特性允许你只改一个文件就能全局的修改某个URL模式