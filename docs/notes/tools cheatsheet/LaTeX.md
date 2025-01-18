# LaTeX 使用备忘录
!!! warning
    只是依据个人使用习惯整理的，以及我只是初学者。如果有错误或者不妥的地方，欢迎指正。
    ??? advice
        我还没有尝试过typst.

## 基本结构
1. **文档类声明**：指定文档的类型，例如article（文章）、report（报告）、book（书籍）等。使用`\documentclass{ }`命令来声明文档类。

2. **导言区**：在`\begin{document}`命令之前的部分，用于加载宏包、设置文档参数、定义命令等。通常包括`\usepackage{}`命令和一些全局设置。

3.**标题信息**：使用`\title{}`、`\author{}`、`\date{}`等命令来设置文档的标题、作者和日期等信息。然后使用\maketitle命令生成标题。

3. **正文**：文档的主要内容部分，包括段落、章节、表格、图片、公式等。

4. **结尾**：使用`\end{document}`命令表示文档的结束。

在文档的正文部分，常见的结构包括：

+ **章节**：使用`\section{}`、`\subsection{}`、`\subsubsection{}`等命令来定义章节结构。

+ **段落**：文档中的基本文本单元，通过空行或`\par`命令分隔。

+ **列表**：包括有序列表（`\begin{enumerate}`）、无序列表（`\begin{itemize}`）和描述列表（`\begin{description}`）等。

+ **表格**：使用table环境和tabular环境来创建表格。

+ **图片**：使用figure环境和`\includegraphics{}`命令来插入图片。

+ **公式**：使用equation环境、align环境或者`\[ ... \]`等命令来插入数学公式。

## 一些宏包
```latex
\usepackage{ctex} % 中文支持
\usepackage{amsmath} % 数学公式
\usepackage{amssymb} % 数学符号
\usepackage{amsthm} % 定理环境
\usepackage{amsfonts} % 数学字体
\usepackage{graphicx} % 插入图片
\usepackage{hyperref} % 超链接
\usepackage{listings} % 代码块
\usepackage{color} % 颜色
\usepackage{xcolor} % 颜色，是前者的扩展
\usepackage{algorithm} % 算法块
\usepackage{algpseudocode} % 伪代码
```

## 字体字号
### 字体族设置
LaTeX中有三种基本的字体族：

+ \textrm{} 或 \rmfamily：罗马字体，用于正文文本。
+ \textsf{} 或 \sffamily：无衬线字体，用于强调文本。
+ \texttt{} 或 \ttfamily：等宽（打字机）字体，用于代码或预格式化文本。

### 字体系列设置
+ \textbf{} 或 \bfseries：加粗字体，用于强调重点。
+ \textmd{} 或 \mdseries：中等字重，通常是文本的默认设置。
字体形状
+ \textit{} 或 \itshape：斜体字，用于强调或书名等。
+ \textsl{} 或 \slshape：倾斜字体，与斜体类似但稍有区别。
+ \textsc{} 或 \scshape：小型大写字体，用于某些标题等。

### 字号设置
LaTeX提供了多种标准字号命令：

+ \tiny
+ \scriptsize
+ \footnotesize
+ \small
+ \normalsize
+ \large
+ \Large（较大）
+ \LARGE
+ \huge
+ \Huge（最大）

??? eg
    ```latex
        \documentclass[12pt]{article}
        \begin{document}

        % 字体族
        这是罗马字体：\textrm{Roman family.}

        这是无衬线字体：\textsf{Sans Serif family.}

        这是等宽字体：\texttt{Typewriter family.}

        % 字体系列
        这是加粗字体：\textbf{Boldface series.}

        % 字体形状
        这是斜体字：\textit{Italic shape.}

        这是小型大写字体：\textsc{Small Caps shape.}

        % 字号
        这是小字号文本：{\small The quick brown fox jumps over the lazy dog.}

        这是大字号文本：{\Large The quick brown fox jumps over the lazy dog.}

        \end{document}
    ```

上述这些都可以嵌套，例如我想要加粗斜体，我可以
`\textbf{\textit{Your text here}}`

## 列表
markdown里无序列表用惯了，感觉LaTeX多少有些不便。

## 插入图片

## 其他
标题换行
``` latex
\section{This is a very long title that needs to be \\ split into two lines}
```
