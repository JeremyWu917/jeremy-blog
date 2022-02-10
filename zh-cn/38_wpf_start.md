# WPF 基础系列教程 - 认识 WPF

![](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/image-20220210094330890.png)

<br>

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    WPF(Windows Presentation Foundation) 是创建桌面客户端应用程序的 UI 框架 
    <br>
    WPF 开发平台支持广泛的应用开发功能，包括应用模型、资源、控件、图形、布局、数据绑定、文档和安全性
    <br>
    WPF 是 .net 的一部分，因此，如果以前使用 ASP.NET 或 Windows 窗体通过 .net 生成了应用程序，则应熟悉编程体验
    <br>
    WPF 使用 Extensible Application Markup Language (XAML)，为应用编程提供声明性模型
</section>

### WPF 简介

`WPF` 是 `Windows Presentation Foundation` 的缩写。它是微软公司最新的利用 `.NET` 框架解决 `GUI` 框架的方案。

但是 `GUI` 框架到底是什么？`GUI` 是图形用户界面（`Graphical User Interface`）的缩写，很有可能你现在看到界面的就是一个。为了与你的电脑交互，`Windows` 系统使用了 `GUI`，并且，你用来阅读本文的浏览器也是一个用于浏览网页的 `GUI`。

一个 `GUI` 架构可以让你创造具备多种 `GUI` 元件的应用，比如标签，文本框以及其他广为人知的元件。如果没有 `GUI` 框架，你不得不手动地去绘制这些元件，并且以文字和鼠标输入的方式处理所有的用户交互。这样的工程量会很大，因此，大多数的开发者都会使用 `GUI` 框架来承担所有的基础工作，以便自己专注于更高层面的应用开发。

`GUI` 框架有许多，但是对于.NET开发者来说，目前最令他们感兴趣的是 `WinForms` 以及 `WPF`。`WPF` 是最新的，但是微软公司依然维护和支持 `WinForms`。这两者存在着不少不同之处，但是它们的目的是一致的：将创造出带有漂亮 `GUI` 的应用的工作变得简单。

### WPF 与 WinForms 区别

这两个框架的设计目标基本相同，但实际上两者之间的差异非常巨大，我会在这一节对这方面进行分析。

`WinForms` 与 `WPF` 间最大的差异在于 `WinForms` 只是单纯在 `Windows` 标准控制项 (例如：`TextBox`) 上迭一层，而 `WPF` 几乎是全面从零建构，并未依赖任何 `Windows` 标准控制项。这差异看起来很微妙，实则不然。如果你曾经使用过依赖 `Win32/WinAPI` 的框架，就一定会注意到这种差异。

举个例子，要是想实现一个带有图像和文本的按钮，在 `WinForms` 里面，你只能自己用画图之类的方式特意去实现一个（或者用第三方控件），因为 ”一个带有图像和文本的按钮”并不是一个标准的 `Windows` 控件。而在 `WPF` 里面，这可以通过递归组合的方式轻松实现，具体来讲，就是在按钮（`Button`）中放置一个图像（`Image`）和文本方块（`TextBlock`）而已。实际上，大部分的 `WPF` 控件都能用这种方式随意组合，一个控件可以包含其他任何控件，你可以透过组合各种基本控件来产生复杂控件，以满足不同的需求，而这种灵活正是 `WinForms` 所不具备的。

`WPF` 这种灵活性所带来的缺点是：你需要做更多的事来做出在 `winForm` 中很容易做出的内容。 因为 `WPF` 是专为你所想要的内容而生的。你或多或少在一开始会有这种感觉，比如当你试着用 `WPF` 来实现一个具有图片元素和华丽字体的 `ListView` 的时候，`WinForms` 的 `ListView` 控件用一句话就能完成了。

这只是两者的一个区别，但是当你使用 `WPF` 时，你会发现这其实是造成其他区别的根本原因-- `WPF` 仅仅是在用自己的方式来实现所有的东西，不论好坏。你不再局限于 `Windows` 的解决方案。然而为了得到这种灵活性，当你真正想要做出 `Windows` 风格的东西时，往往需要花费更多的精力。

以下是 `WPF` 和 `WinForms` 关键优势的主观描述。应该可以让你更好地选择用哪一种技术。

#### WPF 的优势

- 更年轻，与时俱进
- 微软在很多新的应用中使用它，例如 `Visual Studio`
- 更灵活，你不需要自己造或者购买新的控件，就可以完成更多的工作
- 当你使用第三方控件时，开发人员更青睐于新生的 `WPF`
- `XAML` 可以让你更简单地创建和修改你的 `GUI`（界面），并且使前台设计人员和后台编程人员可以分离（ `C#`，`VB.NET` 等等）
- 数据绑定使数据和界面的分离更加简洁
- 使用硬件加速来描绘 `GUI`（界面），性能更好
- 可以允许给 `Windows` 应用和 `Web` 应用同时创建用户接口

#### WinForms 的优势

- 更久远，因此久经考验
- 已经有很多免费或收费的第三方控件供你使用
- 就 `WinForms` 而言，在 `Visual Studio` 中的设计器仍然比 `WPF` 更好，在 `WPF` 中更多的工作需要你自己来完成。

### 开发工具

正像之前所说的一样，`WPF` 是 `XAML`（标记语言）、`C＃/VB.NET` 以及其他任意 `.NET` 语言的组合。`XAML` 和 `.NET` 语言的源代码都被任意编辑器编辑（即使是 `Windows` 也可以）下面的内容），然后通过命令行编译。不过，大多数开发人员都喜欢使用 `IDE`（集成开发环境），因为 `IDE` 植入编辑代码、设计 `UI`、编译等各种工作都更容易了。

`Visual Studio` 本身很昂贵，但对 `.NET/WPF` 开发的首选。不过微软开发了一个免费的社区版来让每个人都能方便地使用 `.NET` 和 `WPF`。社区版功能会少一些，但学习 `WPF` 以及已经制作了实际的应用，已经完全成熟了。

所以从微软官网下载 `VS` 社区版吧，免费，而且安装和使用都很方便。

https://www.visualstudio.com/vs/community/

并完成安装之后，就可以开始我们的 `WPF` 学习之旅了。

![](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/image-20220210100314993.png)

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>特别注意</strong>
    <br><br>
    1. 工业 4.0 的浪潮正在席卷整个行业，以 WPF 开发上位机、工业互联、物联网方向的应用程序依然火爆
    <br>
    2. WPF 中用到的 MVVM 模式以及标记语言 XAML 在接下来的 MAUI 框架中依然延续，作为后续的学习奠定基础
    <br>
    3. IT 行业注定了持续学习，加油
</section>

<br>


感谢阅读 :coffee:

