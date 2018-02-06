---
title: Emacs用法笔记
p: emacs/emacs_base_use
date: 2018-01-29 13:28:40
tags: Emacs
---

# Org-Mode

## 参考资料

* Org-mode手册:

   http://orgmode.org/org.html 

* 博文:
   http://www.cnblogs.com/qlwy/archive/2012/06/15/2551034.html

## 标签
  标签格式, :标签1:标签2:标签N:

## 排版

### 有序列表与无序列表

无序列表以'-'、'+'或者'*'开头

有序列表以'1.'或者'1)'开头

描述列表用'::'

 *注意事项*

列表符号后面都要有空格

同级别的列表缩进要相同

如果想要加入同级别的列表，可以 M-RET

空两行之后列表结束，如果空一行执行M-RET，实际上还是入同级项
    
    无序列表:
    - 列表项1 :: 说明
    - 列表项2 :: 说明

    有序列表
    1. 列表项1 :: 说明
    2. 列表项2 :: 说明


### 缩进

切换缩进模式　 org-indent-mode

如果想让某个文件默认使用缩进方式打开，可以在文件头部增加

````   
    #+STARTUP: indent
````

如果想要全局使用,可以在配置中设置


```,lisp
(setq org-startup-indented t)
```

## 链接
在一个链接上按`C-c C-o`即可访问，至于调用什么程序访问，取决于链接的内容，emacs和org mode的配置了。

### 自动连接
对于符合链接规则的内容，org-mode会自动将其视为链接，包括括文件、网页、邮箱、新闻组、BBDB 数据库项、 IRC 会话和记录等。下面是一些例子：

```，text
http://www.astro.uva.nl/~dominik            on the web
file:/home/dominik/images/jupiter.jpg       file, absolute path
/home/dominik/images/jupiter.jpg            same as above
file:papers/last.pdf                        file, relative path
file:projects.org                           another Org file
docview:papers/last.pdf::NNN                open file in doc-view mode at page NNN
id:B7423F4D-2E8A-471B-8810-C40F074717E9     Link to heading by ID
news:comp.emacs                             Usenet link
mailto:adent@galaxy.net                     Mail link
vm:folder                                   VM folder link
vm:folder#id                                VM message link
wl:folder#id                                WANDERLUST message link
mhe:folder#id                               MH-E message link
rmail:folder#id                             RMAIL message link
gnus:group#id                               Gnus article link
bbdb:R.*Stallman                            BBDB link (with regexp)
irc:/irc.com/#emacs/bob                     IRC link
info:org:External%20links                   Info node link (with encoded space)
```

对于文件链接，可以用::后面增加定位符的方式链接到文件的特定位置。定位符可以是行号或搜索选项。如：

```
file:~/code/main.c::255                     进入到 255 行
file:~/xx.org::My Target                    找到目标‘<<My Target>>’
file:~/xx.org/::p#my-custom-id               查找自定义 id 的项
```

### 手动连接
类似于
```,text
[[link][description]]
[[link]]
```

### 内部连接
内部链接就类似于HTML的锚点（实际上export成HTML文件后就变成了锚点），可以实现在一个文档内部的跳转。如下命令定义了一个名为target的跳转目标：


    n#<<target>> (这里我把锚点设置到*连接*这一部分开始处，大家可以点击下面效果中两个连接试试效果)
    如下方式可以设置到target的链接：
    [[target]] 或 [[target][猛击锚点]]


### 插入图片
插入图片直接使用相对链接，不能有description部分。
显示/隐藏图片显示命令：`org-display-inline-images`

使用相对链接 ,例如： `[[./your-images.png]]`


### 访问链接
快捷键 C-c C-o

### 插入/编辑链接
快捷键 C-c C-l  
命令 org-insert-link

## 嵌入元数据
### 内容元数据

org-mode中有以下几种

    s    #+begin_src ... #+end_src 
    e    #+begin_example ... #+end_example  : 单行的例子以冒号开头
    q    #+begin_quote ... #+end_quote      通常用于引用，与默认格式相比左右都会留出缩进
    v    #+begin_verse ... #+end_verse      默认内容不换行，需要留出空行才能换行
    c    #+begin_center ... #+end_center 
    l    #+begin_latex ... #+end_latex 
    L    #+latex: 
    h    #+begin_html ... #+end_html 
    H    #+html: 
    a    #+begin_ascii ... #+end_ascii 
    A    #+ascii: 
    i    #+index: line 
    I    #+include: line


**例如：** 代码 上面的单字母为快捷键字母，如输入一个<s 然后TAB后就变为：

    #+BEGIN_SRC 

    #+END_SRC

上面的代码我们还可以加入一些参数，如

    #+begin_src c -n -t -h 7 -w 40

    #+end_src

其中：
1. c为所添加的语言
2. -n 显示行号
3. -t 清除格式
4. -h 7 设置高度为7 -w 40设置宽度为40

### 注释 
以‘#‘开头的行被看作注释，不会被导出区块注释采用如下写法：

    #+BEGIN_COMMENT
    块注释
    ...
    #+END_COMMENT


### 表格与图片 
对于表格和图片，可以在前面增加标题和标签的说明，以方便交叉引用。比如在表格的前面添加：

    #+CAPTION: This is the caption for the next table (or link)

则在需要的地方可以通过

\ref{table1}
来引用该表格。

### 嵌入html 
对于导出html以及发布，嵌入html代码就很有用。比如下面的例子适用于格式化为cnblogs的代码块：

```,html
  <div class="cnblogs_Highlighter">
  <pre class="brush:cpp">
  int main()
  {
    return 0;
  }
  </pre>
  </div>
```

相当于在cnblogs的网页编辑器中插入"c++"代码。

### 包含文件 
当导出文档时，你可以包含其他文件中的内容。比如，想包含你的“.emacs”文件，你可以用：
`#+INCLUDE: "~/.emacs" src emacs-lisp `
可选的第二个第三个参数是组织方式（例如，“quote”，“example”，或者“src”），如果是 “src”，语言用来格式化内容。组织方式是可选的，如果不给出，文本会被当作 Org 模式的正常处理。用 C-c ,可以访问包含的文件。
