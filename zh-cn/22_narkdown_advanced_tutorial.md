# Markdown 高级使用教程

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    上一节我们带领大家讲解了 Markdown 基础使用教程，其实掌握了基础教程之后，足够我们应付大多数情况下的使用场景。尽管如此，但是对于前端大牛来说还远远不够，这个时候我们就需要使用 Markdown 的扩展语法了，所以本节我们来简单讲解一下 Markdown 一些常用的扩展语法
</section>

<br>

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #fff7d0; font-size: 10px;">
    <strong>注意</strong>
    <br><br>
    并非所有 Markdown 应用程序都支持扩展语法元素。您需要检查您的应用程序所使用的轻量级标记语言是否支持您要使用的扩展语法元素。如果没有，那么仍然有可能在 Markdown 处理器中启用扩展，本节我们以 Typora 作为 Markdown 编辑器来讲解
</section>

### 表格

#### 创建表格

要添加表，可以使用三个或多个连字符（`---`）创建每列的标题，并使用管道（`|`）分隔每列。

`Markdown` 代码如下：

```text
| 表头1 | 表头2 | 表头3 |
| --- | --- | --- |
| data10 | data20 | data30 |
| data11 | data21 | data31 |
```

显示效果如下：

| 表头1  | 表头2  | 表头3  |
| ------ | ------ | ------ |
| data10 | data20 | data30 |
| data11 | data21 | data31 |

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    如果您觉得通过代码直接创建表格比较麻烦，那么我们也可以通过，Markdown 编辑器右键直接插入表格，也能设置对齐方式
</section>

![](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/insert_table_typora.gif)

#### 对齐方式

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    我们还可以通过在标题行中的连字符的左侧，右侧或两侧添加冒号（ : ），将列中的文本对齐到左侧，右侧或中心
</section>

`Markdown` 代码如下：

```text
| 表头1 | 表头2 | 表头3 |
| :--- | :---: | ---: |
| data10 | data20 | data30 |
| data11 | data21 | data31 |
```

运行效果如下：

| 表头1  | 表头2  |  表头3 |
| :----- | :---: | -----: |
| data10 | data20 | data30 |
| data11 | data21 | data31 |

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #ffe6e6; font-size: 10px;">
    <strong>特别注意</strong>
    <br><br>
    我们可以在表格中设置文本格式。例如，您可以添加链接，代码（仅反引号（`）中的单词或短语，而不是代码块）和强调
    <br>
    我们不能添加标题，块引用，列表，水平规则，图像或 HTML 标签
</section>


### 代码块

#### 创建代码块

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    我们可以通过把行缩进四个空格或一个制表符来创建代码块，也可以通过在代码块的前后使用三个反引号（```）或者三个波浪线（~~~）来创建代码块
</section>

`Markdown` 代码如下：

````text
```
{
	"name": "Typora",
	"version": "beta"
}
```
````

运行效果：

```
{
	"name": "Typora",
	"version": "beta"
}
```

#### 语法高亮

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    Typora 处理器支持受围栏代码块的语法突出显示，我们只需要在代码块的前三个反引号后面写上语言即可
</section>

`Markdown` 代码如下：

````text
```json
{
	"name": "Typora",
	"version": "beta"
}
```
````

运行效果如下：

```json
{
	"name": "Typora",
	"version": "beta"
}
```

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #ffe6e6; font-size: 10px;">
    <strong>特别注意</strong>
    <br><br>
    不同的 Markdown 编辑器支持的代码高亮的语言种类不同，这边可以根据实际效果确认
    <br>
    Markdown 被渲染到浏览器上时，不同的浏览器所能支持的语言也不同，也需要根据实际效果确认
</section>

### 脚注

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    脚注使您可以添加注释和参考，而不会使文档正文混乱。当您创建脚注时，带有脚注的上标数字会出现在您添加脚注参考的位置。读者可以单击链接以跳至页面底部的脚注内容
</section>

#### 创建脚注

在方括号（`[^1]`）内添加插入符号和标识符。标识符可以是数字或单词，但不能包含空格或制表符。标识符仅将脚注参考与脚注本身相关联在输出中，脚注按顺序编号。在括号内使用另一个插入符号和数字添加脚注，并用冒号和文本（`[^1]:footnote`）。

