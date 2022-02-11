# WPF 基础系列教程 - 创建第一个 WPF 程序

<section style="border-left: 5px solid #42b983; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    安装好开发环境之后，咱们就可以开始创建一个简单的 WPF 应用程序了 
    <br>
    我们用 Visual Studio 2019 创建一个 WPF 应用程序，运行后显示 Hi WPF 字样
</section>

### 创建 WPF 应用

1. 打开 `IDE` ，点击 `Continue without code` 

![image-20220211091423847](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/image-20220211091423847.png)

2. 单击菜单栏 `File` -> `New` -> `Project` 打开创建工程面板

![image-20220211090831855](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/image-20220211090831855.png)

3. 在创建新工程面板中，选择 "Blank Solution"，您可以借助搜索框快速找到 "Blank Solution"，然后单击 `Next` 按钮

![image-20220211090948200](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/image-20220211090948200.png)

4. 在配置新项目界面输入解决方案名称和保存路径后，然后单击 `Create` 按钮

![image-20220211091126673](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/image-20220211091126673.png)

5. 单击菜单栏 `File` -> `New` -> `Project` 打开创建工程面板

![image-20220211090831855](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/image-20220211090831855.png)

6. 在创建新工程面板中，选择 "WPF Application"，您可以借助搜索框快速找到 "WPF Application"，然后单击 `Next` 按钮

![image-20220211094022909](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/image-20220211094022909.png)

7. 在配置新项目的界面输入工程名称，保存路径选择你刚才创建的空白解决方案的路径，<strong>注意：Solution 这里选择 Add to solution</strong>，然后点击 `Next` 按钮

![image-20220211093813007](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/image-20220211093813007.png)

8. 当您看到如下界面的时候，说明项目已经创建成功了

![image-20220211093933338](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/image-20220211093933338.png)

### XMAL 代码

所见即所得，我们可以在如下 `XMAL` 编辑框中进行界面的设计

初始代码：

```xaml
<Window x:Class="_00_Hello_WPF.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:_00_Hello_WPF"
        mc:Ignorable="d"
        Title="MainWindow" Height="450" Width="800">
    <Grid>

    </Grid>
</Window>
```

修改代码：

```xaml
<Window x:Class="_00_Hello_WPF.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:local="clr-namespace:_00_Hello_WPF"
        mc:Ignorable="d"
        Title="MainWindow" Height="450" Width="800">
    <Grid>
        <TextBlock HorizontalAlignment="Center" VerticalAlignment="Center" FontSize="30">Hi WPF</TextBlock>
    </Grid>
</Window>
```

![wpf_first_demo](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/wpf_first_demo.gif)

![wpf_first_demo2](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/wpf_first_demo2.gif)

### 运行效果

![wpf_first_demo3](https://gitee.com/jeremywuiot/img-res-all/raw/master/src/iie_shop/wpf_first_demo3.gif)

<section style="border-left: 5px solid #e7c000; padding: 10px; background-color: #f3f5f7; font-size: 10px;">
    <strong>项目地址</strong>
    <br><br>
    GitHub: https://github.com/JeremyWu917/tutorial-wpf
    <br>
    持续更新，欢迎 Watch、Fork 和 Star
</section>


<br>


感谢阅读 :coffee:

