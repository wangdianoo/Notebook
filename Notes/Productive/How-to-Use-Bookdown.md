---
title: 如何使用bookdown编辑图书
date: 2017/10/10
tags: 
 - Markdown
 - Productive
categories:
 - Markdown
comments: true
---

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->
<!-- code_chunk_output -->

* [如何使用bookdown](#Header)
	* [什么是bookdown](#What)
	* [快速建立一个bookdown](#Setup)
	* [Markdown语法](#syntactic)
		* [基础Markdown语法](#Basic)
		* [bookdown扩充语法](#Expand)
			* [特殊标题 (Special Header)](#SHeader)
			* [引用](#reference)
	* [关于图片和表格](#关于图片和表格)

<!-- /code_chunk_output -->

# 如何使用bookdown {#Header}

## 什么是bookdown {#What}

像我这种轻度强迫症患者，在写作时总会不知觉的关注到排版(typoset)，稍微排版差点就很会影响到写作状态了。 我在大学参加数学建模比赛的时候接触过LaTex，除了蛋疼的图片浮动和表格插入外，它的排版效果是极佳的。 但是最终结果只能生成了PDF，就是一锤子买卖，毕竟我还需要一个word版本。 然而一入word深似海，所见即所得， 但是要自己去折腾标题，一点都不极客范。

## 快速建立一个bookdown {#Setup}

首先你需要保证RStudio IDE(version > 1.0.0)已经安装。并且也安装了`bookdown`

```r
install.packages("bookdown")
```

然后你需要以压缩包的形式下载[bookdown-demo](https://github.com/rstudio/bookdown-demo)，解压缩之后，修改文件夹名和`.Rproject`的文件名，双击就能在RStudio中打开。

`_bookdown.yml`， 以YAML格式设置全局输出。以`_`开始的文件名会被忽略。

![bookdown project](../../Pictures/bookdown.png)

随之就可用修改`_bookdown.yml`文件，从而使`bookdown::render_book()`改变渲染方式。

```YAML
# 合并后的markdown文件名
book_filename: bookdown
# clean_book()的输入参数
clean: [packages.bib, bookdown.bbl]
delete_merged_file: true
# RSutdio IDE输出
site: "bookdown::bookdown_site"
output:
  bookdown::gitbook:
    lib_dir: "book_assets"
# 国际化
language:
  lable:
    fig: "图"
    tab: "表"
  ui:
    edit: "编辑"
    chapter_name:  ["第 ", " 章"]
```

接着就是修改一系列章节相关的`.Rmd`文件。形同`01-intro.Rmd`，以开头的数字顺序确定不同文件的前后关系。

最后，通过build的Build Book生成全书。

![](../../Pictures/build_book.png)

## Markdown语法 {#syntactic}

### 基础Markdown语法 {#Basic}

最基本的 Markdown 语法基本用手指头都能数清，都不需要动用脚趾头。不过目前比较常用的标准是 **Pandoc** 所支持的语法。

第一类是**段落内格式**, 包括 _italic_ (`_斜体_`), **bold** (`**粗体**`), 字体~下标~(`字体~下标~`)和字体^上标^(字体^上标^)以及内联代码(`code`)。

还可以插入链接`[github](www.github.com)`, 插入图片也没有问题`![](path/to/image)`. 如果你对脚注有需求(`^[脚注]`)，就是这个效果^[脚注]。

第二类是**区块格式**，比如说不同级别的标题，有序列表和无序列表，引用， 代码块用三个反引号(`)开始和结束。

```markdown
# 一级标题

## 二级标题

### 三级标题

* one item
* one item
* one item
    - one item
    - one item
      + one item
      + one item

1. 第一
2. 第二
3. 第三

> 这是一个引用
```

第三类是数学公式。如果单独成行，就是用两个美元(\$)开始和结束。如果是行内的数学公式，就只要一个美元符合(\$),如 $\sum_a^b$。

当然你还可以像latex那样使用数学公式。

```equation
$$\begin{array}{ccc}
x_{11} & x_{12} & x_{13}\\
x_{21} & x_{22} & x_{23}
\end{array}$$
```

至于如何写数学公式，请自行百度Latex数学公式。

### bookdown扩充语法 {#Expand}

仅仅有markdown自带语法，顶多是写一篇文章，要想组织成一本书，还需要更多的扩展语法。

#### 特殊标题 (Special Header) {#SHeader}

**特殊标题**比较常用，比如说一本书的章节数较多，就可用将相关章节组成几个小部分(PART)， 还看可以插入图书附录(APPENDIX)

```markdown
# (PART) 第一部分 {-}

# 第一章

# (APPENDIX) 附录{-}
```

#### 引用 {#reference}

引用分为多种，包括但不限于如下类型：

- 公式引用
- 图表引用
- 文献引用
- 章节引用

先介绍一下**章节引用**。数字图书的一大优点在于能够快速跳转章节。Pandoc会自动为每一个标题都生成一个索引ID，当然也可用在标题后添加`{#id}`手动添加。

**图标引用**: 当代码块中增加量`fig.cap=foo`，那么就会被加入figure envirnoment.

```markdown
\```{r foo, fig.cap='(ref:foo)'}
plot(cars)  # a scatterplot
\```
```

之后就可用`\@ref(fig:foo)`或者`(ref:foo)`进行索引。

```rmarkdown
(ref:foo) Define a text reference **here**
\@ref(fig:foo) Define a text reference **here**
```

## 关于图片和表格

在我用LaTex排版的那段岁月中，我遇到的最蛋疼的问题有以下两个：

1. 表格不幸的排到一页中的后半部分，结果太长放不完，于是被分割成了两部分
1. 图片不幸的排到一页中的后半部分，结果太长放不下，于是只能另起一页

当然这两个问题在网页中是不存在的，因为一页可以很长很长。然而，一旦要导出成PDF，我就要担心它们的**浮动**问题了。

为了解决这个问题，对于R的作图结果，可以用`fig.width`,`fig.height`控制绘图的实际大小，用`out.width`, `out.height`控制相对大小。还可以用`fig.show='hold'`将一个输出的多个图片视作一个环境。

如果是外部图片，原生markdown的语法对于图片的调整是极其有限，毕竟它的设计哲学就是简单，Pandoc为其增加额外的语法，如`![](file.jpg){ width=50% }`。

但是都不如`knitr::include_graphics()`简单，可以根据输出自动调整图片导入方式；继承了之前所说的控制图片参数；还能机智地选择是否用pdf格式图片作为输入。

表格的处理方法也很简单粗暴，`knitr::kable()`搞定一切。`longtable`对应LaTex的`\usepackage{longtable}`，长表格就不再是问题了。