`Markdown` 代码如下：

```text
脚注1[^1] 脚注2[^a]

[^1]: 这里是脚注 1 的解释

[^a]: 这里是脚注 2 的解释
```

运行效果：

脚注1[^1] 脚注2[^a]

[^1]: 这里是脚注 1 的解释

[^a]: 这里是脚注 2 的解释

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #ffe6e6; font-size: 10px;">
    <strong>特别注意</strong>
    <br><br>
    并非所有的地方都能放置脚注，除列表，块引号和表之类的其他元素之外的任何位置
</section>

### 任务列表

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    任务列表使您可以创建带有复选框的项目列表。在支持任务列表的 Markdown 应用程序中，复选框将显示在内容旁边。要创建任务列表，请在任务列表项之前添加破折号 - 和方括号 [ ]，并在 [ ] 前面加上空格。要选择一个复选框，请在方括号 [x] 之间添加 x 
</section>

`Markdown` 代码如下：

```text
- [x] Action 1
- [ ] Action 2
- [ ] Action 3
```

运行效果：

- [x] Action 1
- [ ] Action 2
- [ ] Action 3

### 删除线

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    我们可以通过在单词中心放置一条水平线来删除单词。此功能使您可以指示某些单词是一个错误，要从文档中删除。若要删除单词，请在单词前后使用两个波浪号 ~~
</section>

`Markdown` 代码：

```text
~~Hi Jeremy~~
```

运行效果：

~~Hi Jeremy~~

### Emoji 表情

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    我们可以在 Markdown 编辑时通过复制粘贴的方式添加表情，或者通过表情码添加表情（表情码的前后需要分别添加一个冒号）
</section>

`Markdown` 代码：

```text
:rocket:
```

运行效果：

:rocket:

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #ffe6e6; font-size: 10px;">
    <strong>特别注意</strong>
    <br><br>
    不同的 Markdown 编辑器支持的表情不同，这边可以根据实际效果确认
    <br>
    Markdown 被渲染到网站时，不同的浏览器、网站所能支持的表情也不同，也需要根据实际效果确认
</section>

我这边总结了一下几乎所有的 `emoji` 表情，放置在了 `GitHub` 仓库，大家可以按需下载看一下。

![github_emoji_list](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/github_emoji_list.gif)



`GitHub` 仓库：https://github.com/JeremyWu917/Github-Markdown-Emojis

### 高级语法

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    下面还有一些更加高级的语法，我这边就不详细展开了，大家可以根据需要自己学习一下
</section>

#### 公式

`Markdown` 语法：

```text
$$	
xxxxxxxxxx6 1\mathbf{V}_1 \times \mathbf{V}_2 =  \begin{vmatrix} 2\mathbf{i} & \mathbf{j} & \mathbf{k} \\3\frac{\partial X}{\partial u} &  \frac{\partial Y}{\partial u} & 0 \\4\frac{\partial X}{\partial v} &  \frac{\partial Y}{\partial v} & 0 \\5\end{vmatrix}6${$tep1}{\style{visibility:hidden}{(x+1)(x+1)}}
$$					
```

运行效果：

![](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/image-20220117170612648.png)

#### 横向流程图

`Markdown` 语法：

```text
graph LR
A[方形] -->B(圆角)
    B --> C{条件a}
    C -->|a=1| D[结果1]
    C -->|a=2| E[结果2]
    F[横向流程图]				
```

运行效果：

![](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/image-20220117170650702.png)

#### 竖向流程图

`Markdown` 语法：

```text
graph TD
A[方形] --> B(圆角)
    B --> C{条件a}
    C --> |a=1| D[结果1]
    C --> |a=2| E[结果2]
    F[竖向流程图]			
```

运行效果：

![](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/image-20220117170710727.png)

#### 标准流程图

`Markdown` 语法：

```text
st=>start: 开始框
op=>operation: 处理框
cond=>condition: 判断框(是或否?)
sub1=>subroutine: 子流程
io=>inputoutput: 输入输出框
e=>end: 结束框
st->op->cond
cond(yes)->io->e
cond(no)->sub1(right)->op		
```

运行效果：

![](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/image-20220117170732520.png)

#### 横向标准流程图

`Markdown` 语法：

```text
st=>start: 开始框
op=>operation: 处理框
cond=>condition: 判断框(是或否?)
sub1=>subroutine: 子流程
io=>inputoutput: 输入输出框
e=>end: 结束框
st(right)->op(right)->cond
cond(yes)->io(bottom)->e
cond(no)->sub1(right)->op			
```

运行效果：

![](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/image-20220117170751094.png)

#### `UML` 时序图 `1`

`Markdown` 语法：

```text
对象A->对象B: 对象B你好吗?（请求）
Note right of 对象B: 对象B的描述
Note left of 对象A: 对象A的描述(提示)
对象B-->对象A: 我很好(响应)
对象A->对象B: 你真的好吗？			
```

运行效果：

![](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/image-20220117170813473.png)

#### `UML` 时序图 `2`

`Markdown` 语法：

```text
Title: 标题：复杂使用
对象A->对象B: 对象B你好吗?（请求）
Note right of 对象B: 对象B的描述
Note left of 对象A: 对象A的描述(提示)
对象B-->对象A: 我很好(响应)
对象B->小三: 你好吗
小三-->>对象A: 对象B找我了
对象A->对象B: 你真的好吗？
Note over 小三,对象B: 我们是朋友
participant C
Note right of C: 没人陪我玩				
```

运行效果：

![](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/image-20220117170833009.png)

#### `UML` 标准时序图

`Markdown` 语法：

```text
%% 时序图例子,-> 直线，-->虚线，->>实线箭头
  sequenceDiagram
    participant 张三
    participant 李四
    张三->王五: 王五你好吗？
    loop 健康检查
        王五->王五: 与疾病战斗
    end
    Note right of 王五: 合理 食物 <br/>看医生...
    李四-->>张三: 很好!
    王五->李四: 你怎么样?
    李四-->王五: 很好!			
```

运行效果：

![](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/image-20220117170857937.png)

#### 甘特图

`Markdown` 语法：

```text
%% 语法示例
        gantt
        dateFormat  YYYY-MM-DD
        title 软件开发甘特图
        section 设计
        需求                      :done,    des1, 2014-01-06,2014-01-08
        原型                      :active,  des2, 2014-01-09, 3d
        UI设计                     :         des3, after des2, 5d
    未来任务                     :         des4, after des3, 5d
        section 开发
        学习准备理解需求                      :crit, done, 2014-01-06,24h
        设计框架                             :crit, done, after des2, 2d
        开发                                 :crit, active, 3d
        未来任务                              :crit, 5d
        耍                                   :2d
        section 测试
        功能测试                              :active, a1, after des3, 3d
        压力测试                               :after a1  , 20h
        测试报告                               : 48h			
```

运行效果：

![](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/image-20220117170921456.png)

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #ffe6e6; font-size: 10px;">
    <strong>特别注意</strong>
    <br><br>
    不同的 Markdown 编辑器支持的不同，这边可以根据实际效果确认
    <br>
    Markdown 被渲染到网站时，不同的浏览器、网站所能支持的也不同，也需要根据实际效果确认
</section>

### 自定义样式

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    您可以利用 HTML、CSS 在 Markdown 编辑器中实现各种各样的样式，我这边举个卡片的例子，旨在抛砖引玉
</section>

`Markdown` 语法：

```text
<section style="margin: auto; width:300px; box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2), 0 6px 20px 0 rgba(0,0,0,0.19); text-align: center; border-radius: 10px; box-shadow: 04px8px0rgba(0, 0, 0, 0.2), 06px20px0rgba(0, 0, 0, 0.19); box-shadow: 0 0 20px #a812ff;">
   <img style="width:20%; height:20%;" src="https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/tack_purple_128x128.png"/>
   <img src="https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/logo_05.png" style="width:100%; border: 1px solid #eee"/>
      <br>
      <img style="width:15%; height:15%; position: relative; top: 5px;left: 0px;" src="https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/tada.gif" />
      <section style="padding: 1px;">
         <p style="font-size:12px; color: #a812ff">长按关注公众号 ⌈全栈民工⌋ 🎉</p>
         <p style="font-size:12px; color: #a812ff">原创技术文章第一时间推送 🙌</p>
      </section>
</section>
```

运行效果：

![](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/image-20220117171700054.png)

### 最后

多看、多练、多总结 :rocket:

感谢阅读 :coffee:

