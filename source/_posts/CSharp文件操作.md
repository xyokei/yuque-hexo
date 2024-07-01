---
title: CSharp文件操作
date: 2022-12-16T12:12:57.000Z
updated: '2024-07-01 21:57:25'
categories:
  - C#
tags:
  - C#
  - Tools
---
C# 文件相关
<!-- more -->

## 查找含有指定字符的文件名 和 删除指定目录
```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Collections;
using System.Text.RegularExpressions;

namespace ConsoleApplication1
{
    class Program
    {
        static ArrayList targetFile = new ArrayList();
        static ArrayList allFile = new ArrayList();

        static ArrayList targetDir = new ArrayList();

        /// <summary>
        /// 目录递归遍历
        /// </summary>
        /// <param name="path"></param>
        static void listDirectory(string path)
        {
            //当前文件目录
            DirectoryInfo theFolder = new DirectoryInfo(@path);

            //遍历当前文件目录下所有文件
            foreach (FileInfo NextFile in theFolder.GetFiles())
            {

                //匹配文件名中 包含test的 Excel文件
                if (Regex.IsMatch(NextFile.Name, @".*test.*\.xlsx$"))
                {
                    targetFile.Add(path + "\\" + NextFile.Name);
                }
                allFile.Add(path + "\\" + NextFile.Name);
            }

            //递归遍历当前文件目录下所有文件夹
            foreach (DirectoryInfo NextFolder in theFolder.GetDirectories())
            {
                //匹配所有以test结尾的文件夹 并保存
                if (Regex.IsMatch(NextFolder.ToString(), @"test$"))
                {
                    targetDir.Add(theFolder + "\\" + NextFolder);
                }
                //递归遍历目录
                listDirectory(NextFolder.FullName);
            }

        }
        /// <summary>
        /// 递归删除指定目录
        /// </summary>
        /// <param name="dir"></param>
        static void DeleteFolder(string dir)
        {
            if (Directory.Exists(dir)) //如果存在这个文件夹删除之 
            {
                foreach (string d in Directory.GetFileSystemEntries(dir))
                {
                    if (File.Exists(d))
                        File.Delete(d); //直接删除其中的文件                        
                    else
                        DeleteFolder(d); //递归删除子文件夹 
                }
                Directory.Delete(dir, true); //删除已空文件夹                 
            }
        }
        static void Main(string[] args)
        {
            //最后一个不用加\
            listDirectory("C\\test\\test\\tsss");
            Console.Write("控制台不换行输出");
            Console.WriteLine("控制台换行输出");
            Console.WriteLine("over end");
            Console.Read();//等待用户输入单个字符，可做阻塞 相当于pause
            Console.ReadLine();//等待用户输入，回车结束。可做阻塞 相当于pause
        }
    }
}
```

## 文件输出处理
A：excel输出 此处用spreadgear
```csharp
public byte[] excelOutput(){
    string filePath = @"tempFile.xlsx";
    System.IO.File.Create(filePath).Close();
    
    //文件输出流
    FileStream fs = new FileStream(filePath, FileMode.Open);
    IWorkbookSet workbookSet = Factory.GetWorkbookSet();
    IWorkbook workbook = workbookSet.Workbooks.OpenFromStream(fs);
    fs.Close();

    //sheet集合
    List<IWorksheet> worksheets = new List<IWorksheet>();
    worksheets.Add(workbook.Worksheets[0]);
    //添加sheet
    worksheets.Add(workbook.Worksheets.Add());

    //创建sheet
    
    createSheet(workbook[0]);
    createSheet(workbook[1]);

    //打开默认选中第一个
    workbook.Worksheets[0].Select();

    workbook.SaveAs(filePath, FileFormat.OpenXMLWorkbook);

    //实施读取文件已写入内容，判断写入时长
    FileStream tempFs = new FileStream(filePath, FileMode.Open, FileAccess.Read);
    byte[] data = new byte[tempFs.Length];
    tempFs.Read(data, 0, (int)tempFs.Length);
    tempFs.Close();

    //Tempファイル削除
    if (System.IO.File.Exists(filePath))
    {
        System.IO.File.Delete(filePath);
    }

    return data;
            
}
private  createSheet(IworkSheet worksheet, String sheetName){
    worksheet.Name = sheetName;

    IRange cells = worksheet.Cells;

    //调整字体
    worksheet.Range.Font.Name = "meiryo UI";

    String[,] data = xxxService.getData();

    int row = 2;
    //行遍历
    for(int i = 0;i< data.GetLength(0); i++){
        //列遍历
        for(int j = 0;i < data.GetLength(1); j++){
            //单元格设定
            setDetailCell(cells[row,j], data[i, j]);
        }
        row++;
    }
}

public static void setDetailCell(IRange cell, String text)
{
    //内容
    cell.Value = text;
    //字体颜色
    cell.Font.Color = SpreadsheetGear.Color.FromArgb(0, 0, 0);
    //格子
    cell.Borders.Color = SpreadsheetGear.Color.FromArgb(122, 122, 122);
    cell.HorizontalAlignment = HAlign.Left;
    //缩进
    cell.IndentLevel = indentLevel;
    //回车
    cell.WrapText = true;
}
```

## 文件读取
```csharp
using System.Reflection;
using System.IO;

/// <summary>
/// 文件读取
/// </summary>
/// <param name="Path">路径</param>
/// <param name="readFile">读取内容</param>
/// <returns></returns>
public static Result GetFileFromResouce(string Path,ref string readFile) 
        {
            Result rs = new Result();
            StreamReader srReadFile = null;
            try
            {
                StringBuilder sql = new StringBuilder();
                Assembly assm = Assembly.GetExecutingAssembly();
                Stream istr = assm.GetManifestResourceStream(Path);
                srReadFile = new StreamReader(istr, System.Text.Encoding.GetEncoding("shift-jis"));
                while (!srReadFile.EndOfStream)
                {
                    sql.Append(srReadFile.ReadToEnd());
                }
                readFile = sql.ToString();

            }
            catch (Exception ex)
            {
                rs.ErrCode = 1;
                rs.ErrMsg = ex.Message.ToString();
            }
            finally 
            {
                if (srReadFile != null) { srReadFile.Close(); }
            }

            return rs;
        }
```
