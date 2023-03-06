## pipenv使用常见问题

## 1 虚拟环境目录

使用pipenv创建虚拟环境后，默认放在C:\Users\用户名 \ .virtualenvs目录下

注：更换目录，通过新建环境变量实现

.为当前目录

.venv更改默认随机命名方式，创建虚拟环境文件夹为.venv

```
WORKON_HOME:.
PIPENV_CUSTOM_VENV_NAME:.venv
```

## 2 下载速度慢换源

清华大学`pip`源：`https://pypi.tuna.tsinghua.edu.cn/simple`

注：华为源速度最快，但是清华稳定

自动换源：新建环境变量

``` 
PIPENV_TEST_INDEX:https://pypi.tuna.tsinghua.edu.cn/simple
```

