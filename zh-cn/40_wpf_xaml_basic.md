# WPF 基础系列教程 - XAML 基础

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    XAML 是可扩展应用标记语言的缩写，是微软用于描述 GUI 的 XML 变种
    <br>
    在之前的 GUI 框架如 WinForms 中，GUI 是用相同语言创建，例如 C# 或 VB.NET，并且通常由设计者来维护（例如 Visual Studio ）。但是，通过 XAML，微软使用了另一种方式。非常类似 HTML，你现在可以轻松写你的 GUI
    <br>
    XAML 是 WPF 最基础的部分。不管你是创建 1 个窗体还是页面，都会包含 1 个 XAML 文档和 1 个背后的程序文件，这各自共同创建了这个窗体/页面。XAML 文件描述了所有元素的接口，程序文件处理了所有事件并能够操作 XAML 控件
    <br>
    本节简单介绍一下 XAML 的工作原理以及如何使用它来创建你的界面
</section>

### XAML 基础

在上次的讲解中，我们在 `XAML` 文件里创建了一个 `TextBlock` 控件，并为控件设定了对齐方式为居中对齐，字体大小为 `30px`，控件的内容为 `Hi WPF`，例如：

```xaml
<TextBlock HorizontalAlignment="Center" VerticalAlignment="Center" FontSize="30">Hi WPF</TextBlock>
<!-- 或者 -->
<TextBlock HorizontalAlignment="Center" VerticalAlignment="Center" FontSize="30" Text="Hi WPF"/>
```

我们也可以定义一个按钮

```xaml
<Button></Button>
<!-- 或者 -->
<Button />
```

通过以上代码你会发现 `XAML` 标签必须有结尾，在起始标签尾部用斜杠或者使用结束标签都可以，并且 `XAML` 和 `HTML` 语法非常相似

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>特别注意</strong>
    <br><br>
    HTML 不区分大小写，但 XAML 区分
    <br>
    在 XAML 中控件的名字最终会有对应到 .NET 框架下的类型( Type )，同理 XAML 标签的属性也区分大小写，因为这是控件的属性
</section>

另外，我们还可以通过标签来指定控件的属性内容，标签名点号连接控件名和属性名，例如：

```xaml
<Button>
    <Button.FontWeight>Bold</Button.FontWeight>
    <Button.Content>Click Me</Button.Content>
</Button>
```

在 `XAML` 中大多数控件允许使用文字以外的作为内容，我们可以在按钮中嵌套 `TextBlock` 控件来显示 `3` 种不同颜色的文字，例如：

```xaml
<Button>
    <Button.FontWeight>Bold</Button.FontWeight>
    <Button.Content>
        <WrapPanel>
            <TextBlock Foreground="Yellow">Yellow</TextBlock>
            <TextBlock Foreground="Pink">Pink</TextBlock>
            <TextBlock>Click Me</TextBlock>
        </WrapPanel>
    </Button.Content>
</Button>
```

当然，这里的 `FontWeight` 属性也可以直接写在 `Button` 按钮里面

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>警告</strong>
    <br><br>
    内容( Content )属性只接受一个元素, 所以我们用 WrapPanel 控件把三个 TextBlock 控件包起来了
    <br>
    Panel 控件有很多的用途，本节不再展开这个我们以后再讲
</section>

运行效果如下：

![](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/image-20220211151538662.png)

### XAML 事件

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    现在大多数的 UI 框架都是事件驱动的，WPF 也是
    <br>
    所有的控件，包括窗体（ Window 控件）都提供了大量的事件可以订阅。您订阅这些事件，这表明您的应用程序将在事件发生的时候接受程序的通知并且您可以对这些事件形成响应
    <br>
    有很多不同类型的事件，大量的事件用于在用户使用鼠标键盘和你的应用互动的时候。在大多数的控件上你会找到  KeyDown, KeyUp, MouseDown, MouseEnter, MouseLeave, MouseUp 等类的事件
</section>

我们为主窗体的 `Grid` 命名为 `MainGrid` ，并且为了好区分我们为它设置一个颜色，最后再订阅了 `MouseUp` 事件，自动生成的方法名为 `MainGrid_MouseUp`，如下：

![](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/wpf_basic_xaml1.gif)

写一个简单事件弹出区域坐标信息，代码如下：

```c#
private void MainGrid_MouseUp(object sender, MouseButtonEventArgs e)
{
	_ = MessageBox.Show($"Clicked at position: [ {e.GetPosition(this)} ]");
}
```

运行效果：

![](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/wpf_basic_xaml2.gif)

<br>

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>警告</strong>
    <br><br>
    _ = 表示丢弃返回的结果
    <br>
    插补实现字符串的连接
    <br>
    你也可以直接在代码中订阅事件，在 C# 中这会使用到 += 语法
</section>

在 `C#` 代码中直接实现事件订阅：

```C#
/// <summary>
/// Interaction logic for MainWindow.xaml
/// </summary>
public partial class MainWindow : Window
{
   public MainWindow()
   {
       InitializeComponent();
       MainGrid.MouseUp += MainGrid_MouseUp1;
   }

   private void MainGrid_MouseUp1(object sender, MouseButtonEventArgs e)
   {
       _ = MessageBox.Show("The subscribe event in your code had been fired.");
   }
}
```

运行效果：

![wpf_basic_xaml3](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/wpf_basic_xaml3.gif)

<br>

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>特别注意</strong>
    <br><br>
    您需要知道使用那种委托，在 Visual Studio 中提供了丰富的语法糖
    <br>
    输入 += 后，双击 【TAB】键会自动生成正确的事件处理方案，您只需要实现方法的内容就可以了
</section>

就像这样：

![](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/wpf_basic_xaml4.gif)


感谢阅读 :coffee:

