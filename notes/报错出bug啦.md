## docker

```
-1、permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/images/json": dial unix /var/run/docker.sock: connect: permission denied
解决方法：没有权限建议命令加sudo或者直接su root 切换到root用户
```

## Ubuntu

```
-1、su root切换用户失败
使用sudo passwd重置密码，再登录即可，因为安装时默认锁定了root用户
```

