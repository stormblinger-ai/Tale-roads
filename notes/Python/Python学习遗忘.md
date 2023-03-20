# Python基本类型

1，多个变量接收列表值

![image-20230109165514957](D:\工具\Typora\image\image-20230109165514957.png)

![image-20230109165527706](D:\工具\Typora\image\image-20230109165527706.png)

# 语法糖

Python语法糖是代码特殊写法，使得代码更简洁，形式更直观

1，三元运算表达式；在表达式比较简单的时候用：

运算效率和if判断正常形式差不多，但可读性高，判断交条件复杂时不适用

``` py
x if x > y else y
```

2，列表推导式

[表达式 for i in L if语句]

采用中右左形式,基于三步走

l = [1, 2, 3]

print([ i for i in l if i%2==0])

[]											--构建新列表

1,for i in l:							--for循环

2，		if i%2==0:

3，			[].append(i)		--操作

3,字典推导式

字典推导式同理列表，也是三步走形式的更改为推导式

列表不可以哈希

l = {'a': 1, 'b': 2}

new_ = {}

for k,v in l.items():

​	new_[k.upper()] = v

{k.upper():V for k,v in l.items()}



# 函数相关

1，作用域

global ：设置全局变量

nonlocal：修改enclosing变量

enclosing：非全局，非局部

闭包函数需要有三个条件，缺一不可：

​	1.必须有一个内嵌函数

​	2.内部函数引用外部函数变量

​	3.外部函数必须返回内嵌函数

闭包可用作装饰器

例子：

x = 1

def foo():

​	x = 10       该变量为enclosing变量

​	def bar():

​		x = 100

![image-20230110133205190](D:\工具\Typora\image\image-20230110133205190.png)

![image-20230110133220593](D:\工具\Typora\image\image-20230110133220593.png)

- 通过这个函数可以发现内层函数的a和外层函数无关，下层函数a相当于局部变量

改变一下情况：

![image-20230110133422825](D:\工具\Typora\image\image-20230110133422825.png)

![image-20230110133447259](D:\工具\Typora\image\image-20230110133447259.png)

- 这里下层函数没有声明局部变量，而是a+=10，相当于修改上层全局变量而上层变量也可看做enclosing变量，这是不允许的，可以在内层函数：

  1，nonlocal指定a

  2，或者先声明局部变量a都行

  3，global只能用在针对全局变量来用，在嵌套函数里无法指定上层而非全局变量使用，而且必须先声明才能用

  举个例子：

  ![image-20230110140508163](D:\工具\Typora\image\image-20230110140508163.png)

  ![image-20230110140525446](D:\工具\Typora\image\image-20230110140525446.png)

  这里global声明的是全局a，非上层变量，再改用nonlocal：

  ![image-20230110140641471](D:\工具\Typora\image\image-20230110140641471.png)

  ![image-20230110140652585](D:\工具\Typora\image\image-20230110140652585.png)

  

2，参数*args和**kwargs，提供函数扩展兼容功能

*args：可以传不定长的信息实参给其，默认可以空

**kwargs：可以传不定长的键值对参数，即字典

3，zip函数和sorted函数

zip函数：用于压缩，两个序列，Python为了节省空间，生成一个新的ZIp对象；可迭代，可用list()或dict()转换zip对象查看

生成的结果是多个元祖，按照一一对应关系生成，例如

![image-20230114125851442](D:\工具\Typora\image\image-20230114125851442.png)

运行结果：

![image-20230114125911798](D:\工具\Typora\image\image-20230114125911798.png)

其中。zip(*对象)，可以理解为解压返回一个二元矩阵

sorted函数：可变数据类型，可以用sort()来按照ascaii码进行原对象排序，sorted(序列)，返回一个新对象

重点：sorted可以自定义排序规则

sorted(iterable, key= , reverse=)

  - iterable: 可迭代对象（Python序列，文件）

  - key:指定排序参数或函数

      - 字典：sorted(dict.items(), lambda x: x[0], reverse = False)

        迭代字典里key和value，通过x[0]即key排序，升序排列

      - 元组列表l = [('Alex', 18),('alex', 20),('blex', 19)]

    def foo(item):

    ​		return item[0]

    sorted(l,key=foo)

    eg,按照升序，用item[0]对象排序，即对列表中元组的第一个值

    sorted传入的l，l的每一个迭代元素会作为item参传入foo函数，

      - 字典列表 l = [{'name': 'Alex', 'score': [98, 99 ,100]},{'name': 'alex', 'score': [97, 96 ,90]},{'name': 'blex', 'score': [80, 89 ,99]}]

    def foo(foo):

    ​		return item['score'] [2]

    sorted(l,key=foo)

    eg,升序，itemp['score'] [2]对象，即字典列表中的键score的值列表的第三个值为参数；实例，对学生的各科成绩排序

  - reverse: False升序和True逆序

# 是否产生新旧序列，永久性和暂时性

1，sort()函数是永久性的，会变更当前对象，并且返回当前对象

2，sorted()，是暂时性的，不会变更当前对象，并返回一个已排序新的序列

3，reverse()是永久性的，会改变对象本身