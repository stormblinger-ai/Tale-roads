### 1，xlwt创建写入excel

```
pip install xlwt
import xlwt
# 创建Excel,并实例化对象
wb = xlwt.Workbook()
# 选中工作簿,命名，实例化
sheet1 = wb.add_sheet('工作簿1')
# 按坐标值，填入表格
sheet1.write(0,0,'坐标（0，0）')
#保存对象，并存储命名
wb.save('excel1.xlsx')
```

### 2,xlrd读取excel内容
```
import xlrd
# 打开文件
wb = xlrd.open_workbook('excel1.xlsx')
# 用{wb.sheet_names},实例化文件对象课获得工作簿详情
print(f'excel中有{wb.nsheets}个工作簿')
print(f'excel中的sheets的名字:{wb.sheet_names()}')
# 实例化工作簿方法可以用名称或ID
sh1 = wb.sheet_by_index(0)
sh2 = wb.sheet_by_name('工作簿1')
print(sh1,sh2)
# 工作簿中总行和总列数是nrows和ncols
print(f'sheet里面一共有{sh1.nrows}行{sh1.ncols}列的数据')
print(f'第一行第一列的值是:{sh1.cell_value(0,0)}')
print(f'{sh1.col(0)[0].value}')
# value值，row行，col列，cell坐标
print(sh1.row(0))
print(sh1.row_values(0))
# 遍历数据
for r in range(sh1.nrows):
    for c in range(sh1.ncols):
        print(f'第{r}行，第{c}列数据是{sh1.cell_value(c,r)}')
```
### 3,xlutils修改表格
```
import xlrd
from xlutils.copy import copy
# 实例化目标文件
read_book = xlrd.open_workbook('excel1.xlsx')
# 复制目标文件
wb = copy(read_book)
# 获取目标文件工作簿
sh = wb.get_sheet(0)
# 在复制文件中添加内容
sh.write(0, 1, '1行2列')
sh.write(1, 0, '2行1列')
sh.write(1, 1, '2行2列')

sh2 = wb.add_sheet('汇总')
# 统计所有行或所有列
# count = ''
# rs = read_book.sheet_by_index(0)
# for i in range(1, rs.nrows):
#     num = rs.cell_value(i, 1)
#     # count.format(num)
#     count.join(str(num))
# print(rs.nrows)
# print(count)
# 写入内容
sh2.write(0, 0, '汇总')
# sh2.write(0, 1, count)
# 保存修改后对象
wb.save('excel1_change.xlsx')
```

