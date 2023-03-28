# orm字段类型详细

自己平时看到啥，偶尔会查缺补漏，用整数而不用数值类型，是因为有很些类型是依靠数值实现的，不好细分

## 整数类型

```
在Django3.2的ORM中，常用的整数类型字段有：

1. IntegerField：用于存储整数，范围为 -2147483648 到 2147483647。

2. BigIntegerField：用于存储大整数，范围为 -9223372036854775808 到 9223372036854775807。

3. SmallIntegerField：用于存储较小的整数，范围为 -32768 到 32767。

4. PositiveIntegerField：用于存储正整数，范围为 0 到 2147483647。

5. PositiveSmallIntegerField：用于存储较小的正整数，范围为 0 到 32767。

6. AutoField：自动增长的整数类型，用于主键字段，范围为 1 到 2147483647。

7. BigAutoField：自动增长的大整数类型，用于主键字段，范围为 1 到 9223372036854775807。

8. SmallAutoField：自动增长的较小整数类型，用于主键字段，范围为 1 到 32767。

其中，浮点类型和十进制数字字段
1. FloatField：用于浮点数
2. DecimalField：用于金额精确树
```

# 字符串类型

```
Django3.2的ORM中常见的字符串类型字段有：

1. CharField：用于存储短字符串，最大长度为255个字符。

2. TextField：用于存储长文本，没有最大长度限制。

3. EmailField：用于存储电子邮件地址，最大长度为254个字符。

4. URLField：用于存储URL地址，最大长度为200个字符。

5. SlugField：用于存储URL友好的短标识符，最大长度为50个字符。

6. IPAddressField：用于存储IPv4地址。

7. GenericIPAddressField：用于存储IPv4或IPv6地址。

8. BinaryField：用于存储二进制数据，最大长度为255个字节。

9. UUIDField：用于存储UUID。

10. JSONField：用于存储JSON格式的数据。
```

## 日期时间类型

```
Django3.2的ORM中字段日期时间类型包括：

1. DateField：日期类型，格式为“YYYY-MM-DD”。

2. TimeField：时间类型，格式为“HH:MM:SS”。

3. DateTimeField：日期时间类型，格式为“YYYY-MM-DD HH:MM:SS”。

4. DurationField：持续时间类型，表示时间段。

5. TimeZoneField：时区类型，表示时区。

6. IntervalField：时间间隔类型，表示两个日期时间之间的差值。

7. YearField：年份类型，表示年份。

8. YearMonthField：年月类型，表示年月。

9. MonthField：月份类型，表示月份。

10. DayField：日期类型，表示日期。

11. TimezoneOffsetField：时区偏移量类型，表示时区偏移量。

12. TimestampField：时间戳类型，表示从1970年1月1日0时0分0秒开始的秒数。
```

## 关系类型

```
Django3.2的ORM中字段关系类型有以下几种：

1. 一对一关系（OneToOneField）
2. 一对多关系（ForeignKey）
3. 多对多关系（ManyToManyField）
4. 外键关系（GenericForeignKey）
5. 自引用关系（ForeignKey或OneToOneField）
```

## 布尔类型

```
Django3.2的ORM中字段布尔类型有BooleanField和NullBooleanField。 

BooleanField用于存储True或False，而NullBooleanField允许存储True、False或Null。 
```

## 文件类型

```
Django3.2的ORM中字段文件类型包括：

1. FileField：用于存储文件，包括上传的文件和本地文件路径。

2. ImageField：与FileField类似，但是只允许上传图片文件，并提供了一些额外的处理选项，如缩略图生成等。

3. FilePathField：用于存储文件路径，可以指定一个目录来限制可选的文件路径。

4. BinaryField：用于存储二进制数据，如图像或其他文件的字节流。

5. TextField：可以存储大量的文本数据，如HTML代码或其他格式的文本。
```

## 其他类型

