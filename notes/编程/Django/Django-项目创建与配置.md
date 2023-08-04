# Django的创建与项目结构

- 虚拟环境+项目+APP创建
  1. pycharm创建纯净的Python项目+虚拟环境
  2. 安装Django和相关库API
  3. 创建APP
  4. 运行

## 1，虚拟环境+项目+APP创建（使用pycharm2022.3创建）

虚拟环境创建选择有多种，virtualenv，pipenv，conda，或者选用PDM相关的本地环境都可以，我这里主要选用virtualenv，后期项目做大，迭代版本或者多版本共存，出现三方库，包依赖管理问题时，建议conda或PDM/poetry

```
```

## 2，Django安装和创建APP

```
1，安装Django3.2
pip install django==3.2
2,安装djangorestframework
安装DRF3.x版本，包名为djangorestframework，2.x包名为django-restframework
3，在当前目录中创建项目,用.
Django-admin startproject pj .
4,创建文件夹apps/news,创建APP
python manage.py startapp apps/news
```

## 3，目录结构调整

### settings目录

```
```

4,pycharm运行配置

```
```



# Django的项目配置

## 1、常见APP注册，

```
INSTALLED_APPS = [
    # 原生
    'django.contrib.admin',
    'django.contrib.auth',
    # 三方库
    'rest_framework',
    # 自己编写
    'apps.news', 这里注意如果想直接写APP名字，需要把文件夹路径加到BASE_DIR中
    # 新增一个系统导包路径，可见上3，容易出现找不到APP问题
	# import sys
	# sys.path.insert(0,os.path.join(BASE_DIR,"apps"))
]
```





## 2、数据库配置

```
1、Django配置MySQL
-pip install pymysql
2,最开始创建的项目的init文件写，
import pymysql
pymysql.install_as_Mysqldb()
3,连接属性
‘default’：{
	'ENGINGE': 'django.db.backends.mysql'
  # 'ENGINE': 'dj_db_conn_pool.backends.mysql',连接池
	'NMAE': 数据库名字
	'USER': 
	'PASSWORD':
	'HOST': IP地址
	'PORT':
	# 设置连接池，看情况选择
	pip install django-db-connection-pool
	'POOL_OPTIONS': {
            'POOL_SIZE': 10,  # 最小
            'MAX_OVERFLOW': 10,  # 在最小的基础上，还可以增加10个，即：最大20个。
            'RECYCLE': 24 * 60 * 60,  # 连接可以被重复用多久，超过会重新创建，-1表示永久。
            'TIMEOUT':30, # 池中没有连接最多等待的时间。
        }
}
2、Django配置Redis

```

## 3、静态文件和上传文件配置

```

1、#需要安装图片文件处理模块
pip install pillow

2、#配置settings.py文件
# 访问静态文件的url地址前缀
STATIC_URL = '/static/'
# 设置django的静态文件目录
STATICFILES_DIRS = [
    os.path.join(BASE_DIR,"static")
]

3、# 项目中存储上传文件的根目录[暂时配置]，注意，uploads目录需要手动创建否则上传文件时报错
MEDIA_ROOT=os.path.join(BASE_DIR,"uploads")
# 访问上传文件的url地址前缀
MEDIA_URL ="/media/"

4、总路由urls.py
#from django.urls import re_path
from django.conf import settings
from django.views.static import serve

urlpatterns = [
  	...
    re_path(r'media/(?P<path>.*)', serve, {"document_root": settings.MEDIA_ROOT}),
]
```

## 4、日志配置

## 5、文档配置

## 6、异常配置

# Django项目API接口创建

## 1、创建APP

```
python manage.py startapp name或者
django-admin startapp name apps/name
```

## 2、APP注册

```
settings.py文件
INSTALLAPP = [
'name' 或者 'apps/name'
]
```



## 3、模型models创建/迁移

- models.py

```
-导Django自带模型包
from django.db import models
-声明模型表数据类
class Flower(models.model)  # 命名用单词
-字段名 = 字段类型（字段约束）
name = models.charField(max_length= , verbose_name=)
-外键/表关系
name = models.ForeignKey(to="Dictionary", db_constraint=False, on_delete=models.PROTECT, blank=True, null=True,verbose_name="父级", help_text="父级")
-自定义函数
def __str__():
	return self.title
-class meta、表信息声明
class_Meta:
	verbose_name = 
```

- 迁移

```
python manage.py makemigrations
python manage.py migrate
```

- 常用选项

```
-数值型
AutoField	主键自增 int(11)
IntegerField	有符号整数 int(11)
PostiveIntegerField	正整数 init(11)
SmallInteterField	整数 smallint
DecimalField	定点数 decimal
　　max_digits：总位数，decimal_places：小数位
BooleanField	布尔 tinyint(1)
-字符型
CharField	varchar
　　max_length：最大长度
TextField	大文本longtext
URLField	字符串 *********
UUIDField	char(32)
EmailField	email
FileField	文件
ImageField	图片
-日期类型
字段	说明
DateField	date
DateTimeField	datetime
TimeField	time
-关系类型
ForeignKey	多对一
OneToOneField	一对一
ManyToManyField	多对多
    through
    on_delete
    	CASCADE:这就是默认的选项，级联删除，你无需显性指定它。
    	models.DO_NOTHING
		PROTECT: 保护模式，如果采用该选项，删除的时候，会抛出ProtectedError错误。
		SET_NULL: 置空模式，删除的时候，外键字段被设置为空，前提就是blank=True, null=True,定义该字段的时候，允许为空。
		SET_DEFAULT: 置默认值，删除的时候，外键字段设置为默认值，所以定义外键的时候注意加上一个默认值。
		SET(): 自定义一个值，该值当然只能是对应的实体了
    related_name
    to: 利用orm自动创建第三张关系表
    db_constraint

2 字段约束
2.1 表内约束
db_index	建立索引
db_column	字段名
primary_key	主键
null	表内为空
unique	唯一
unique_for_date	日期唯一
unique_for_year	年份唯一
unique_for_month	月份唯一
default	默认值
required 必填项
2.2 业务层约束
blank	提交可以为空
chioces	选项
editable	是否可编辑
verbose_name	业务层展示名字
error_message	验证失败的异常提示
validators	自定义检验
help_text	字段提示语
```

### classmeta

```
-classmeta:
使用内部meta类 来给模型赋予元数据，所谓模型的元数据即“所有不是字段的东西”，比如培训选项（ordering）,数据库表名（db_table），或是阅读友好的单复数名（verbose_name和verbose_name_plural）。这些都不是必须的，并且在模型当中添加meta类也完全是可选的
-自定义方法：
在模型中添加自定义方法会给你的对象提供自定义的“行级”操作能力。与之对应的是类manager的方法意在提供“表级”的操作，模型方法应该在某个对象实例上生效。这是一个将相关逻辑代码放在一个地方的技巧--模型。
*__ str__():返回值展示了一个对象。Python和Django在要将模型实例展示为纯文本时调用。最有可能的应用场景是交互式控制台或后台，你将会经常定义此方法；默认提供的不是很好用
*get_absolute_url():该方法告诉Django如何甲酸一个对象的URL，Django在后台接口使用此方法，或任意时间它需要计算一个对象的URL。任何需要一个唯一URL的对象需要定义此方法
```



## 4、后台admin集成

当前APP/apps.py

```
admin.site.register(模型名)
```



## 5、序列化器serializer编写

serializers.py

```
-导DRF序列化器包
from rest_framework import serializers
-导入模型表
from .models import name
-声明序列化器类名（继承）
class nameModelSerializer(serializers.ModelSerializer):
    """
    name序列化器
    """
    class Meta:
        model = name
        fields = ["id","name"]
```

- 常用选项

```
```



## 6、视图view

view.py

```
-导DRF视图包
from rest_framework.generics import ListAPIView
-导入模型类
from .models import CourseCategory
-导入序列化器类
from .serializers import CourseCategoryModelSerializer
-声明视图类名（继承）
class CourseListAPIView(ListAPIView):
-queryset对象和条件
	queryset = Course.objects.filter(is_show=True, is_deleted=False).order_by("orders","-id")
-序列化器
    serializer_class = CourseModelSerializer
-附加条件
    filter_backends = [DjangoFilterBackend, OrderingFilter]
    filter_fields = ['course_category']
    ordering_fields = ('id', 'students', 'price')
    pagination_class = CoursePageNumberPagination

```

- 常用选项

```
-视图包GenericAPIView
##  GenericAPIView的视图子类
（1）CreateAPIView
提供了post方法，内部调用了create方法
继承自： GenericAPIView、CreateModelMixin
#### （2）ListAPIView
提供了get方法，内部调用了list方法
继承自：GenericAPIView、ListModelMixin
#### （3）RetrieveAPIView
提供了get方法，内部调用了retrieve方法
继承自: GenericAPIView、RetrieveModelMixin
#### （4）DestoryAPIView
提供了delete方法，内部调用了destory方法
继承自：GenericAPIView、DestoryModelMixin
#### （5）UpdateAPIView
提供了put和patch方法，内部调用了update和partial_update方法
继承自：GenericAPIView、UpdateModelMixin
#### （6）ListCreateAPIView
提供了get和post方法，内部调用了list和create方法
继承自：GenericAPIView、ListModelMixin、CreateModelMixin
#### （7）RetrieveUpdateAPIView
提供 get、put、patch方法
继承自： GenericAPIView、RetrieveModelMixin、UpdateModelMixin
#### （8）RetrieveDestoryAPIView
提供 get、delete方法
继承自：GenericAPIView、RetrieveModelMixin、DestoryModelMixin
#### （9）RetrieveUpdateDestoryAPIView
提供 get、put、patch、delete方法
继承自：GenericAPIView、RetrieveModelMixin、UpdateModelMixin、DestoryModelMixin

-queryset条件

3.1 链式调用接口

all()	查询所有
filter(条件)	按条件查询
exclude(条件)	查询不满足条件的
reverse()	把QuerySet结果倒叙排列
distinct()	去重
none	返回空的QuerySet
3.2 非链式调用接口

get	返回满足条件的唯一结果，多或没有满足都报异常
create	写入数据库并创建对象
get_or_create	没有get到就create
update_or_create	没有update到就create
count	QuerySet的结果数量
latest	最新的一条记录
　　必填字段：field_name，按字段排序的最新一条
earliest	最早的一条记录
　　必填字段：field_name，按字段排序的最早一条
first	QuerySet第一条
last	QeurySet最后一条
exists	是否存在
bulk_create	多条创建，返回list
in_bulk	批量查询
update	更新
delete	删除
values	返回字段值
values_list	返回tuple的QuerySet
比如：
# 多条创建
user_s = User.objects.bulk_create([User(name='name%s' % i) for i in range(10)])
# User中最新的一条
user_latest = User.objects.latest(field_name='id')

3.3 进阶接口

defer	需要的字段延时加载
only	只加载指定字段
select_related	多对一查询，只能从多中查一，不能一中查多
prefetch_related	多对多关联查询
比如：
博客懒加载内容
# 懒加载name
topic_s = Topic.objects.all().defer('name')
# 多对一查询关联sender，避免N+1次查询
topic_s = Topic.objects.all().select_related('sender')
# 多对多关联collectors，避免N+1次查询
topic_s = Topic.objects.all().prefetch_related('collectors')
only案例同defer

3.4 字段查询

contains	包含
icontains	包含、忽略大小写
exact	相等
iexact	相等、忽略大小写
in	范围查询
gt	大于
gte	大于等于
lt	小于
lte	小于等于
startwith	开头
istartwith	开头、忽略大小
endswith	结尾
iendswith	结尾、忽略大小
range	范围查询

3.5 进阶查询

F	表内字段比较
Q	逻辑比较
Count	个数
Sum	求和
比如：
User.objects.get(id=Count('user')
User.objects.get(id=Sum('money'))
```



## 7、路由urls、子路由——>总路由

当前app/urls.py

```
-导Django自带URL包中的path和re_path
from django.urls import path,re_path
-导入视图类
from . import views
-urlpattrns=[]列表
urlpatterns = [
    path(r"category/", views.CourseCategoryListAPIView.as_view() ),
    path(r"", views.CourseListAPIView.as_view() ),
    re_path(r"(?P<pk>\d+)/", views.CourseRetrieveAPIView.as_view()),
    path(r"chapter/", views.CourseChapterListAPIView.as_view()),
]
```

当前项目/urls.py

```
-按需添加内容
urlpatterns = [
    re_path(r'media/(?P<path>.*)', serve, {"document_root": settings.MEDIA_ROOT}),
    path(r'^ckeditor/', include('ckeditor_uploader.urls')),
    path('', include('home.urls')),
    path('user/', include('user.urls')),
]
```

- 常用选项

```
```



# Django三方组件库和插件

```
    Django-allauth - 用户注册登录管理
    官网地址：https://django-allauth.readthedocs.io/en/latest/

    Django-haystack - 全文检索引擎

    官网地址：https://django-haystack.readthedocs.io/en/master/
    全文检索不同于标题的简单匹配，是一件技术难度比较高的活。当文章很长时，很难找到精确的匹配，同时搜索全文需要消耗大量的计算资源。有了haystack，在django中直接添加搜索功能，像搜索标题一样搜索全文，而无需关注索引建立、搜索解析等技术问题。haystack支持多种搜索引擎，不仅仅是whoosh，使用solr、elasticsearch等搜索，也可通过haystack，而且直接切换引擎即可，甚至无需修改搜索代码。

    Django-ckeditor - 富文本编辑器

    官网地址：https://django-ckeditor.readthedocs.io/en/latest/
    django没有提供官方的富文本编辑器，而ckeditor恰好是内容型网站后台管理中不可或缺的控件。ckeditor是一款基于javascript，使用非常广泛的开源网页编辑器。它允许用户直接编写图文，插入列表和表格，并支持文本和HTML格式代码输入。

    Django-imagekit - 自动化处理图像

    官网地址：https://django-imagekit.readthedocs.io/en/latest/
    现代网站开发一般免不了处理一些图片，例如头像、用户上传的图片等内容。django-imagekit 帮配合 django 的 model 模块自动完成图片的裁剪、压缩、生成缩略图、加水印等一系列图片相关的操作。

    Django-debug-toolbar - django项目调试利器

    官网地址：https://django-debug-toolbar.readthedocs.io/en/latest/
    该工具给django web开发提供了强大的调试功能，包括查看执行的sql语句，db查询次数，request，headers，调试概览等。通过安装插件Pympler，还可以了解内存使用情况。

    Django-celery - 执行异步任务或定时任务的最佳选择

    官网地址：http://docs.celeryproject.org/en/latest/django/index.html
    https://docs.celeryproject.org/en/stable/django/first-steps-with-django.html
    Celery 是一款非常简单、灵活、可靠的分布式系统，可用于处理大量消息，并且提供了一整套操作此系统的一系列工具。另外，Celery 是一款消息队列工具，可用于处理实时数据以及任务调度。官网给出了 Celery 如下四个特点：

    • 简单：开箱机用，维护简单，不需要使用配置文件；
    • 高可靠性：如果连接丢失或者出现故障，客户端进程会自动重试。一些 broker 会以主/主或者主/备的方式维持系统的高可靠性；
    • 快速：单个 Celery 进程每分钟内可以处理数百万个任务，往返的延迟在毫秒级别(使用RabbitMQ 做中间件，再加上一些优化的设置)；
    • 灵活：几乎 Celery 的每个部分都可以自行扩展，如使用自定义池、序列化器，压缩方案、日志记录，调度程序，消费者，生产者，代理传输等等。
      

    注意到基于 Django 框架构建的 Web 系统实际上是一个同步服务。这意味着当客户端发起一个请求时，后端只有在视图函数处理完后才会返回结果。如果这个请求背后要做的工作比较耗时，或者因为某种原因导致非常耗时，那么此时客户端会一直等待请求的响应，这非常影响用户体验。对于一个优秀的网站而言，良好的用户体验十分重要，这也说明了一个支持异步功能的第三方插件的重要性。为了能让 Django 搭建的 Web 系统支持这样的异步功能，于是 django-celery 便应运而生。django-celery项目之后也被移到 celery 下进行统一管理。
    django-celery是django web开发中执行异步任务或定时任务的最佳选择。它的应用场景包括:

    * 异步任务: 当用户触发一个动作需要较长时间来执行完成时，可以把它作为任务交给celery异步执行，执行完再返回给用户。这点和在前端使用ajax实现异步加载有异曲同工之妙。
    * 定时任务。假设有多台服务器，多个任务，定时任务的管理是很困难的，要在不同电脑上写不同的crontab，而且还不好管理。Celery可以帮助快速在不同的机器设定不同任务。
    * 其他可以异步执行的任务。比如发送短信，邮件，推送消息，清理/设置缓存等。这点还是比较有用的。
       
    Django-constance - 常量管理

    官网地址：https://django-constance.readthedocs.io/en/latest/
    有时会在 django 的 settings 中设置一些常量，但是有可能会进行变更。利用这个包，只需简单的配置就可以自动生成 admin 管理后台可以修改管理常量。

    Django-filter - 过滤数据使用

    官网地址：https://django-filter.readthedocs.io/en/latest/
    Django-filter允许用户模型字段进一步过滤从数据库查得到的queryset，从而筛选显示用户想要的查询结果，这样避免了对数据库的再次查询，大大提升了效率。比如用户访问文章列表页面，已经看到了文章清单，但用户还希望通过标题关键词进一步在查询结果中筛选出自己感兴趣的文章清单(而不必重新查询数据库)，这时使用Django-filter就可以轻松帮实现上述需求。
  
    Django-guardian

    官网地址：https://django-guardian.readthedocs.io/en/stable/index.html
    在介绍Django权限管理时，提到过Django没有提供对象（Object）级别的权限控制。一个用户如果对Article模型有修改的权限，那么他将对所有Article对象有修改权限。使用django-guardian可以帮助实现对对象级别（比如某篇具体文章）的权限控制。

    Django-activity-stream

    官网地址：https://github.com/justquick/django-activity-stream
    Django-activity-stream是一个Django应用程序，用于记录和显示活动流，例如用户活动、社交事件和系统通知。它可以帮助开发人员更好地跟踪和展示应用程序中的活动。
    如果要开发一个社交类网站，需要实现用户关注、收藏、点赞、用户动态等功能时，这个第三方库可以快速帮实现并提供用户的活动流。出于学习目的话，该项目的源码也是一个很好的学习对象。

  

    Django-pure-pagination

    官网地址：https://github.com/jamespacileo/django-pure-pagination
    Django虽然自带强大分页给功能，但当页数非常多时显示结果对用户并不友好。比如有1000页，那么1到1000每个数字都会出现页面上。使用django-pure-pagination则可以快速实现如下按范围展示的完美分页方式，值得一试。

    

    Django-rules

    官网地址：https://github.com/dfunckt/django-rules
    Django-rules是一个Django应用程序，用于定义简单的规则系统，在运行时根据这些规则授权、限制或过滤用户行为。它可以帮助开发人员更轻松地实现复杂的授权和权限管理。
    一个小巧但是强大的应用，提供对象级别的权限管理，且不需要使用数据库。


    django-import-export

    官网地址：https://django-import-export.readthedocs.io/en/latest/
    django-import-export是一个Django应用程序，用于导入和导出数据到和从多种格式，例如CSV、JSON、Excel等。它可以帮助开发人员更方便地管理数据导入和导出。

    django-watchman

    官网地址：https://django-watchman.readthedocs.io/en/stable/
    django-watchman是一个Django应用程序，用于监控Django应用程序运行状况、数据库连接、缓存、文件改变等。它可以帮助开发人员更实时地监测应用程序的状态。

    django-redis

    官网地址：https://github.com/jazzband/django-redis
    django-redis是一个Django应用程序，用于管理Redis数据库连接和缓存。它可以帮助开发人员更高效地处理Redis缓存和数据存储。

    django-cors-headers

    官网地址：https://github.com/ottoyiu/django-cors-headers
    django-cors-headers是一个Django应用程序，用于跨域资源共享，过滤cors。它可以帮助开发人员更轻松地处理跨域请求和响应。

    pymysql - 连接mysql数据库

    官网地址：https://github.com/PyMySQL/PyMySQL
    pymysql是Python的一个MySQL数据库接口，可以帮助开发人员更方便地连接和操作MySQL数据库。

    

    Django-simpleui

    官网地址：https://github.com/newpanjing/simpleui
    Django-simpleui是一个Django的扩展，提供了基于Bootstrap的后台模板和界面组件。它可以帮助开发人员更快速地创建美观的后台管理界面。

    Django-elasticsearch-dsl

    官网地址：https://django-elasticsearch-dsl.readthedocs.io/en/latest/index.html
    Django-elasticsearch-dsl是一个Django应用程序，用于更方便地与Elasticsearch搜索引擎集成。它可以帮助开发人员更轻松地处理搜索和过滤逻辑。

    Django-prometheus

    官网地址：https://github.com/korfuri/django-prometheus
    Django-prometheus是一个Django应用程序，用于与Prometheus监控系统集成，提供更好的应用程序性能和运行状态监控。它可以帮助开发人员更实时地监测和处理应用程序运行状况。

    Django-bulk-update

    官网地址：https://github.com/aykut/django-bulk-update
    Django-bulk-update是一个Django应用程序，用于更高效地批量更新Django模型。它可以帮助开发人员更快速地更新和处理模型数据。

    restful-Api使用

    官网地址 https://restfulapi.net/
    restful-Api是一种设计和开发Web API的方式，是一种基于HTTP协议的、符合RESTful风格的Web API，可以用于前后端分离的Web开发，通过对资源的描述和HTTP动词的定义，实现对客户端的请求和响应。

    grpcio - rpc通信的第三方库

    官网地址：https://grpc.io/
    grpcio是一个通用的、高性能的RPC框架，基于Google开发的gRPC协议，支持多种编程语言和平台，在分布式系统中应用广泛。其主要作用是简化服务之间的通信，提高效率和可维护性。

    simplejwt - 用于登录认证的权限

    官网地址：https://django-rest-framework-simplejwt.readthedocs.io/en/latest/
    simplejwt是对Django REST framework的一个插件，用于提供基于JSON Web Tokens (JWT)的认证系统，可以用于开发安全的RESTful API服务。
    具体使用参考：7、DRF实战总结：JWT认证原理和使用及第三方库simplejwt 的详解（附源码）

    drf-yasg

    官网地址：https://drf-yasg.readthedocs.io/en/stable/
    drf-yasg是一个为Django REST framework设计的OpenAPI文档生成工具，可以自动生成API文档，提供Swagger UI和Redoc两种风格的API文档展示方式。

    django-extensions

    官网地址：https://django-extensions.readthedocs.io/en/latest/
    django-extensions是一个Django扩展模块，可以提供一些工具和命令，方便开发者进行Django应用的开发和调试，包括但不限于Shell命令增强、数据库命令简化、代码检查等。
    具体使用参考：七、Django进阶：第三方库Django-extensions的开发使用技巧详解（附源码）

    drf-extensions

    官网地址：https://drf-extensions.readthedocs.io/en/latest/
    drf-extensions是对Django REST framework的一个扩展，提供了一系列常用的、易用的DRF功能增强，包括但不限于缓存、过滤、排序、搜索等。这些增强能够降低开发难度，提高WebAPI性能和可用性。
   
```



# Django项目常见错误

