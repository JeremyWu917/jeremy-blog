# WPF 基础系列教程 - 应用程序

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    和其他 .NET 应用一样，运行 WPF 应用需要 .NET 框架
    <br>
    从 Windows Vista 开始，微软已将 .NET 框架内置于所有 Windows 版本中，对于更早的版本，.NET 框架也通过 Windows 更新而被追加。也就是说，大部分 Windows 用户都可以运行 WPF 应用
    <br>
    接下来我们将了解 WPF 应用程序的结构以及其他各个方面
</section>


### 窗体

当我们创建一个 `WPF` 应用时，我们首先看到的是 `Windows` 类。作为窗体的根节点，提供了边框、标题栏和最小化/最大化和关闭按钮

`WPF` 窗体是 `XAML` 文件和 `CS` 后台代码文件的组合

`XAML` 代码如下：

```xaml
<Window x:Class="_02_Wpf_App.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:_02_Wpf_App"
        mc:Ignorable="d"
        Title="MainWindow" Height="450" Width="800">
    <Grid>

    </Grid>
</Window>
```

`CS` 代码如下：

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Data;
using System.Windows.Documents;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Imaging;
using System.Windows.Navigation;
using System.Windows.Shapes;

namespace _02_Wpf_App
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }
    }
}
```

<br>

<section style="border-left: 5px solid #cc0000; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>特别注意</strong>
    <br><br>
    1. 在 XAML 代码中 x:Class 指示了所使用的类为 MainWindow
    <br>
    2. MainWindow 类被定义为部分，因为它在运行时与 XAML 文件组合后提供完整的窗体。这实际上是调用 InitializeComponent() 完成的
    <br>
    3. XAML 文件中 Window 节点自动创建了一个 Grid 控件。Grid 是 WPF 面板之一。虽然这个被包含在 Window 中的控件也可以是任何面板或控件，但 Window 只能拥有一个子控件，因此，使用一个可以包含多个子控件的面板通常是非常必要的，这就涉及到页面的布局了，后面我们会展开讲解
</section>

#### 重要的 Windows 属性

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    WPF 的 Window 类有许多有趣的属性，您可以设置这些属性来控制应用程序窗口的外观和行为
</section>

**Icon** - 允许你定义窗口的图标，该图标通常显示在窗口标题之前的左上角

**ResizeMode** - 这可以控制最终用户是否以及如何调整窗口大小。默认是 `CanResize` 允许用户像任何其他窗口一样调整窗口大小，使用最大化/最小化按钮或拖动其中一个边缘。`CanMinimize` 将允许用户最小化窗口，但不能最大化它或拖动它更大或更小。`NoResize` 是最严格的，最大化和最小化按钮被移除，窗口不能被拖得更大或更小

**ShowInTaskbar** - 默认值为 `true`，但如果将其设置为 `false`，则窗口将不会在 `Windows` 任务栏中显示。适用于非主窗口或应尽量减少托盘的应用程序

**SizeToContent** - 决定 `Window` 是否应调整自身大小以自动适应其内容。默认是 `Manual`, 这意味着窗口不会自动调整大小。其他选项有 `Width`，`Height` 和 `WidthAndHeight`，分别对应自动调整宽度，高度或同时调整两者

**Topmost** - 默认是 `false`, 但如果设置为 `true`，除非最小化，否则您的窗口将保持在其他窗口之上。仅适用于特殊情况

**WindowStartupLocation** - 控制窗口的初始位置。默认是 `Manual`, 表示窗口最初将根据窗口的 `Top` 和 `Left` 属性进行定位。其他选项是 `CenterOwner`，它将窗口定位在其所有者窗口的中心，以及 `CenterScreen`，它将窗口定位在屏幕的中心

**WindowState** - 控制初始窗口状态。它可以是 `Normal`，`Maximized` 或 `Minimized`。默认值为 `Normal`，除非您希望窗口最大化或最小化，否则应该使用它

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>警告</strong>
    <br><br>
    Windows 属性有很多，以上只是介绍了常用的一些，我们可以根据需要进行配置
</section>

### 使用 App.xaml

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    App.xaml 是应用程序定义的起点。当我们创建一个新的 WPF 应用时，Visual Stuido 将自动为你创建它，同时还包括一个名为 App.xaml.cs 的后置代码文件
    <br>
    跟 Window 类似，这两个文件里面定义的是部分类，它们允许你同时在 XAML 标记和后置代码中完成工作
</section>

`App.xaml.cs` 继承自 `Application` 类，在 `WPF Windows` 应用程序中是一个中心类

`.NET` 会进入这个类，从这里启动需要的窗口或页面。这也是一个订阅一些重要应用程序事件的地方，例如，应用程序启动事件，未处理的异常事件等，本节不再展开后面我们会展开讲解

`App.xaml` 文件最常被用到的功能之一，是定义全域性资源，而那些资源可能会被用在整个应用程式中，用来取代全域风格。同样的，本节不再展开后面我们会展开讲解

#### App.xaml 结构

```xaml
<Application x:Class="_02_Wpf_App.App"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:local="clr-namespace:_02_Wpf_App"
             StartupUri="MainWindow.xaml">
    <Application.Resources>
         
    </Application.Resources>