### 4,xlwt设置样式
```angular2html
import xlwt

wb = xlwt.Workbook()
sh = wb.add_sheet('数据')

ft = xlwt.Font()
ft.name = '微软雅黑'
ft.colour_index = 2
ft.height = 11 * 20
ft.bold = True
ft.underline = True
ft.italic = True
# 对齐
alg = xlwt.Alignment()
alg.horz = 2  # 1左，2中，3右
alg.vert = 1  # 0上，1中，2下
# 行列高度大小，开启高度
sh.row(3).height_mismatch = True
sh.row(3).height = 10 * 256
sh.col(3).width = 20 * 256
# 设置边框
border = xlwt.Borders()
border.left = 1
border.right = 1
border.top = 1
border.bottom =1
# 边框颜色
border.left_colour =1
border.right_colour = 2
border.top_colour = 3
border.bottom_colour =4
# 背景颜色
pattern = xlwt.Pattern()
pattern.pattern = xlwt.Pattern.SOLID_PATTERN
pattern.pattern_fore_colour = 5

# 样式实例化
style = xlwt.XFStyle()
style.font = ft
style2 = xlwt.XFStyle()
style2.alignment = alg
style3 = xlwt.XFStyle()
style3.borders = border
style4 = xlwt.XFStyle()
style4.pattern = pattern
style5 = xlwt.easyxf('font: bold on,color_index 6;'
                     'align: vert center, horiz center'
                     )

sh.write(1, 1, '三国演义', style)
sh.write(2, 2, '红楼梦')
sh.write(3, 3, '水浒传', style2)
sh.write(3, 2, '西游记', style3)
sh.write(2, 3, '金瓶梅', style4)
sh.write(1, 3, '白姐', style5)
wb.save('04_xlwt样式.xlsx')

```
### 5,简单数据汇总
```
import xlrd
from xlutils.copy import copy


def read_data():
    wb = xlrd.open_workbook('../data/05_data统计.xlsx')
    sh = wb.sheet_by_index(0)
    # 分类字典{a:100}
    fen_type = {}
    # 单品价格
    count_price = []
    for r in range(sh.nrows):
        count = sh.cell_value(r, 3) * sh.cell_value(r, 4)
        count_price.append(count)
        key = sh.cell_value(r, 0)
        if fen_type.get(key):
            fen_type[key] += count
        else:
            fen_type[key] = count
    return fen_type, count_price


def save(fen, count):
    wb = xlrd.open_workbook('../data/05_data统计.xlsx')
    sh_t = wb.sheet_by_index(0)
    wb2 = copy(wb)
    sh = wb2.get_sheet(0)
    for r in range(sh_t.nrows):
        sh.write(r, sh_t.ncols, count[r])
    sh2 = wb2.add_sheet('汇总的数据')
    for i, d in enumerate(fen.keys()):
        sh2.write(i, 0, d)
        sh2.write(i, 1, fen.get(d))
    wb2.save('../data/05_汇总.xlsx')


if __name__ == '__main__':
    f, c = read_data()
    save(f, c)

```
### 6.表格拆分
```python
import xlrd
from xlutils.copy import copy


def get_data():
    wb = xlrd.open_workbook('../data/05_data统计.xlsx')
    sh = wb.sheet_by_index(0)
    all_data = {}
    for r in range(sh.nrows):
        d = {'type': sh.cell_value(r, 1), 'name': sh.cell_value(r, 2), 'count': sh.cell_value(r, 3),
             'price': sh.cell_value(r, 4)}
        key = sh.cell_value(r, 0)
        if all_data.get(key):
            all_data[key].append(d)
        else:
            all_data[key] = [d]
    return all_data


def save(data):
    wb = xlrd.open_workbook('../data/05_data统计.xlsx')
    wb2 = copy(wb)
    for key in data.keys():
        temp_sheet = wb2.add_sheet(key)
        for i, d in enumerate(data.get(key)):
            temp_sheet.write(i, 0, d.get('type'))
            temp_sheet.write(i, 1, d.get('name'))
            temp_sheet.write(i, 2, d.get('count'))
            temp_sheet.write(i, 3, d.get('price'))
    wb2.save('../data/06_表格拆分.xlsx')


if __name__ == '__main__':
    all_data = get_data()
    save(all_data)

```
### 7.openpyxl
```python
# pip install openpyxl

# import openpyxl
def open():
    from openpyxl import load_workbook
    wb = load_workbook('../data/05_data统计_bak.xlsx')
    # 获取工作簿
    sh1 = wb.active
    sh2 = wb['sheet1']
    sh3 = wb.get_sheet_by_name('sheet1')
    print(sh1 is sh2 is sh3)
    wb.close()

def show_sheet():
    from openpyxl import load_workbook
    wb = load_workbook('../data/06_表格拆分_bak.xlsx')
    print(wb.sheetnames)
    for sh in wb:
        print(sh.title)
    wb.close()
def get_one_value():
    from openpyxl import load_workbook
    wb = load_workbook('../data/06_表格拆分_bak.xlsx')
    sh1 = wb.active
    value1 = sh1.cell(2,3).value
    value2 = sh1['c2'].value
    print(value1,value2)
    wb.close()
def get_many_value():
    from openpyxl import load_workbook
    wb = load_workbook('../data/06_表格拆分_bak.xlsx')
    print(type(wb))
    sh1 = wb['工作簿1']
    print(type(sh1),':',sh1)
    # 切片
    cells1 = sh1['c2':'d3']
    print(cells1)
    # 获取整行整列
    cell_row3 = sh1[3]
    cell_col3 = sh1['c']
    print(cell_row3)
    print(cell_col3)
    cell_row3_5 = sh1[3:5]
    print(cell_row3_5)
    # 获取指定行或列使用迭代
    for row in sh1.iter_rows(min_row=2, max_row =5, max_col = 3):
        for cell in row:
            print(cell.value)
    wb.close()
def get_all_data():
    from openpyxl import load_workbook
    wb = load_workbook('../data/06_表格拆分_bak.xlsx')
    sh1 = wb['A']
    for row in sh1.rows:
        for cell in row:
            print(cell.value)
    for column in sh1.columns:
        for cell in column:
            print(cell.value)
def get_num():
    from openpyxl import load_workbook
    wb = load_workbook('../data/06_表格拆分_bak.xlsx')
    sh1 = wb['A']
    print(sh1.max_row)
    print(sh1.max_column)
if __name__ == '__main__':
    # open()
    # show_sheet()
    # get_one_value()
    # get_many_value()
    # get_all_data()
    get_num()
```
### 8.openpyxl创建excel
```python
def new():
    from openpyxl import Workbook
    wb = Workbook()
    sh1 = wb.active
    sh2 = wb.create_sheet('数据')
    sh3 = wb.create_sheet('人员', 0)
    wb.save('../data/08_openpyxl创建.xlsx')


def set_value():
    # 百度RGB颜色表设置colors
    from openpyxl.styles import Font, Alignment, colors
    bold_italic_30_font = Font(name='微软雅黑', size=30, italic=True, bold=True, color=colors.BLUE)
    bold_italic_20_font = Font(name='微软雅黑', size=20, italic=True, bold=True, color='BBFFFF')

    from openpyxl import Workbook
    wb = Workbook()
    sh1 = wb.active
    sh1['A1'] = 'Hello'
    sh1['B2'] = 'Excel'
    sh1['B2'].font = bold_italic_30_font
    sh1['C3'] = 'Excel'
    sh1['C3'].font = bold_italic_20_font
    wb.save('../data/08_openpyxl创建.xlsx')


def set_value2():
    # 百度RGB颜色表设置colors

    from openpyxl import Workbook
    wb = Workbook()
    sh1 = wb.active
    data = ['python', 'java', 'C++']
    for i, d in enumerate(data):
        sh1.cell(i + 1, 1).value = d
    wb.save('../data/08_openpyxl创建2.xlsx')


def set_value3():
    # 百度RGB颜色表设置colors
    from openpyxl.styles import Alignment

    from openpyxl import Workbook
    wb = Workbook()
    sh1 = wb.active
    sh1.row_dimensions[1].height = 30
    sh1.column_dimensions['A'].width = 30
    data = ['python', 'java', 'C++']
    for i, d in enumerate(data):
        sh1.cell(i + 1, 1).value = d
        sh1.cell(i + 1, 1).alignment = Alignment(horizontal='center', vertical='center')
    wb.save('../data/08_openpyxl创建3.xlsx')


def set_merge():
    from openpyxl import Workbook
    wb = Workbook()
    sh1 = wb.active
    sh1.merge_cells('A1:C1')  # 单元格合并顺序只能从小到大，从前到后，从上到下
    sh1.merge_cells('d2:e5')
    sh1['A1'] = '横向合并'  # 填值只能在单元第一个
    sh1['d2'] = '多合并'
    wb.save('../data/08_openpyxl创建4.xlsx')


def set_image():
    from datetime import date
    from openpyxl import Workbook
    from openpyxl.chart import LineChart, Reference

    wb = Workbook()
    # 激活worksheet
    ws = wb.active
    rows = [
        ['Date', 'Batch 1', 'Batch 2', 'Batch 3'],
        [date(2023, 12, 1), 40, 30, 25],
        [date(2023, 12, 2), 40, 25, 30],
        [date(2023, 12, 3), 50, 30, 45],
        [date(2023, 12, 4), 30, 25, 40],
        [date(2023, 12, 5), 25, 35, 30],
        [date(2023, 12, 6), 20, 40, 35],

    ]
    for row in rows:
        ws.append(row)
    c1 = LineChart()
    c1.title = 'LineChart'
    c1.x_axis.title = 'TestNumber'
    c1.y_axis.title = 'Size'
    # 从几行几列读取数据到几行几列，第一行是title
    data = Reference(ws, min_col=2, min_row=1, max_col=4, max_row=7)
    c1.add_data(data, titles_from_data=True)
    ws.add_chart(c1, 'A9')
    wb.save('../data/openpyxl_05画图.xlsx')


if __name__ == '__main__':
    # new()
    # set_value()
    # set_value2()
    # set_value3()
    # set_merge()
    set_image()

```
### 09.合并表格,多个excel合并一个excel中的一个sheet
```python
from openpyxl import load_workbook, Workbook
import os


# excel文档合并
def copy_data():
    wb = Workbook()
    sh = wb.active
    all_data = []

    for name in os.listdir('../data_bak'):
        path = f'../data_bak/{name}'
        tmp_wb = load_workbook(path)
        tmp_sh = tmp_wb.active
        for r in range(1, tmp_sh.max_row + 1):
            row_value = []
            for c in range(1, tmp_sh.max_column + 1):
                value = tmp_sh.cell(r, c).value
                row_value.append(value)

            all_data.append(row_value)
    for data in all_data:
        sh.append(data)
    wb.save('../data/09_openpyxl合并.xlsx')


if __name__ == '__main__':
    copy_data()

```
### 10.多个excel合并为一个excel中多个sheet
```python
from openpyxl import load_workbook, Workbook
import os


# excel文档合并
def copy_data():
    wb = Workbook()

    for name in os.listdir('../data_bak'):
        path = f'../data_bak/{name}'
        tmp_wb = load_workbook(path)
        tmp_sh = tmp_wb.active
        sh = wb.create_sheet(name[:-5])
        for r in range(1, tmp_sh.max_row + 1):
            row_value = []
            for c in range(1, tmp_sh.max_column + 1):
                value = tmp_sh.cell(r, c).value
                row_value.append(value)
            sh.append(row_value)
    del wb['Sheet']
    wb.save('../data/10_openpyxl合并多.xlsx')


if __name__ == '__main__':
    copy_data()

```
### 11.工资条生成
```python
from openpyxl import load_workbook, Workbook


def create_excel():
    wb = load_workbook('../data_bak/05_data统计_bak.xlsx',data_only=True) # data_only不考虑表达式，只考虑值
    sh = wb.active
    # 两种方案，先读第一条，后每次插入，或往列表里存
    title = ['工号', '部门', '姓名', '基本工资', '应发工资']
    for i, row in enumerate(sh.rows):
        if i == 0:
            continue
        else:
            temp_wb = Workbook()
            temp_sh = temp_wb.active
            temp_sh.append(title)
            # 列表推导式
            row_data = [cell.value for cell in row]
            temp_sh.append(row_data)
            temp_wb.save(f'../data/{row_data[2]}.xlsx')
if __name__ == '__main__':
    create_excel()

```
###12.隔行变色
```python
from datetime import date

from openpyxl import Workbook
from openpyxl.styles import PatternFill


def create_excel():
    wb = Workbook()
    sh = wb.active
    rows = [
        ['Date', 'Batch 1', 'Batch 2', 'Batch 3'],
        [date(2023, 12, 1), 40, 30, 25],
        [date(2023, 12, 2), 40, 25, 30],
        [date(2023, 12, 3), 50, 30, 45],
        [date(2023, 12, 4), 30, 25, 40],
        [date(2023, 12, 5), 25, 35, 30],
        [date(2023, 12, 6), 20, 40, 35],

    ]
    for row in rows:
        sh.append(row)
    # 修改样式,填充背景色
    bak_color = PatternFill('solid', fgColor='40E0D0')
    for i in range(1, sh.max_row + 1):
        if i % 2 == 0:
            for cell in range(1, sh.max_column + 1):
                sh.cell(i, cell).fill = bak_color
    wb.save(f'../data/openpyxl_变色.xlsx')


if __name__ == '__main__':
    create_excel()

```
### 13.统计加班时间
```python
from openpyxl import Workbook, load_workbook
from datetime import date


def create_data():
    wb = Workbook()
    sh = wb.active
    rows = [
        ['Date', '姓名', '打卡时间'],
        [date(2023, 12, 1), '吕布', '18:50'],
        [date(2023, 12, 2), '张飞', '18:10'],
        [date(2023, 12, 3), '刘备', '18:02'],
        [date(2023, 12, 4), '吕布', '18:50'],
        [date(2023, 12, 5), '关羽', '18:45'],
        [date(2023, 12, 6), '吕布', '18:50'],

    ]
    for row in rows:
        sh.append(row)
    wb.save('../data/打卡时间.xlsx')


def statistics():
    # 读取数据
    wb = load_workbook('../data/打卡时间.xlsx')
    sh = wb.active
    data = []
    for i in range(2, sh.max_row + 1):
        t_data = []
        for j in range(1, sh.max_column + 1):
            t_data.append(sh.cell(i, j).value)
        # 汇总
        h, m = t_data[2].split(':')
        full = int(h) * 60 + int(m)
        tmp = full - 18 * 60
        t_data.append(tmp)
        # 处理时间格式
        t_data[0] = t_data[0].date()
        data.append(t_data)
    # 保存
    n_wb = Workbook()
    n_sh = n_wb.active
    for d in data:
        n_sh.append(d)
    n_wb.save('../data/统计时间.xlsx')


if __name__ == '__main__':
    create_data()
    statistics()

```
### 14.数据去重
```python
from openpyxl import load_workbook
from openpyxl.styles import PatternFill


def dum():
    wb = load_workbook(f'../data/打卡时间.xlsx')
    sh = wb.active
    # 存储哪一行是重复数据
    index = []
    tmp = []  # 存储没有重复的数据
    for i, c in enumerate(sh['B']):
        if c.value not in tmp:
            tmp.append(c.value)
        else:
            index.append(i)
    fill = PatternFill('solid', fgColor='40E0D0')

    for i, r in enumerate(sh.rows):
        if i in index:
            for c in r:
                c.fill = fill
            print(f'第{i+1}行是重复数据')
    wb.save('../data/重复数据.xlsx')


if __name__ == '__main__':
    dum()

```
### 15.提取身份信息
```python
from openpyxl import load_workbook
from datetime import datetime


def create_time():
    now_year = datetime.now().year
    wb = load_workbook('../data_bak/09_身份信息3.xlsx')
    sh = wb.active
    max_column = sh.max_column
    for i, cell in enumerate(sh['B']):
        pno = cell.value
        year = pno[6:10]  # 区划6位，年4位，月日4位，个人识别码4位
        month = pno[10:12]
        day = pno[12:14]
        age = now_year - int(year)
        sh.cell(i + 1, max_column + 1).value = year
        sh.cell(i + 1, max_column + 2).value = month
        sh.cell(i + 1, max_column + 3).value = day
        sh.cell(i + 1, max_column + 4).value = age
    wb.save('../data/提取身份信息.xlsx')


if __name__ == '__main__':
    create_time()

```