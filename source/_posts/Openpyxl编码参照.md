---
title: Openpyxl编码参照
date: 2020-08-24T12:11:30.000Z
updated: '2024-07-01 20:41:02'
categories:
  - Python
tags:
  - Python
  - Tools
---
```python
# coding:utf-8
import os
from unicodedata import name
from openpyxl import load_workbook
from openpyxl import Workbook
from openpyxl import NamedStyle, Font, Border, Side, PatternFill, colors, Alignment

filePath = r"C:\Users\yeji.HYRON-JS\Desktop\ExcelTestFile"
excelFileName = r"test11.xlsx"
excelFullFileName = os.path.join(filePath, excelFileName)


def openExistExecl():
    # 打开Excel文件（.xlsx）
    wb = load_workbook(
        r"C:\Users\20201104StringDeal.xlsx")
    # 所有sheet的list
    sheetList = wb.sheetnames
    sheetList[0].title = "sheet名"
    curSheet = wb["sheet名"]
    # 单元格
    curCell = curSheet(row=1, column=2)
    curCell.value = 3
    # 遍历所有
    # for row in curSheet.rows:
    #   for cell in row:
    #     print(cell.value)
    cell_range = curSheet['A1':'C2']
    
    # common style
    commonFont = Font(name="",           # 字体名称
                      size=11,           # 字号
                      bold=False,        # 粗体
                      italic=False,      # 斜体
                      vertAlign=None,    # 纵向对齐（‘superscript’, ‘baseline’, ‘subscript’)
                      underline='none',  # 下划线（‘’）
                      strike=False,      # 删除线
                      color='FF000000'   # 字体颜色
                     )
    commonFill = PatternFill(fill_type='solid',           # 指定填充的类型，支持的有：'solid'等。
                             start_color=colors.YELLOW,   # 指定填充的开始颜色
                             end_color=colors.YELLOW      # 指定填充的结束颜色
                            )
    border = Border(left=Side(border_style='thin', color='FF000000'),  # 左边框设置，Side类定义 边类型/颜色。'thin'/'thick'
                    # 右边框设置，Side类定义 边类型/颜色。'thin'/'thick'
                    right=Side(border_style='thin', color='FF000000'),
                    # 上边框设置，Side类定义 边类型/颜色。'thin'/'thick'
                    top=Side(border_style='thin', color='FF000000'),
                    # 下边框设置，Side类定义 边类型/颜色。'thin'/'thick'
                    bottom=Side(border_style='thin', color='FF000000'),
                    # 对角线边框设置，Side类定义 边类型/颜色。'thin'/'thick'
                    diagonal=Side(border_style=None, color='FF000000'),
                    diagonal_direction=0,  # 对角线方向
                    # 外边框线设置，Side类定义 变类型/颜色。'thin'/'thick'
                    outline=Side(border_style=None, color='FF000000'),
                    # 垂直线设置，Side类定义 变类型/颜色。'thin'/'thick'
                    vertical=Side(border_style=None, color='FF000000'),
                    # 水平线设置，Side类定义 变类型/颜色。'thin'/'thick'
                    horizontal=Side(border_style=None, color='FF000000'),
                    diagonalDown=False,
                    start=None,
                    end=None
                   )
    
    alignment = Alignment(horizontal='left',  # 水平对齐('centerContinuous', 'general', 'distributed','left', 'fill', 'center', 'justify', 'right')
                          vertical='center',  # 垂直对齐（'distributed', 'top', 'center', 'justify', 'bottom'）
                          text_rotation=0,  # 文字旋转
                          wrap_text=True,  # 自动换行
                          shrink_to_fit=False,  # 缩小字体填充
                          mergeCell=None,  # 合并单元格
                          indent=0  # 缩进
                         )
    commonStyle = NamedStyle(name="CommenHeader")
    
    # 样式_正文样式
    style_zhengwen = NamedStyle(name='style_zhengwen',
                                font=Font(),
                                fill=PatternFill(),
                                border=border,  # 所有边框
                                alignment=alignment,  # 水平居中，垂直左对齐，自动换行
                               )
    
    # 样式_标题行样式
    style_titleRow = NamedStyle(name='style_titleRow',
                                font=Font(b=True),  # 粗体
                                fill=PatternFill(fill_type='solid',  # 指定填充的类型，支持的有：'solid'等。
                                                 start_color='CCCCCC',  # 指定天聪的开始颜色
                                                 end_color='CCCCCC'  # 指定填充的结束颜色
                                                ),
                                border=border,  # 所有边框
                                alignment=Alignment(horizontal='center',  # 水平居中
                                                    vertical='center',  # 垂直居中
                                                    wrap_text=True,  # 自动换行
                                                   )
                               )
    # 样式_标题列样式
    style_titleCol = NamedStyle(name='style_titleCol',
                                font=Font(b=True),  # 粗体
                                fill=PatternFill(fill_type='solid',  # 指定填充的类型，支持的有：'solid'等。
                                                 start_color='CCCCCC',  # 指定天聪的开始颜色
                                                 end_color='CCCCCC'  # 指定填充的结束颜色
                                                ),
                                border=border,  # 所有边框
                                alignment=Alignment(horizontal='center',  # 水平居中
                                                    vertical='center',  # 垂直居中
                                                    wrap_text=True,  # 自动换行
                                                   )
                               )
    
    # 设置标题行样式
    curSheet.append(('A', 'B', 'C')) # 增加A B C 三列
    for row in curSheet['A1:C1']:  # 设置标题行样式
        for cell in row:
            cell.style = style_titleRow
            
            curSheet['F2'].font = commonFont
            #设置合并的单元格的所有边框
            for row in curSheet['F2:J5']:
                for cell in row:
                    cell.style = style_zhengwen
                    
                    #设置指定范围单元格的所有边框
                    for row in curSheet['A10':'D15']:
                        for cell in row:
                            cell.border = border
                            wb.save()
                            
                            def newExcel():
                                wb = Workbook()
                                sheet = wb.active
                                sheet.title = "test1"
                                sheet1 = wb.create_sheet("sheet名")
                                sheet.sheet_properties.tabColor = "ff0000"
                                sheet1.__format__
                                wb.save(r'C:\Users\yeji.HYRON-JS\Desktop\ExcelTestFile\test_1.xlsx')
                                
                                
                                if __name__ == "__main__":
                                    # 单元格
                                    newExcel()
                                    # 批量文件指定值替换
                                    #

```
