.. title: Excel表格数据转txt文本文档数据
.. slug: exceltotxt
.. date: 2020-02-24 19:59:00 UTC+08:00
.. tags: python, excel
.. category: python
.. link: 
.. description: 一种使用Python及其相关库编写的，用于将Excel指定表格数据转换成MdxBuilder可以识别的txt文本的代码。
.. type: text
.. status: published

<h1 data-tool="mdnice编辑器" style="margin-top: 40px; margin-bottom: 20px; font-weight: bold; color: black; border-bottom: 2px solid rgb(248,57,41); font-size: 1.3em;"><span style="display: inline-block; font-weight: normal; background: rgb(248,57,41); color: #ffffff; padding: 3px 10px 1px; border-top-right-radius: 3px; border-top-left-radius: 3px; margin-right: 3px;">一、背景</span></h1>

&emsp;&emsp;由于自己工作的关系，工作过程中会填写很多表格，在办公室使用电脑查看或者打印出来查看还好，一但在没有电脑的场合——车间、出差路上、外协厂等场所，只能使用手机查看Excel数据，这时的体验还是比较痛苦的，虽然手机可以横屏查看，但是相比于表格内容横屏的体验也算不上太好。

<!-- TEASER_END -->

&emsp;&emsp;前段时间在少数派上拜读了`hAppydOg`所写的《利用欧路词典查询产品信息》[^1]一文，感觉很有启发。引用文章中的一句话：

<blockquote data-tool="mdnice编辑器" style="display: block; font-size: 0.9em; overflow: auto; overflow-scrolling: touch; padding-top: 10px; padding-bottom: 10px; padding-left: 20px; padding-right: 10px; margin-bottom: 20px; margin-top: 20px; font-style: normal; border-left: none; padding: 10px; position: relative; line-height: 1.8; border-radius: 0px 0px 10px 10px; color: #FEEEED; background: #000; box-shadow: #84A1A8 0px 10px 15px;"><span style="display: inline; color: #FFF; font-size: 4em; font-family: Arial,serif; line-height: 1em; font-weight: 700;"> </span>
<p style="padding-top: 8px; padding-bottom: 8px; letter-spacing: 0.2em; word-spacing: 0.1em; margin: 0px; line-height: 26px; color: #FEEEED; font-size: 13px; display: inline;">表格中的产品信息本质上来说都是一组组词典，一个代码对应一条详细内容。既然是词典，用词典类 App 是最好的解决方案。</p>
<span style="float: right; display: inline; color: #FFF; font-size: 3em; line-height: 1em; font-weight: 500;">”</span></blockquote>

&emsp;&emsp;文章中提到的两个工具，`ExcelToTxt`和`MdxBuilder`。其中`ExcelToTxt`是一个将Excel表格中的数据转换成符合`Mdx`字典格式转换标准的txt文档的工具，而`MdxBuilder`则是将获取的txt文档转换为`Mdx`字典格式的工具。

&emsp;&emsp;最近在学习Python，Python 是一种易于学习又功能强大的编程语言。它提供了高效的高级数据结构，还能简单有效地面向对象编程[^2]。

&emsp;&emsp;苦于目前没有合适的机会测试下自己的学习成果，于是想着自己用Python写一段代码，用来实现和`ExcelToTxt`实现相同的功能。

<h1 data-tool="mdnice编辑器" style="margin-top: 40px; margin-bottom: 20px; font-weight: bold; color: black; border-bottom: 2px solid rgb(248,57,41); font-size: 1.3em;"><span style="display: inline-block; font-weight: normal; background: rgb(248,57,41); color: #ffffff; padding: 3px 10px 1px; border-top-right-radius: 3px; border-top-left-radius: 3px; margin-right: 3px;">二、准备</span></h1>

&emsp;&emsp;工欲善其事必先利其器。

&emsp;&emsp;要读写Excel文档，光有Python环境还不行，还需要有合适的库来支撑文件的读写。在Python众多第三方读写Excel文档库中，我最终选择了`OpenPyxl`这个第三方库。`OpenPyXL`是一个用于读写Excel2010及以上版本的`xlsx/xlsm/xltx`及`xltm`文件的第三方Python库[^3]。通过`OpenPyXL`库可以完成Excel的大多数操作。

<h1 data-tool="mdnice编辑器" style="margin-top: 40px; margin-bottom: 20px; font-weight: bold; color: black; border-bottom: 2px solid rgb(248,57,41); font-size: 1.3em;"><span style="display: inline-block; font-weight: normal; background: rgb(248,57,41); color: #ffffff; padding: 3px 10px 1px; border-top-right-radius: 3px; border-top-left-radius: 3px; margin-right: 3px;">三、思路</span></h1>
&emsp;&emsp;因为最终我只需要每个单元格中的值即可，在`OpenPyxl`中，我们可以通过`cell.value`属性，通过迭代单元格获取每个单元格的值：

```python
for row in rows:
    for cell in row:
        cellvalue = cell.value
```

&emsp;&emsp;通过这样的方法，可以分别获取Excel表格中的标题部分和正文部分的内容，然后再通过迭代将他们一一关联起来。

&emsp;&emsp;获取了标题和正文对应的数据，还需要将他们保存起来以便后续调用：基于`List`是有序的，可更改的且允许重复成员，我选择使用`List`来存储最终获得的数据。

&emsp;&emsp;最终将所需的数据依次写入txt文档。

<h1 data-tool="mdnice编辑器" style="margin-top: 40px; margin-bottom: 20px; font-weight: bold; color: black; border-bottom: 2px solid rgb(248,57,41); font-size: 1.3em;"><span style="display: inline-block; font-weight: normal; background: rgb(248,57,41); color: #ffffff; padding: 3px 10px 1px; border-top-right-radius: 3px; border-top-left-radius: 3px; margin-right: 3px;">四、实现步骤</span></h1>
1. 读取Excel表格数据

   * 读取标题

     在实现这部分功能时，考虑到后续会使用到标题行故将标题行单元格的值放到了`TotalTitleName`这个`List`中：

```python
     TotalTitleName = []
     for row in ws['A1':'I1']:
         for cell in row:
             TotalTitleName.append(cell.value)
```

   * 遍历表格数据

     &emsp;&emsp;要获取表格数据，和前文获取标题行数据类似，也是遍历所有单元格，只是这次遍历的是标题行外的行。

     &emsp;&emsp;遍历过程中首先将各行数据写入`List_CellValue`中，遍历完一行再将其值写入`List_AllCell_Value`中，其中`List_AllCell_Value`是一个包含所有行数据的二维`List`：

```python
     # 遍历单元格
     List_AllCell_Value = []
     for row in ws['A2':'I9']:
         if row[0].value != None: 
             List_CellValue = []
             for cell in row:
                 List_CellValue.append(cell.value)
             List_AllCell_Value.append(List_CellValue) 
```

   * 将标题与每行对应数据关联

     将获取到的标题和单元格内容进行关联，并将关联后的文件写入新的`List`中：
```python
     Info_Row_Value = []
     for x in range(0:8):
         Info_All_Row_Value = [] 
         for y in range(0:9):    
             NewCatlog = Titlename[y] +'：'+ Textvalue[x][y]
             Info_All_Row_Value.append(NewCatlog)
             Info_All_Row_Value[y] =Info_All_Row_Value[y] + '<p>' 
         Info_Row_Value.append(Info_All_Row_Value)
```


  &emsp;&emsp;如此便完成了写入txt文件中读取Excel表格部分的工作，接下来就是要将获得的`List`中的数据写入txt文档即可。

   * 写入txt文件
     这里采用直接将数据写入txt文件的方式：
```python 
   # 写入数据
   with open(fname,"w") as f:
       for x in range(0:8): 
           for y in range(0:9): 
           	f.writelines(InputListInfo[x][y]) 
   # 关闭txt文档
   f.close
```
***
&emsp;&emsp;因为写代码的依据是统计自己发表的各类知识产权的统计表：

<p><center><img src="https://gitee.com/qianbi_tou/uploadpic/raw/master/20200224/xlsx2txt1.png" width="80%"></center></p>
<br>

&emsp;&emsp;因此也就拿这个表格做了测试。程序运行后生成的txt文档：

<p><center><img src="https://gitee.com/qianbi_tou/uploadpic/raw/master/20200224/xlsx2txt2.png" width="80%"></center></p>
<br>

&emsp;&emsp;通过`MdxBuilder`生成的词典数据：

<p><center><img src="https://gitee.com/qianbi_tou/uploadpic/raw/master/20200224/xlsx2txt3.png" width="40%"></center></p>
<br>


&emsp;&emsp;最终经过对代码进行调整和优化，生成的`Mdx`字典词条如上图，已经能达到使用`ExcelToTxt`转换文件生成词典数据类似的效果。

<center><a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br />本作品采用<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议</a>进行许可。</center>

[^1]:《利用欧路词典查询产品信息》：https://sspai.com/post/39503
[^2]:Python 教程：
https://docs.python.org/zh-cn/3/tutorial/index.html

[^3]:OpenPyxl：https://openpyxl.readthedocs.io