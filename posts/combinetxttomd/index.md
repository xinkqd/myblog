.. title: Python批量合并文本文件
.. slug: combinetxttomd
.. date: 2020-03-02 16:30:00 UTC+08:00
.. tags: python
.. category: python
.. link: 
.. description: 使用Python读取若干文本文档中的数据，并将数据汇总写入一个Markdown文档中。
.. type: text
.. status: published

这几天在Coursela（一个著名的MOOC平台）上报名参加了一门课程：Machine Design。

Machine Design是一门专业性很强的课程，主要讲述了诸如静力破坏及疲劳破坏的理论知识、常见机械结构的分析，机械系统设计等内容。

<!-- TEASER_END -->

看了一下课程整体的信息及课程安排，课程全英文授课，无任何中文资源。而且里面用到了非常多的机械类专业名词，这些词虽然在以前的工作和学习生活中有所了解，但是仅仅是中文层面，英文层面还是一片空白的状态。为了保证后面学习的流畅性，我将课程视频的字幕稿下载了下来并为自己做了一个计划：在开始每周的学习前先将预习字幕稿，了解接下来一周课程的主要内容，尤其是用到的机械相关专有名。

最终下载完成的第一周的字幕稿有11份之多。既然学习了`Python`，那就用`Python`编写一段程序，用来批量读取txt文件中的字幕稿并将其汇总后统一保存到一个`Markdown`文档中。这样便可以免去打开11次重复的打开txt文件并粘贴复制到`Markdown`文档的繁琐，可以提高转换效率。

<h1 data-tool="mdnice编辑器" style="margin-top: 40px; margin-bottom: 20px; font-weight: bold; color: black; border-bottom: 2px solid rgb(248,57,41); font-size: 1.3em;"><span style="display: inline-block; font-weight: normal; background: rgb(248,57,41); color: #ffffff; padding: 3px 10px 1px; border-top-right-radius: 3px; border-top-left-radius: 3px; margin-right: 3px;">一、所需材料</span></h1>

* 字幕稿文本文件若干
* Python开发环境

<h1 data-tool="mdnice编辑器" style="margin-top: 40px; margin-bottom: 20px; font-weight: bold; color: black; border-bottom: 2px solid rgb(248,57,41); font-size: 1.3em;"><span style="display: inline-block; font-weight: normal; background: rgb(248,57,41); color: #ffffff; padding: 3px 10px 1px; border-top-right-radius: 3px; border-top-left-radius: 3px; margin-right: 3px;">二、步骤</span></h1>

* 通过迭代依次打开字幕稿文本文档并读取文件内的内容

  

  ```
  # 迭代读取字幕稿文本文档中的内容
  for n in range(rangew):
  	f=open(fname,encoding='utf-8')
  	ftxt = f.read()
  	text.append(ftxt)
  ```

  

  其中：

  变量`fname`存放文件的文件名。

  txt文档中的内容通过Python提供的`file.read()`方法获取，并将获取到的文本内容赋值给`ftxt`。

  变量`text`为一个`List`，用于存储每个字幕稿文件中的内容。通过`List.append()`方法将`ftxt`的内容增加到`text`这个`List`中。

* 将获取的内容保存到新的`Markdown`文档中

  经过测试，Python可以直接打开并保存`Markdown`的文件：

  

  ```
  # 写入Markdown文档
  fmd = open('subtitle.md','w')
  for t in range(len(text)):
  	fmd.write(text[t])
  fmd.close()
  ```

  

  这里首先应用到了Python的`open()`函数：`open()`函数可以用于打开一个文件，创建一个`file`对象。

  `open()`函数有两个参数：一个是`文件名name`，即上面的`transcript.md`，另一个是模式`mode`，用于确定文件的打开方式，即上面的`w`。通过`open()`函数，程序创建并打开了一个名为`subtitle.md`的文档。

  之后通过遍历，将上面获取到的内容依次写入`Markdown`文档。

* 写入效果

  <p><center><img src="https://gitee.com/qianbi_tou/uploadpic/raw/master/20200302/20030201.png" width="40%"></center></p>

  <center><font size=2>图1 运行代码生成文档顶部</font></center>

  <p><center><img src="https://gitee.com/qianbi_tou/uploadpic/raw/master/20200302/20030202.png" width="40%"></center></p>

  <center><font size=2>图2 运行代码生成文档（两字幕稿之间）</font></center>
  
  总结一下，程序的功能算是完成了，但是汇总后的稿件看着好不舒服：
  
  <p><center><img src="https://gitee.com/qianbi_tou/uploadpic/raw/master/expression/unhappy.jpg" width="20%"></center></p>
  
  1. 生成的文件没有目录不方便快速定位不同字幕稿；
  2. 不同字幕稿之间没有明显的分界，不方便阅读和查找；
  3. 句子断句跟随视频，中间很多被换行符断开，阅读不便。

<h1 data-tool="mdnice编辑器" style="margin-top: 40px; margin-bottom: 20px; font-weight: bold; color: black; border-bottom: 2px solid rgb(248,57,41); font-size: 1.3em;"><span style="display: inline-block; font-weight: normal; background: rgb(248,57,41); color: #ffffff; padding: 3px 10px 1px; border-top-right-radius: 3px; border-top-left-radius: 3px; margin-right: 3px;">三、修改</span></h1>

分析一下，其实解决起来还是很简单的：

* `Markdown`文档中加入目录

`Markdown`中，要加入目录，只需要在文档正文前加入`[toc]`即可。

这里通过向文档中写入字幕稿前先写入`[toc]`的方式解决。

* 为文本增加标题

为了增加辨识度且便于在文档中阅读，需要为每段字幕稿在开始的位置加入标题。

这里通过为获取到的文本前和后增加对应字符串的方式解决。

* 查找并替换文档中的换行符`\n`。

这里选择通过使用`Python`的`replace()`方法将文本中的换行符`\n`替换掉，换成空格。

`replace()`方法需要输入2个参数，第一个参数即旧字符串（需要被替换掉的字符串），第二个参数即新字符串（需要的字符串）。

修改后的代码：



```
# 1 迭代读取字幕稿文本文档中的内容
for n in range(rangew):
	f=open(fname,encoding='utf-8')
	ftxt = f.read()
    
    # 替换换行符为空格
	chtxt = ftxt.replace('\n', ' ')
    
    # 为Markdown文档插入标题
	addfilename = '#Subtitle-' + str(n+1) + '\n' + chtxt 
	text.append(addfilename)
    
# 2 写入Markdown文档
fmd = open('transcript.md','w+')

# 写入目录
fmd.write('[toc]\n')

for t in range(len(text)):
	fmd.write(text[t])
    
    # 当前文档内容写入完成后加入换行符隔开两段内容
	fmd.write('\n')
fmd.close()
```



代码修改后的运行效果：

<p><center><img src="https://gitee.com/qianbi_tou/uploadpic/raw/master/20200302/20030205.png" width="40%"></center></p>

<center><font size=2>图3 修改代码后生成文档</font></center>

<p><center><img src="https://gitee.com/qianbi_tou/uploadpic/raw/master/20200302/20030204.png" width="40%"></center></p>

<center><font size=2>图4 修改代码后生成文档（两字幕稿之间）</font></center>

<h1 data-tool="mdnice编辑器" style="margin-top: 40px; margin-bottom: 20px; font-weight: bold; color: black; border-bottom: 2px solid rgb(248,57,41); font-size: 1.3em;"><span style="display: inline-block; font-weight: normal; background: rgb(248,57,41); color: #ffffff; padding: 3px 10px 1px; border-top-right-radius: 3px; border-top-left-radius: 3px; margin-right: 3px;">四、总结</span></h1>

对比代码修改前后生成的`Markdown`文档，代码修改后的文档颜值增加明显，十分便于阅读。

以后工作中再遇到需要进行文字稿的批量汇总，调整一下代码就可以嗖的一下完成，可以为自己节省出更多的时间给思考问题。

<p><center><img src="https://gitee.com/qianbi_tou/uploadpic/raw/master/expression/happy.gif" width="20%"></center></p>

****

<center><a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="知识共享许可协议" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br />本作品采用<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">知识共享署名-非商业性使用-相同方式共享 4.0 国际许可协议</a>进行许可。</center>



<p><center><img src="https://gitee.com/qianbi_tou/uploadpic/raw/master/20200229/qrcode.jpg" width="20%"></center></p>

<center>如果你有什么想要和我交流的内容，欢迎关注我的微信公众号并留言︿(￣︶￣)︿</center>