</Application>
```

这里要注意的主要是 `StartupUri` 属性，它实际上指定了当应用程序启动时应该被加载的 `Window` 或 `Page`。在这个例子中，`MainWindow.xaml` 会被启动，但是如果你想使用另外一个 `Window` 作为启动入口点，你只需要修改一下这个属性即可

在某些场景下，你可能想在第一个窗口显示时或者在它的显示方式上多一些控制。在这个例子中，你可以移除 `StartupUri` 属性和值，然后把对它做的所有工作通过代码后置方式来替代

#### App.xaml.cs 结构

```c#
using System;
using System.Collections.Generic;
using System.Configuration;
using System.Data;
using System.Linq;
using System.Threading.Tasks;
using System.Windows;

namespace _02_Wpf_App
{
    /// <summary>
    /// Interaction logic for App.xaml
    /// </summary>
    public partial class App : Application
    {
    }
}
```

这个类继承自 `Application` 类，它允许我们在应用级别去做一些事情。举个例子，你可以订阅 `Startup` 事件，然后手动创建你的启动窗口

#### 示例

订阅 `Startup` 事件

```xaml
<Application x:Class="_02_Wpf_App.App"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:local="clr-namespace:_02_Wpf_App"
             Startup="Application_Startup"
             StartupUri="MainWindow.xaml">
    <Application.Resources>
         
    </Application.Resources>
</Application>
```

使用 `Startup` 事件：

```C#
using System;
using System.Collections.Generic;
using System.Configuration;
using System.Data;
using System.Linq;
using System.Threading.Tasks;
using System.Windows;

namespace _02_Wpf_App
{
    /// <summary>
    /// Interaction logic for App.xaml
    /// </summary>
    public partial class App : Application
    {
        private void Application_Startup(object sender, StartupEventArgs e)
        {
            // Create the startup window
            MainWindow wnd = new()
            {
                // Do stuff here, e.g. to the window
                Title = "Something else"
            };
            // Show the window
            wnd.Show();
        }
    }
}
```

运行效果：

![](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/image-20220222093043593.png)

### 命令行参数

命令行参数是一种技术，你可以给一个你想要启动的应用程序传一组参数，以某种方式去影响它。最常见的例子就是让应用程序去打开一个指定的文件，例如，在一个编辑器里打开。你可以用 `windows` 内置的 `Notepad` 应用程序尝试一下，通过运行（从【开始菜单】中选择 【运行】或按下[ `Windows` 键 + `R` ]）：

```powershell
notepad.exe c:\Windows\win.ini
```

![](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/image-20220222093502403.png)

这个会打开 `Notepad` 去打开 `win.ini` 文件（你可能需要调整一下路径，以对应到你的系统）。`Notepad` 简单的地查找一个或几个参数，然后使用这些参数，你的应用程序也可以这么做。

命令行的参数通过 `Startup` 事件(它是我们在 `App.xaml` 文章中订阅过的事件)传给你的WPF应用程序，我们会在这个例子中做同样的事情，然后使用这些通过方法参数传进来的值。首先，是 `App.xaml` 文件

```xaml
<Application x:Class="_02_Wpf_App.App"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:local="clr-namespace:_02_Wpf_App"
             Startup="Application_Startup"
             StartupUri="MainWindow.xaml">
    <Application.Resources>
         
    </Application.Resources>
</Application>
```

我们所做的就是去订阅 **Startup** 事件。然后在 `App.xaml.cs` 中实现这个事件

```C#
using System;
using System.Collections.Generic;
using System.Configuration;
using System.Data;
using System.Linq;
using System.Threading.Tasks;
using System.Windows;

namespace _02_Wpf_App
{
    /// <summary>
    /// Interaction logic for App.xaml
    /// </summary>
    public partial class App : Application
    {
        private void Application_Startup(object sender, StartupEventArgs e)
        {
            // Create the startup window
            MainWindow wnd = new();
            // Do stuff here, e.g. to the window
            // wnd.Title = "Something else";
            if (e.Args.Length == 1)
            {
                _ = MessageBox.Show("Now opening file: \n\n" + e.Args[0]);
            }
            // Show the window
            wnd.Show();
        }
    }
}
```

<br>

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>警告</strong>
    <br><br>
    StartupEventArgs 是我们这里使用的。它和它的名称 e 被传给了 Application Startup 事件。它有 Args 属性，这个属性是一组字符串。除了引号当中的空格，命令行的参数是以空格来分割的
</section>

#### 测试命令行

如果直接运行上面的例子，不会发生任何变化，因为没有指定任何的命令参数

我们可以通过 `Visual Studio` 使得它在你的应用程序中易于测试

从**项目**菜单选择**"[程序名称] 属性"**然后进入到 **Debug** 页签（你可以在这里定义命令行参数）。这个应该能够看到像这样的内容：

![](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/image-20220222104148389.png)

#### 运行效果

![](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/image-20220222104408022.png)

#### 命令行的潜在价值

自动化任务，自动化定时报表等

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>项目地址</strong>
    <br><br>
    GitHub: https://github.com/JeremyWu917/tutorial-wpf
    <br>
    持续更新，欢迎 Watch、Fork 和 Star
</section>

<br>

感谢阅读 :coffee:

