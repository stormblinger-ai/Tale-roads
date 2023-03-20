# 1.0 安装工具包

- pipenv

pipenv 是 Pipfile 主要倡导者、requests 作者 Kenneth Reitz  写的一个命令行工具，主要包含了Pipfile、pip、click、requests和virtualenv，能够有效管理Python多个环境，各种第三方包及模块。

**pipenv 的主要特性：**

> 1. **pipenv集成了pip，virtualenv两者的功能，且完善了两者的一些缺陷。**
> 2. **过去用virtualenv管理requirements.txt文件可能会有问题，Pipenv使用Pipfile和Pipfile.lock，后者存放将包的依赖关系，查看依赖关系是十分方便。**
> 3. **各个地方使用了哈希校验，无论安装还是卸载包都十分安全，且会自动公开安全漏洞。**
> 4. **通过加载.env文件简化开发工作流程。**
> 5. **支持Python2 和 Python3，在各个平台的命令都是一样的。**

- 安装pipenv

​		1，cmd窗口下直接： pip install pipenv

​		2, 换国内镜像源    

用pip管理工具安装库文件时，默认使用国外的源文件，因此在国内的下载速度会比较慢，可能只有50KB/s。

比较常用的国内镜像有：

（1）阿里云 http://mirrors.aliyun.com/pypi/simple/
（2）豆瓣http://pypi.douban.com/simple/
（3）清华大学 https://pypi.tuna.tsinghua.edu.cn/simple/
（4）中国科学技术大学 http://pypi.mirrors.ustc.edu.cn/simple/
（5）华中科技大学http://pypi.hustunique.com/

设置方法：（以清华镜像为例）
（1）临时使用：
可以在使用pip的时候，加上参数-i和镜像地址(如
https://pypi.tuna.tsinghua.edu.cn/simple)，
例如：pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pandas，这样就会从清华镜像安装pandas库。

# 2 创建虚拟环境

pipenv常用命令

1、pipenv --python版本， 在当前目录下初始化虚拟环境

```Python
pipenv --two  # 使用当前系统中的Python2 创建环境
pipenv --three  # 使用当前系统中的Python3 创建环境

pipenv --python 3  # 指定使用Python3创建环境
pipenv --python 3.6  # 指定使用Python3.6创建环境
pipenv --python 2.7.14  # 指定使用Python2.7.14创建环境
```

1.1、修改依赖镜像，想要安装依赖包速度快点，可以修改Pipfile文件里面的源(直接修改url地址)，常见的国内python加速镜像源：

2、pipenv shell 启动当前目录下的虚拟环境，如果当前目录下无虚拟环境则会在当前目录下自动创建虚拟环境。启动了虚拟环境，执行python脚本才能正确找到依赖模块

```bash
 aiden@**** pipenv_01 % pipenv shell
     Launching subshell in virtual environment...
      . /Users/aiden/.local/share/virtualenvs/pipenv_01-IEV0TcCW/bin/activate
     aiden@**** pipenv_01 %  . /Users/aiden/.local/share/virtualenvs/pipenv_01-IEV0TcCW/bin/activate
     (pipenv_01) aiden@**** pipenv_01 % 
     # 注：环境经激活后，会发现命令提示符变了：（下图中的“pipenv_01”前缀，表示生成了名为“pipenv_01-IEV0TcCW”的虚拟环境
     
     # 执行python脚本
     (pipenv_01) aiden@**** pipenv_01 % python3.8 panda_study/demo.py 
```

3、安装或卸载依赖模块到虚拟环境中

```bash
  pipenv install XXX  # 安装XXX模块并加入到Pipfile
     pipenv install XXX==1.11  # 安装固定版本的XXX模块并加入到Pipfile
     pipenv install pytest --dev # 仅仅安装开发环境下的依赖包（项目部署上线不需要的包）
     
     pipenv uninstall XXX  # 卸载XXX模块并从Pipfile中移除
     pipenv uninstall --all  # 卸载全部包并从Pipfile中移除
     pipenv uninstall --all-dev  # 卸载全部开发包并从Pipfile中移除
```

4、pipenv graph 查看当前虚拟环境下的所有依赖，在虚拟环境中安装了pandas和requests两个库：

```swift
 aiden@**** pipenv_01 % pipenv graph
     pandas==1.2.5
       - numpy [required: >=1.16.5, installed: 1.21.0]
       - python-dateutil [required: >=2.7.3, installed: 2.8.1]
         - six [required: >=1.5, installed: 1.16.0]
       - pytz [required: >=2017.3, installed: 2021.1]
     requests==2.25.1
       - certifi [required: >=2017.4.17, installed: 2021.5.30]
       - chardet [required: >=3.0.2,<5, installed: 4.0.0]
       - idna [required: >=2.5,<3, installed: 2.10]
       - urllib3 [required: >=1.21.1,<1.27, installed: 1.26.5]
```

5、pipenv --venv 查看当前虚拟环境的安装路径

```bash
  aiden@**** pipenv_01 % pipenv --venv
     /Users/aiden/.local/share/virtualenvs/pipenv_01-IEV0TcCW
```

6、exit 退出当前虚拟环境，命令行提示符前缀消失：

```bash
  (pipenv_01) aiden@**** pipenv_01 % exit
     Saving session...
     ...copying shared history...
     ...saving history...truncating history files...
     ...completed.
     Deleting expired sessions...      24 completed.
     aiden@**** pipenv_01 % 
```

7、pipenv update * 升级部分包或依赖

```bash
  pipenv update --outdated  # 查看所有需要更新的依赖项
     pipenv update  # 更新所有包的依赖项
     pipenv update <包名>  # 更新指定的包的依赖项
```

8、pipenv  check  检查安全漏洞

9、requirements文件兼容，pipenv可以像virtualenv一样用命令生成requirements文件：

```css
  pipenv lock -r > requirements.txt  # 将Pipfile和Pipfile.lock文件里面的包导出为requirements.txt文件
     pipenv lock -r --dev > requirements.txt  # 将Pipfile和Pipfile.lock文件里面的开发包导出为requirements.txt文件
```

10、pipenv通过requirements文件安装模块：

```css
 pipenv install -r requirements.txt
     pipenv install -r --dev requirements.txt  # 只安装开发包
```

pipenv项目部署

```bash
 git clone project.git
     
     cd project
     
     pip3 install pipenv
     
     pipenv shell
     
     pipenv sync # 如果要安装跟Pipfile.lock版本一致的包，则执行pipenv sync; 
     pipenv install # 如果不需要与Pipfile.lock版本一致，则执行pipenv install即可
     
     pipenv run python main.py
```

![image-20230129121003514](D:\工具\Typora\image\image-20230129121003514.png)

![image-20230129121013641](D:\工具\Typora\image\image-20230129121013641.png)

![image-20230129121028418](D:\工具\Typora\image\image-20230129121028418.png)