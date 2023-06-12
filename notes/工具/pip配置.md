mkdir ~/.pip  #创建一个名为.pip的文件夹
cd ~/.pip    #进入创建的文件夹
touch pip.conf  #创建pip.conf
sudo gedit ~/.pip/pip.conf   #编辑文件

复制下面的内容到文件中（配置的豆瓣源，也可以配置别的）
 [global]
 index-url = https://pypi.douban.com/simple
 [install]
 trusted-host=pypi.douban.com