---
Date: 2021-09-21
Draft: false
Categories:
  - Tech
Title: 如何使用python处理excel文件？
---

[Working with Excel Files in Python](http://www.python-excel.org/)

# 1 哪些包可以优雅的处理 excel？

在对 excel 进行一些操作时，可以使用 python 进行处理。常见的操作有增加、删除、修改等，经过一段时间的使用分析，确定比较好用的有以下几个，各有各的特点以及注意事项，以此记录。

- `xlrd`
- `xlwt`
- `xlutils`
- `openpyxl`
- `pandas`

下面一一介绍。

# 2 `xlrd` & `xlwt`

[xlrd - xlrd 2.0.1 documentation](https://xlrd.readthedocs.io/en/latest/)

**最多处理 6 万多行**

`xlrd` 主要用于读取 excel 文件，其主要针对于 `xls` 格式的 excel 文件，但是实际使用过程中是可以读取 `xlsx` 格式文件的。

`xlwt` 主要用于写入 excel 文件，其只能将文件保存为 `xls` 格式，而不能保存为 `xlsx` 格式。这两个包都主要是针对于老版本（office 2003）的 excel处理。

**常见操作**以及示例代码如下：

```python
# xlrd
workbook = xlrd.open_workbook(filepath)   # 读取文件
sheets = workbook.sheet_names()           # 读取excel文件中所有的表单名字
worksheet = workbook.sheet_by_name(sheets[0])   # 使用第一个表单sheet

# 读取内容
worksheet.cell_value(row,col)  # 读取第row行第col列的单元格的值，**注意，row和col是从0起的**
worksheet.row_values(row)      # 取出第row+1行的数据
worksheet.col_values(row)      # 取出第row+1行的数据
```

```python
# xlwt
workbook = xlwt.Workbook(encoding = 'utf-8')  # 生成excel文件格式的对象
worksheet = workbook.add_sheet('result')      # 在此对象内创建名字为result的sheet
worksheet.write(row,col,value)                # 将value写入row行col列，**注意，row和col是从0起的**
```

# 3 xlutils

这个包是前面两个包的一个封装，包含了前面两个包所需的所有的依赖，能进行相应的操作。所以在使用时，可能会需要前面两个模块创建的对象，主要用来执行 copy 操作。执行完 copy 操作之后，将会获得原 excel 一样的内容和表格，结合 `xlwt` 一起，可以实现对表格的修改。

示例代码如下：

```python
from xlutils.copy import copy
old_excel = xlrd.open_workbook(filepath)
new_excel = copy(old_excel)
```

# 4 openpyxl

**最多可以处理几百万行**

官方推荐使用的工具，操作与前文的类似，但是也有缺点。可以直接使用这个包进行增删改查等操作。**默认下标是从 1 开始**

示例代码如下：

```python
# 打开excel
workbook = openpyxl.load_workbook(filepath)
sheets = workbook.sheetnames
worksheet = workbook[sheets[0]]

# 创建表格
wb.create_sheet(0) # 在第一个位置创建表格
wb.create_sheet()  # 在末尾创建表格

# 读取excel
maxrow = worksheet.max_row  # 最大行数
maxcol = worksheet.max_col  # 最大列数
value = worksheet.cell(row,col).value   # 读取单元格的值

rows = worksheet.iter_rows(min_row, max_col, max_row)  # 按行读取
rows = worksheet.iter_cols(min_row, max_col, max_row)  # 按列读取

# 写入excel文件
worksheet.cell(row,col).value = value  # 直接赋值
worksheet.append([a,b,c])  # 按行写入

# 保存文件
workbook.save("filename")
```

# 5 Pandas

数据分析库，使用不多，待补充