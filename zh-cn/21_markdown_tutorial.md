# Markdown 基础使用教程

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    作为程序员，如果你不清楚 Markdown 估计没人敢相信，毫不夸张地说，Markdown 是目前世界上最受欢迎的标记语言之一，所以今天我就带领大家初步了解一下 Markdown 的魅力，带你快速上手 Markdown
</section>

### 什么是 Markdown

`Markdown` 是一种轻量级标记语言，它允许人们使用易读易写的文本格式编写文档，`Markdown` 文件的后缀名为 `.md`，有如下显著特点：

1. 专注于文字内容
2. 纯文本，易读易写，可以方便地纳入版本控制
3. 语法简单，没有什么学习成本，能轻松在码字的同时做出美观大方的排版

### 为什么要使用 Markdown

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #fff7d0; font-size: 10px;">
    很多小伙伴会有疑问，为什么要使用 Markdown 呢？其实，就当下互联网现状来看 Markdown 有如下 4 条显著特点 
</section>

1. `Markdown` 无处不在。
2. `Markdown` 是纯文本可移植的。
3. `Markdown` 是独立于平台的。
4. `Markdown` 能适应未来的变化。

### 常用的 Markdown 编辑器

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    这里我结合自身的使用经验，推荐大家使用 Typora 编辑工具，或者使用 VS Code 安装 Markdown All in One 插件后使用。
</section>

![image-20220117092353525](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/image-20220117092353525.png)

![image-20220117092420634](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/image-20220117092420634.png)

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #fff7d0; font-size: 10px;">
    还是那句话，作为学习氪金就没有必要了，Typora 使用 beta 版本学习足够了
</section>

`Typora` 的优点不用多说，支持各种定制化，只要你的前端技能足够牛，你可以按照需要开发各种各样的主题，`Typora` 都能够很好的渲染出来。

`VS Code` 更不用多说，宇宙最强编辑器之一。推荐在 `VS Code` 中编辑 `Markdown` 的原因有两个：

1. 不用再安装别其余的应用
2. 更好的在 `Gitee` 或者 `GitHub` 中渲染，因为一些网站或者开发平台为了安全考虑不会支持太多的渲染。

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #ffe6e6; font-size: 10px;">
    也就是说，不同的 Markdown 编辑器都有不同的特性，能够支持的渲染也不同
</section>

### 基础语法简介

#### 标题

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    要创建标题，只需要在语句前面添加 <code>#</code> 即可，<code>#</code> 的数量代表了标题的级别。例如，添加四个 <code>#</code>，表示创建一个四级标题（例如 <code>#### Header></code>）
</section>

| Markdown 语法     | HTML                |
| ----------------- | ------------------- |
| `# Header 1`      | `<h1>Header 1</h1>` |
| `## Header 2`     | `<h2>Header 2</h2>` |
| `### Header 3`    | `<h3>Header 3</h3>` |
| `#### Header 4`   | `<h4>Header 4</h4>` |
| `##### Header 5`  | `<h5>Header 5</h5>` |
| `###### Header 6` | `<h6>Header 6</h6>` |

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #fff7d0; font-size: 10px;">
    <strong>注意</strong>
    <br>
    还可以在文本下方添加任意数量的 == 号来标识一级标题，或者 -- 号来标识二级标题
    <br>一般情况下，我们会考虑在 # 和标题之间添加一个空格
</section>

#### 段落

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    要创建段落，只需要用空白行将一行或多行文本进行分隔即可
</section>

| Markdown 语法                                | HTML                                                       |
| -------------------------------------------- | ---------------------------------------------------------- |
| `Hello world`  <br>`Coding change the world` | `<p>Hello world</p>` <br> `<p>Coding change the world</p>` |

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #fff7d0; font-size: 10px;">
    <strong>注意</strong>
    <br>
    在编辑 Markdown 时，一般不建议使用空格（spaces）或制表符（ tabs）缩进段落
</section>

#### 换行

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    要换行，一般使用 <code>br</code> 即可
</section>

#### 强调

1. 加粗，请在单词或短语的前后各添加**两个**星号（`asterisks`）或下划线（`underscores`）
2. 斜体，要用斜体显示文本，请在单词或短语前后添加**一个**星号（`asterisk`）或下划线（`underscore`）
3. 粗体和斜体同时使用，要同时用粗体和斜体突出显示文本，请在单词或短语的前后各添加**三个**星号或下划线

#### 引用

1. 要创建块引用，请在段落前添加一个 `>` 符号
2. 块引用可以嵌套。在要嵌套的段落前添加一个 `>>` 符号

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #fff7d0; font-size: 10px;">
    <strong>注意</strong>
    <br>
    块引用可以包含其他 Markdown 格式的元素。并非所有元素都可以使用，你需要进行实验以查看哪些元素有效
</section>

#### 列表

1. 有序列表，在每个列表项前添加数字并紧跟一个英文句点。数字不必按数学顺序排列，但是列表应当以数字 `1` 起始
2. 无序列表，在每个列表项前面添加破折号 (`-`)、星号 (`*`) 或加号 (`+`) 。缩进一个或多个列表项可创建嵌套列表

#### 代码

1. 行内代码，要将单词或短语表示为代码，请将其包裹在反引号 (\``) 中
2. 转义反引号，要表示为代码的单词或短语中包含一个或多个反引号，则可以通过将单词或短语包裹在双反引号(\``)中
3. 代码块，要创建代码块，请将代码块的每一行缩进至少四个空格或一个制表符，或者将其包裹在三个反引号 (\```) 中

#### 分割线

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    要创建分隔线，请在单独一行上使用三个或多个星号 (***)、破折号 (---) 或下划线 (___) ，并且不能包含其他内容
</section>

#### 链接

链接文本放在中括号内，链接地址放在后面的括号中，链接title可选

超链接 Markdown 语法代码：`[超链接显示名](超链接地址 "超链接 title")`

#### 图片

要添加图像，请使用感叹号 (`!`), 然后在方括号增加替代文本，图片链接放在圆括号里，括号里的链接后可以增加一个可选的图片标题文本

插入图片 Markdown 语法代码：`![图片alt](图片链接 "图片 title")`

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #fff7d0; font-size: 10px;">
    <strong>注意</strong>
    <br>
    如果要给图片增加链接，请将图像的 Markdown 括在方括号中，然后将链接添加在圆括号中
</section>

#### 转义字符语

要显示原本用于格式化 `Markdown` 文档的字符，请在字符前面添加反斜杠字符 `\`

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #ffe6e6; font-size: 10px;">
    需要特别注意的是，在 Markdown 的块级元素和内联元素中， < 和 & 两个符号都会被自动转换成 HTML 实体，这项特性让你可以很容易地用 Markdown 写 HTML
</section>

#### 内嵌 HTML 标签

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    对于 Markdown 涵盖范围之外的标签，都可以直接在文件里面用 HTML 本身。如需使用 HTML，不需要额外标注这是 HTML 或是 Markdown，只需 HTML 标签添加到 Markdown 文本中即可
</section>

### 最后

多看，多练，多总结！



感谢阅读 :coffee: