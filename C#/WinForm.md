## 窗体属性

窗体的常用属性如下表所示。

| 属性                  | 作用                                                         |
| --------------------- | ------------------------------------------------------------ |
| Name                  | 用来获取或设置窗体的名称                                     |
| WindowState           | 获取或设置窗体的窗口状态，取值有3种，即Normal（正常）、Minimized（最小化）、Maximized（最大化），默认为 Normal，即正常显示 |
| StartPosition         | 获取或设置窗体运行时的起始位置，取值有 5 种，即 Manual（窗体位置由 Location 属性决定）、CenterScreen（屏幕居中）、WindowsDefaultLocation（ Windows 默认位置）、WindowsDefaultBounds（Windows 默认位置，边界由 Windows 决定）、CenterParent（在父窗体中居中），默认为 WindowsDefaultLocation |
| Text                  | 获取或设置窗口标题栏中的文字                                 |
| MaximizeBox           | 获取或设置窗体标题栏右上角是否有最大化按钮，默认为 True      |
| MinimizeBox           | 获取或设置窗体标题栏右上角是否有最小化按钮，默认为 True      |
| BackColor             | 获取或设置窗体的背景色                                       |
| BackgroundImage       | 获取或设置窗体的背景图像                                     |
| BackgroundImageLayout | 获取或设置图像布局，取值有 5 种，即 None（图片居左显示）、Tile（图像重复，默认值）、Stretch（拉伸）、Center（居中）、Zoom（按比例放大到合适大小） |
| Enabled               | 获取或设置窗体是否可用                                       |
| Font                  | 获取或设置窗体上文字的字体                                   |
| ForeColor             | 获取或设置窗体上文字的颜色                                   |
| Icon                  | 获取或设置窗体上显示的图标                                   |

Program.cs 文件的代码如下。

```cs
static class Program
{
    /// <summary>
    /// 应用程序的主入口点。
    /// </summary>
    [STAThread]
    static void Main()
    {
        Application.EnableVisualStyles();
        Application.SetCompatibleTextRenderingDefault(false);
        Application.Run(new Form1());
    }
}
```

在上述代码的 Main 方法中的：

- 第 1 行代码：用于启动应用程序中可视的样式。
- 第 2 行代码：控件支持 UseCompatibleTextRenderingproperty 属性，该方法将此属性设置为默认值。
- 第 3 行代码：用于设置在当前项目中要启动的窗体，这里 new Form1() 即为要启动的窗体。

## 窗体事件

窗体中常用的事件如下表所示。

| 事件             | 作用                                     |
| ---------------- | ---------------------------------------- |
| Load             | 窗体加载事件，在运行窗体时即可执行该事件 |
| MouseClick       | 鼠标单击事件                             |
| MouseDoubleClick | 鼠标双击事件                             |
| MouseMove        | 鼠标移动事件                             |
| KeyDown          | 键盘按下事件                             |
| KeyUp            | 键盘释放事件                             |
| FormClosing      | 窗体关闭事件，关闭窗体时发生             |
| FormClosed       | 窗体关闭事件，关闭窗体后发生             |

双击可编写具体的代码：

使用代码设置的方式是使用 this 关键字代表当前窗体的实例。

BackColor 属性类型是 Color 枚举类型的(在WinForm章节中直接使用，无需类名.枚举数据类型名进行指定调用)，代码如下。

```cs
this.BackColor = Color.Red;
```

## 窗体方法

自定义的窗体都继承自 System.Windows.Form 类，能使用 Form 类中已有的成员，包括属性、方法、事件等。

| 方法                      | 作用                     |
| ------------------------- | ------------------------ |
| void Show()               | 显示窗体                 |
| void Hide()               | 隐藏窗体                 |
| DialogResult ShowDialog() | 以对话框模式显示窗体     |
| void CenterToParent()     | 使窗体在父窗体边界内居中 |
| void CenterToScreen()     | 使窗体在当前屏幕上居中   |
| void Activate()           | 激活窗体并给予它焦点     |
| void Close()              | 关闭窗体                 |

**实例：**

添加所需的 MainForm 窗体和 NewForm 窗体。

```cs
public partial class MainForm : Form
{
    public MainForm()
    {
        InitializeComponent();
    }
    //创建NewForm 窗体实例
    private void MainForm_MouseClick(object sender, MouseEventArgs e)
    {
        //创建NewForm窗体实例
        NewForm newForm = new NewForm();
        //打开NewForm窗体
        newForm.Show();
    }
}
```

```cs
public partial class NewForm : Form
{
    public NewForm()
    {
        InitializeComponent();
    }
    //窗体的鼠标单击事件
    private void NewForm_MouseClick(object sender, MouseEventArgs e)
    {
        //将窗体居中
        this.CenterToScreen();
    }
    //窗体的鼠标双击事件
    private void NewForm_MouseDoubleClick(object sender, MouseEventArgs e)
    {
        //关闭窗体
        this.Close();
    }
}
```

当前窗体需要调用方法直接使用 this 关键字代表当前窗体，通过this.方法名 (参数列表)的方式调用即可。

如果要操作其他窗体，则需要用窗体的实例来调用方法。

## McssageBox：消息框

消息框是通过 McssageBox 类来实现的，在 MessageBox 类中仅定义了 Show 的多个重载方法，该方法的作用就是弹出一个消息框。

由于 Show 方法是一个静态的方法，因此调用该方法只需要使用MessageBox.Show( 参数 )的形式即可弹出消息框。

| 方法                                                         | 说明                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| DialogResult Show(string text)                               | 指定消息框中显示的文本（text）                               |
| DialogResult Show(string text, string caption)               | 指定消息框中显示的文本（text）以及消息框的标题（caption）    |
| DialogResult Show(string text, string caption, MessageBoxButtons buttons) | 指定消息框中显示的文本（text）、消息框的 标题（caption）以及消息框中显示的按钮 （buttons） |
| DialogResult Show(string text, string caption, MessageBoxButtons buttons, MessageBoxIcon icon) | 指定消息框中显示的文本（text）、消息框的 标题（caption ）、消息框中显示的按钮 （buttons）以及消息框中显示的图标（icon） |

MessageBoxButtons 枚举类型主要用于设置消息框中显示的按钮，具体的枚举值如下。

- OK：在消息框中显示“确定”按钮。
- OKCancel：在消息框中显示“确定”和“取消”按钮。
- AbortRetryIgnore：在消息框中显示“中止” “重试”和“忽略”按钮。
- YesNoCancel：在消息框中显示“是” “否”和“取消”按钮。
- YesNo：在消息框中显示“是”和“否”按钮。
- RetryCancel：在消息框中显示“重试”和“取消”按钮。

MessageBoxIcon 枚举类型主要用于设置消息框中显示的图标，具体的枚举值如下。

- None：在消息框中不显示任何图标。
- Hand、Stop、Error：在消息框中显示由一个红色背景的圆圈及其中的白色X组成 的图标。
- Question：在消息框中显示由圆圈和其中的一个问号组成的图标。
- Exclamation、Warning：在消息框中显示由一个黄色背景的三角形及其中的一个感叹号组成的图标。
- Asterisk、Information：在消息框中显示由一个圆圈及其中的小写字母 i 组成的图标。

调用 MessageBox 类中的 Show 方法将返回一个 DialogResult 类型的值。

DialogResult 也是一个枚举类型，是消息框的返回值，通过单击消息框中不同的按钮得到不同的消息框返回值。

DialogResult 枚举类型的具体值如下。

- None：消息框没有返回值，表明有消息框继续运行。
- OK：消息框的返回值是 0K （通常从标签为“确定”的按钮发送）。
- Cancel：消息框的返回值是 Cancel （通常从标签为“取消”的按钮发送）。
- Abort：消息框的返回值是 Abort （通常从标签为“中止”的按钮发送）。
- Retry：消息框的返回值是 Retry （通常从标签为“重试”的按钮发送）。
- Ignore：消息框的返回值是 Ignore （通常从标签为“忽略“的按钮发送）。
- Yes：消息框的返回值是 Yes （通常从标签为“是“的按钮发送）。
- No：消息框的返回值是 No （通常从标签为“否“的按钮发送）。

```cs
public partial class MainForm : Form
{
    public MainForm()
    {
        InitializeComponent();
    }
    private void MainForm_MouseClick(object sender, MouseEventArgs e)
    {
        //弹出消息框，并获取消息框的返回值
        DialogResult dr = MessageBox.Show("是否打开新窗体？", "提示", MessageBoxButtons.YesNo, MessageBoxIcon.Warning);
        //如果消息框返回值是Yes，显示新窗体
        if (dr == DialogResult.Yes)
        {
            MessageForm messageForm = new MessageForm();
            messageForm.Show();
        }
        //如果消息框返回值是No，关闭当前窗体
        else if (dr == DialogResult.No)
        {
            //关闭当前窗体
            this.Close();
        }
    }
}
```

## Label和LinkLabel：标签控件

标签控件王要分为普通的标签 (Label) 和超链接形式的标签 (LinkLabel) 。

普通标签 (Label) 控件的常用属性如下表所示。

| 属性名    | 作用                                                         |
| --------- | ------------------------------------------------------------ |
| Name      | 标签对象的名称，区别不同标签唯一标志                         |
| Text      | 标签对象上显示的文本                                         |
| Font      | 标签中显示文本的样式                                         |
| ForeColor | 标签中显示文本的颜色                                         |
| BackColor | 标签的背景颜色                                               |
| Image     | 标签中显示的图片                                             |
| AutoSize  | 标签的大小是否根据内容自动调整，True 为自动调整，False 为用户自定义大小 |
| Size      | 指定标签控件的大小                                           |
| Visible   | 标签是否可见，True 为可见，False 为不可见                    |

普通标签控件 (Label) 中的事件与窗体的事件类似，常用的事件主要有鼠标单击事件、 鼠标双击事件、标签上文本改变的事件等。

超链接标签控件 (LinkLabel) 也具有与普通标签控件类似的属性和事件。

超链接标签主要应用的事件是鼠标单击事件，通过单击标签完成不同的操作。

```cs
public partial class ChangeTextForm : Form
{
    public ChangeTextForm()
    {
        InitializeComponent();
    }
    //超链接标签控件的单击事件
    private void linkLabel1_LinkClicked(object sender, LinkLabelLinkClickedEventArgs e)
    {
        //交换标签上的信息。
        string temp = label1.Text;
        label1.Text = label2.Text;
        label2.Text = temp;
    }
}
```

## TextBox：文本框控件

文本框的属性如下：

| 属性名       | 作用                                                         |
| ------------ | ------------------------------------------------------------ |
| Text         | 文本框对象中显示的文本                                       |
| MaxLength    | 在文本框中最多输入的文本的字符个数                           |
| WordWrap     | 文本框中的文本是否自动换行，如果是 True，则自动换行，如果是 False，则不能自动换行 |
| PasswordChar | 将文本框中出现的字符使用指定的字符替换，通常会使用“*”字符    |
| Multiline    | 指定文本框是否为多行文本框，如果为 True，则为多行文本框，如果 为 False，则为单行文本框 |
| ReadOnly     | 指定文本框中的文本是否可以更改，如果为 True，则不能更改，即只读文本框，如果为 False，则允许更改文本框中的文本 |
| Lines        | 指定文本框中文本的行数                                       |
| ScrollBars   | 指定文本框中是否有滚动条，如果为 True，则有滚动条,如果为 False， 则没有滚动条 |

最常用的事件是文本改变事件(TextChange)，即在文本框控件中的内容改变时触发该事件。

```cs
public partial class TextBoxTest : Form
{
    public TextBoxTest()
    {
        InitializeComponent();
    }
    //文本框文本改变事件
    private void textBox1_TextChanged(object sender, EventArgs e)
    {
        //将文本框中的文本值显示在标签中
        label2.Text = textBox1.Text;
    }
}
```

```cs
public partial class LoginForm : Form
{
    public LoginForm()
    {
        InitializeComponent();
    }
    //判断是否登录成功
    private void linkLabel1_LinkClicked(object sender, LinkLabelLinkClickedEventArgs e)
    {
        //获取用户名
        string username = textBox1.Text;
        //获取密码
        string password = textBox2.Text;
        //判断用户名密码是否正确
        if ("xiaoming".Equals(username) && "123456".Equals(password))
        {
            MessageBox.Show("登录成功！");
        }
        else
        {
            MessageBox.Show("登录失败！");
        }
    }
}
```

## Button：按钮控件

按钮包括普通的按钮 (Button)、单选按钮 (RadioButton)。

按钮常用的属性包括在按钮中显示的文字 (Text) 以及按钮外观设置的属性，最常用的事件是单击事件。

```cs
public partial class RegForm : Form
{
    public RegForm()
    {
        InitializeComponent();
    }
    //“确定”按钮的单击事件，用于判断注册信息并跳转到新窗口显示注册信息
    private void button1_Click(object sender, EventArgs e)
    {
        string name = textBox1.Text;
        string pwd = textBox2.Text;
        string repwd = textBox3.Text;
        if (string.IsNullOrEmpty(name))
        {
            MessageBox.Show("用户名不能为空！");
            return;
        }
        else if (string.IsNullOrEmpty(textBox2.Text))
        {
            MessageBox.Show("密码不能为空！");
            return;
        }
        else if (!textBox2.Text.Equals(textBox3.Text))
        {
            MessageBox.Show("两次输入的密码不一致！");
            return;
        }
        //将用户名和密码传递到主窗体中
        MainForm mainForm = new MainForm(name, pwd);
        mainForm.Show();
    }
    //“取消”按钮的事件，用于关闭窗体
    private void button2_Click(object sender, EventArgs e)
    {
        //关闭窗体
        this.Close();
    }
}
```

```cs
public partial class MainForm : Form
{
    public MainForm(string name,string pwd)
    {
        InitializeComponent();
        label2.Text = "用户名："+ name;
        label3.Text = "密码："+pwd;
    }
}
```

如果需要在两个窗体中传递参数，则可以使用按钮和文本框。

## RadioButton：单选按钮控件

RadioButton 是单选按钮控件，多个 RadioButton 控件可以为一组，这一组内的 RadioButton 控件只能有一个被选中。

<img src="https://raw.githubusercontent.com/wqcblog/picgo-image/master/2022/05/20220520_1653009654.gif" alt="4-1Z329145K9310" style="zoom:80%;" />

```cs
public partial class RadioButtonForm : Form
{
    public RadioButtonForm()
    {
        InitializeComponent();
    }
    //单击“确定”按钮的事件
    private void button1_Click(object sender, EventArgs e)
    {
        string msg = "";
        if (radioButton1.Checked)
        {
            msg = radioButton1.Text;
        }
        else if (radioButton2.Checked)
        {
            msg = radioButton2.Text;
        }
        else if (radioButton3.Checked)
        {
            msg = radioButton3.Text;
        }
        MessageBox.Show("您选择的权限是：" + msg, "提示");
    }
}
```

>提示：Checked 属性可用于判断单选按钮是否被选中。如果该属性的返回值为 True，则代表选中；如果返回值为 False，则表示未选中。

## CheckBox：复选框控件

复选框主要的属性是：Name、Text、Checked。

其中：

- Name：表示这个组件的名称；
- Text：表示这个组件的标题；
- Checked：表示这个组件是否已经选中。

主要的事件就是 CheckedChanged 事件。

<img src="https://raw.githubusercontent.com/wqcblog/picgo-image/master/2022/05/20220520_1653009667.gif" alt="4-1Z329154303D1" style="zoom:80%;" />

```cs
public partial class CheckBoxForm : Form
{
    public CheckBoxForm()
    {
        InitializeComponent();
    }
    //单击“确认”按钮，显示选择的爱好
    private void button1_Click(object sender, EventArgs e)
    {
        string msg = "";
        if (checkBox1.Checked)
        {
            msg = msg + " " + checkBox1.Text;
        }
        if (checkBox2.Checked)
        {
            msg = msg + " " + checkBox2.Text;
        }
        if (checkBox3.Checked)
        {
            msg = msg + " " + checkBox3.Text;
        }
        if (checkBox4.Checked)
        {
            msg = msg + " " + checkBox4.Text;
        }
        if (checkBox5.Checked)
        {
            msg = msg + " " + checkBox5.Text;
        }
        if (checkBox6.Checked)
        {
            msg = msg + " " + checkBox6.Text;
        }
        if (checkBox7.Checked)
        {
            msg = msg + " " + checkBox7.Text;
        }
        if(msg != "")
        {
            MessageBox.Show("您选择的爱好是：" + msg, "提示");
        }
        else
        {
            MessageBox.Show("您没有选择爱好", "提示");
        }
    }
}
```

界面上的每一个控件都继承自 Control 类，直接判断界面上的控件是否为复选框即可，实现上述功能的代码可以简化为如下。

```cs
private void button1_Click(object sender, EventArgs e)
{
    string msg = "";
    foreach(Control c in Controls)
    {
        //判断控件是否为复选框控件
        if(c is CheckBox)
        {
            if (((CheckBox)c).Checked)
            {
                msg = msg + " " + ((CheckBox)c).Text;
            }
        }
    }
    if(msg != "")
    {
        MessageBox.Show("您选择的爱好是：" + msg, "提示");
    }
    else
    {
        MessageBox.Show("您没有选择爱好", "提示");
    }
}
```

Control 除了可以在界面上查找复选框以外，还可以查找其他控件。

## CheckedListBox：复选列表框控件

复选列表框显示的效果与复选框类似，但在选择多个选项时操作比一般的复选框更方便。

<img src="https://raw.githubusercontent.com/wqcblog/picgo-image/master/2022/05/20220520_1653009776.gif" alt="4-1Z329161330914" style="zoom:80%;" />

```cs
public partial class CheckedListBox : Form
{
    public CheckedListBox()
    {
        InitializeComponent();
    }
    //“购买”按钮的点击事件，用于在消息框中显示购买的水果种类
    private void button1_Click(object sender, EventArgs e)
    {
        string msg = "";
        for(int i = 0; i < checkedListBox1.CheckedItems.Count; i++)
        {
            msg = msg + " " + checkedListBox1.CheckedItems[i].ToString();
        }
        if (msg != "")
        {
            MessageBox.Show("您购买的水果有：" + msg, "提示");
        }
        else
        {
            MessageBox.Show("您没有选购水果！", "提示");
        }
    }
}
//在使用复选列表框控件时需要注意获取列表中的项使用的是 Checkedltems 属性，获取当前选中的文本使用的是 Selectedltem 属性。
```

## ListBox：列表框控件

列表框 (ListBox) 将所提供的内容以列表的形式显示出来，并可以选择其中的一项或多项内容，从形式上比使用复选框更好一些。

在列表框控件中有一些属性与前面介绍的控件不同，如下表所示。

| 属性名        | 作用                                                         |
| ------------- | ------------------------------------------------------------ |
| MultiColumn   | 获取或设置列表框是否支持多列，如果设置为 True，则表示支持多列； 如果设置为 False，则表示不支持多列，默认为 False |
| Items         | 获取或设置列表框控件中的值                                   |
| SelectedItems | 获取列表框中所有选中项的集合                                 |
| SelectedItem  | 获取列表框中当前选中的项                                     |
| SelectedIndex | 获取列表框中当前选中项的索引，索引从 0 开始                  |
| SelectionMode | 获取或设置列表框中选择的模式，当值为 One 时，代表只能选中一项， 当值为 MultiSimple 时，代表能选择多项，当值为 None 时，代表不能选 择，当值为 MultiExtended 时，代表能选择多项，但要在按下 Shift 键后 再选择列表框中的项 |

列表框还提供了一些方法来操作列表框中的选项，由于列表框中的选项是一个集合形式的，列表项的操作都是用 Items 属性进行的。

例如 `Items.Add` 方法用于向列表框中添加项，`Items.Insert` 方法用于向列表框中的指定位置添加项，`Items.Remove` 方法用于移除列表框中的项。

>提示：ListBox实现多选需要设置窗体的 SelectionMode 属性为 MultiSimple。

```cs
public partial class ListBoxForm : Form
{
    public ListBoxForm()
    {
        InitializeComponent();
    }
    //单击“确定”按钮事件
    private void button1_Click(object sender, EventArgs e)
    {
        string msg = "";
        for(int i = 0; i < listBox1.SelectedItems.Count; i++)
        {
            msg = msg + " " + listBox1.SelectedItems[i].ToString();
        }
        if (msg != "")
        {
            MessageBox.Show("您选择的爱好是：" + msg, "提示");
        }
        else
        {
            MessageBox.Show("您没有选择爱好", "提示");
        }
    }
}
```

```cs
//将列表框中的选中项删除
private void button2_Click(object sender, EventArgs e)
{
    //由于列表框控件中允许多选所以需要循环删除所有已选项
    int count = listBox1.SelectedItems.Count;
    List<string> itemValues = new List<string>();
    if (count != 0)
    {
        for(int i = 0; i < count; i++)
        {
            itemValues.Add(listBox1.SelectedItems[i].ToString());
        }
        foreach(string item in itemValues)
        {
            listBox1.Items.Remove(item);
        }
    }
    else
    {
        MessageBox.Show("请选择需要删除的爱好！");
    }
}
//将文本框中的值添加到列表框中
private void button3_Click(object sender, EventArgs e)
{
    //当文本框中的值不为空时将其添加到列表框中
    if (textBox1.Text != "")
    {
        listBox1.Items.Add(textBox1.Text);
    }
    else
    {
        MessageBox.Show("请添加爱好！");
    }
}
```

在编写删除操作的功能时需要注意，首先要将列表框中的选中项存放到一个集合中, 然后再对该集合中的元素依次使用 Remove 方法移除。

<img src="https://raw.githubusercontent.com/wqcblog/picgo-image/master/2022/05/20220520_1653012478.gif" alt="4-1Z3291P54D49" style="zoom:80%;" />

## ComboBox：组合框控件

组合框（ComboBox）控件也称下拉列表框，用于选择所需的选项，可以有效避免非法值的输入。

在组合框中常用属性，如下表所示。

| 属性名           | 作用                                                         |
| ---------------- | ------------------------------------------------------------ |
| DropDownStyle    | 获取或设置组合框的外观，如果值为 Simple，同时显示文本框和列表框，并且文本框可以编辑；如果值为 DropDown，则只显示文本框，通过鼠标或键盘的单击事件展开文本框，并且文本框可以编辑；如果值为 DropDownList，显示效果与 DropDown 值一样，但文本框不可编辑。默认情况下为 DropDown |
| Items            | 获取或设置组合框中的值                                       |
| Text             | 获取或设置组合框中显示的文本                                 |
| MaxDropDownltems | 获取或设置组合框中最多显示的项数                             |
| Sorted           | 指定是否对组合框列表中的项进行排序，如果值为 True，则排序， 如果值为 False，则不排序。默认情况下为 False |

在组合框中常用的事件是改变组合框中的值时发生的，即组合框中的选项改变事件 SelectedlndexChanged。

此外，在组合框中常用的方法与列表框类似，也是向组合框中添加项、从组合框中删除项。

<img src="https://raw.githubusercontent.com/wqcblog/picgo-image/master/2022/05/20220520_1653012647.gif" alt="4-1Z401094945243" style="zoom:80%;" />

```cs
public partial class ComboBoxForm : Form
{
    public ComboBoxForm()
    {
        InitializeComponent();
    }
    //窗体加载事件，为组合框添加值
    private void ComboBoxForm_Load(object sender, EventArgs e)
    {
        comboBox1.Items.Add("计算机应用");
        comboBox1.Items.Add("英语");
        comboBox1.Items.Add("会计");
        comboBox1.Items.Add("软件工程");
        comboBox1.Items.Add("网络工程");
    }
    //组合框中选项改变的事件
    private void comboBox1_SelectedIndexChanged(object sender, EventArgs e)
    {
        //当组合框中选择的值发生变化时弹出消息框显示当前组合框中选择的值
        MessageBox.Show("您选择的专业是：" + comboBox1.Text, "提示");
    }
    //“添加”按钮的单击事件，用于向组合框中添加文本框中的值
    private void button1_Click(object sender, EventArgs e)
    {
        //判断文本框中是否为空，不为空则将其添加到组合框中
        if (textBox1.Text != "")
        {
            //判断文本框中的值是否与组合框中的的值重复
            if (comboBox1.Items.Contains(textBox1.Text))
            {
                MessageBox.Show("该专业已存在！");
            }
            else
            {
                comboBox1.Items.Add(textBox1.Text);
            }
        }
        else
        {
            MessageBox.Show("请输入专业！", "提示");
        }
    }
    //“删除按钮的单击事件，用于删除文本框中输入的值”
    private void button2_Click(object sender, EventArgs e)
    {
        //判断文本框是否为空
        if (textBox1.Text != "")
        {
            //判断组合框中是否存在文本框中输入的值
            if (comboBox1.Items.Contains(textBox1.Text))
            {
                comboBox1.Items.Remove(textBox1.Text);
            }
            else
            {
                MessageBox.Show("您输入的专业不存在", "提示");
            }
        }
        else
        {
            MessageBox.Show("请输入要删除的专业","提示");
        }
    }
}
```

## PictureBox：图片控件

使用图片控件 ( PictureBox )，图片的设置方式与背景图片的设置方式相似。

图片控件中常用的属性如下表所示。

| 属性名        | 作用                                                         |
| ------------- | ------------------------------------------------------------ |
| Image         | 获取或设置图片控件中显示的图片                               |
| ImageLocation | 获取或设置图片控件中显示图片的路径                           |
| SizeMode      | 获取或设置图片控件中图片显示的大小和位置，如果值为 Normal，则图片显不在控件的左上角；如果值为 Stretchimage，则图片在图片控件中被拉伸或收缩，适合图片的大小；如果值为AutoSize，则控件的大小适合图片的大小；如果值为 Centerimage，图片在图片控件中居中；如果值为 Zoom，则图片会自动缩放至符合图片控件的大小 |

图片控件中图片的设置除了可以直接使用 ImageLocation 属性指定图片路径以外，还可以通过 Image.FromFile 方法来设置。

实现的代码如下。

```cs
图片控件的名称 .Image = Image. FromFile( 图像的路径 );
```

<img src="https://raw.githubusercontent.com/wqcblog/picgo-image/master/2022/05/20220520_1653013233.gif" alt="4-1Z401103G2594" style="zoom:80%;" />

```cs
public partial class PictureBoxForm : Form
{
    public PictureBoxForm()
    {
        InitializeComponent();
    }
    //窗体加载事件，设置图片空间中显示的图片
    private void PictureBoxForm_Load(object sender, EventArgs e)
    {
        pictureBox1.Image = Image.FromFile(@"D:\\C#_test\\111.jpg");
        pictureBox1.SizeMode = PictureBoxSizeMode.StretchImage;
        pictureBox2.Image = Image.FromFile(@"D:\\C#_test\\222.jpg");
        pictureBox2.SizeMode = PictureBoxSizeMode.StretchImage;
    }
    //“交换”按钮的单击事件，用于交换图片
    private void button1_Click(object sender, EventArgs e)
    {
        //定义中间变量存放图片地址，用于交换图片地址
        PictureBox pictureBox = new PictureBox();
        pictureBox.Image = pictureBox1.Image;
        pictureBox1.Image = pictureBox2.Image;
        pictureBox2.Image = pictureBox.Image;
    }
}
```

图片也可以用二进制的形式存放到数据库中，并使用文件流的方式读取数据库中的图片。

通过图片控件的 FromStream 方法来设置使用流读取的图片文件。

## Timer定时器控件

定时器控件（Timer）与其他的控件略有不同，它并不直接显示在窗体上，而是与其他控件连用，表示每隔一段时间执行一次 Tick 事件。

定时器控件中常用的属性是 Interval，用于设置时间间隔，以毫秒为单位。

此外，在使用定时器控件时还会用到启动定时器的方法（Start）、停止定时器的方法（Stop）。

<img src="https://raw.githubusercontent.com/wqcblog/picgo-image/master/2022/05/20220520_1653013502.gif" alt="4-1Z4011141513S" style="zoom:80%;" />

```cs
public partial class TimerForm : Form
{
    //设置当前图片空间中显示的图片
    //如果是 Timer1.jpg   flag的值为FALSE
    //如果是 Timer2.jpg   flag的值为TRUE
    bool flag = false;
    public TimerForm()
    {
        InitializeComponent();
    }
    //窗体加载事件，在图片空间中设置图片
    private void TimerForm_Load(object sender, EventArgs e)
    {
        pictureBox1.Image = Image.FromFile(@"D:\C#_test\Timer1.jpg");
        pictureBox1.SizeMode = PictureBoxSizeMode.StretchImage;
        //设置每隔1秒调用一次定时器Tick事件
        timer1.Interval = 1000;
        //启动定时器
        timer1.Start();
    }
    //触发定时器的事件，在该事件中切换图片
    private void timer1_Tick(object sender, EventArgs e)
    {
        //当flag的值为TRUE时将图片控件的Image属性切换到Timer1.jpg
        //否则将图片的Image属性切换到Timer2.jpg
        if (flag)
        {
            pictureBox1.Image = Image.FromFile(@"D:\C#_test\Timer1.jpg");
            flag = false;
        }
        else
        {
            pictureBox1.Image = Image.FromFile(@"D:\C#_test\Timer2.jpg");
            flag = true;
        }
    }
    //“启动定时器”按钮的单击事件
    private void button1_Click(object sender, EventArgs e)
    {
        timer1.Start();
    }
    //“停止定时器”按钮的单击事件
    private void button2_Click(object sender, EventArgs e)
    {
        timer1.Stop();
    }
}
```

## DateTimePicker：日期时间控件

日期时间控件（DateTimePicker）在时间控件中的应用最多，主要用于在界面上显示当前的时间。

日期时间控件中常用的属性是设置其日期显示格式的 Format 属性。

Format 属性提供了 4 个属性值，如下所示。

- Short：短日期格式，例如2017/3/1；
- Long：长日期格式，例如2017年3月1日；
- Time：仅显示时间，例如，22:00:01；
- Custom：用户自定义的显示格式。

如果将 Format 属性设置为 Custom 值，则需要通过设置 CustomFormat 属性值来自定义显示日期时间的格式。

![4-1Z401134613152](https://raw.githubusercontent.com/wqcblog/picgo-image/master/2022/05/20220520_1653014519.gif)

```cs
public partial class DateTimePickerForm : Form
{
    public DateTimePickerForm()
    {
        InitializeComponent();
    }
    //DateTimePickerForm窗体加载事件
    private void DateTimePickerForm_Load(object sender, EventArgs e)
    {
        //设置日期时间控件中仅显示时间
        dateTimePicker1.Format = DateTimePickerFormat.Time;
        //设置每隔一秒调用一次定时器Tick事件
        timer1.Interval = 1000;
        //启动定时器
        timer1.Start();
    }
    private void timer1_Tick(object sender, EventArgs e)
    {
        //重新设置日期时间控件的文本
        dateTimePicker1.ResetText();
    }
}
```

## MonthCalendar：日历控件

日历控件（MonthCalendar）用于显示日期，通常是与文本框联用，将日期控件中选择的日期添加到文本框中。

<img src="https://raw.githubusercontent.com/wqcblog/picgo-image/master/2022/05/20220520_1653014661.gif" alt="4-1Z401143H94V" style="zoom:80%;" />

```cs
public partial class MonthCalendarForm : Form
{
    public MonthCalendarForm()
    {
        InitializeComponent();
    }
    //窗体加载事件
    private void MonthCalendarForm_Load(object sender, EventArgs e)
    {
        //隐藏日历控件
        monthCalendar1.Hide();
    }
    //“选择”按钮的单击事件
    private void button1_Click(object sender, EventArgs e)
    {
        //显示日历控件
        monthCalendar1.Show();
    }
    //日历控件的日期改变事件
    private void monthCalendar1_DateSelected(object sender, DateRangeEventArgs e)
    {
        //将选择的日期显示在文本框中
        textBox1.Text = monthCalendar1.SelectionStart.ToShortDateString();
        //隐藏日历控件
        monthCalendar1.Hide();
    }
}
```

## ContextMenuStrip：右键菜单控件（上下文菜单）

右键菜单又叫上下文菜单，即右击某个控件或窗体时出现的菜单。

<img src="https://raw.githubusercontent.com/wqcblog/picgo-image/master/2022/05/20220520_1653014990.gif" alt="4-1Z40115053T50" style="zoom:80%;" />

```cs
public partial class ContextMenuStrip : Form
{
    public ContextMenuStrip()
    {
        InitializeComponent();
    }
    //打开新窗体的菜单项单击事件
    private void 打开窗体ToolStripMenuItem_Click(object sender, EventArgs e)
    {
        ContextMenuStrip menu1 = new ContextMenuStrip();
        menu1.Show();
    }
    //关闭窗体菜单项的单击事件
    private void 关闭窗体ToolStripMenuItem_Click(object sender, EventArgs e)
    {
        this.Close();
    }
}
```

## MenuStrip：菜单栏控件

完成 MenuStrip 控件的添加后，在 Windows 窗体设计界面中就能看到“请在此处键入” 选项，直接单击它，然后输入菜单的名称，例如，“文件”“编辑”“视图”等。

此外，添加一级菜单后还能添加二级菜单，例如，为“文件”菜单添加“新建”“打开”“关闭”等二级菜单，如下图所示，模拟一个文件菜单（包括二级菜单）和编辑菜单。

## StatusStrip：状态栏菜单控件

界面中新添加的状态栏控件，则会显示如下图所示的下拉菜单，其中包括标签控件（StatusLabel）、进度条（ProgressBar）、下拉列表按钮（DropDownButton）、分割按钮（SplitButton）。

在状态栏上不能直接编辑文字，需要添加其他的控件来辅助。

## ToolStrip：工具栏控件

在添加了 ToolStrip 控件之后，它只是一个工具条，上面并没有控件，所以它不能响应 一些事件，从而没有功能。

我们可以把它理解成一个占位符，就像是占着一个区域的位置，然后在其上面再添加按钮。

## MDI窗体

**在一个窗体中打开另一个窗体**的方式可以通过设置 MDI 窗体的方式实现。

MDI (Multiple Document Interface) 窗体被称为多文档窗体，它是很多 Windows 应用程序中常用的界面设计。

MDI 窗体的设置并不复杂，只需要将窗体的属性 IsMdiContainer 设置为 True 即可。

该属性既可以在 Windows 窗体的属性窗口中设置，也可以通过代码设置，这里在窗体加载事件 Load 中设置窗体为 MDI 窗体，代码如下。

```cs
this.IsMdiContainer = True;
```

在 MDI 窗体中，弹出窗体的代码与直接弹出窗体有些不同，在使用 Show 方法显示窗体前需要使用窗体的 MdiParent 设置显示当前窗体的父窗体，实现的代码如下。

```cs
Test t = new Test();
t.MdiParent = this;//this代表当前窗体
t.Show();
```

```cs
public partial class MDIForm : Form
{
    public MDIForm()
    {
        InitializeComponent();
        this.IsMdiContainer = true;
    }
    //打开文件菜单项的单击事件
    private void 打开文件ToolStripMenuItem_Click(object sender, EventArgs e)
    {
        OpenFile f = new OpenFile();
        f.MdiParent = this;
        f.Show();
    }
    //保存文件菜单项单击事件
    private void 保存文件ToolStripMenuItem_Click(object sender, EventArgs e)
    {
        SaveFile f = new SaveFile();
        f.MdiParent = this;
        f.Show();
    }
}
```

<img src="https://raw.githubusercontent.com/wqcblog/picgo-image/master/2022/05/20220520_1653016062.gif" alt="4-1Z401164955622" style="zoom:80%;" />

## ColorDialog：颜色对话框控件

颜色对话框控件（ColorDialog）用于对界面中的文字设置颜色。

在使用颜色对话框时不会在窗体中直接显示该控件，需要通过事件调用该控件的 ShowDialog 方法显示对话框。

```cs
public partial class ColorDialogForm : Form
{
    public ColorDialogForm()
    {
        InitializeComponent();
    }
    //“更改文本颜色”按钮的单击事件
    private void button1_Click(object sender, EventArgs e)
    {
        //显示颜色对话框
        DialogResult dr = colorDialog1.ShowDialog(); //DialogResult以对话框模式显示窗体
        //如果选中颜色，单击“确定”按钮则改变文本框的文本颜色
        if (dr == DialogResult.OK) //DialogResult作为一个枚举类型
        {
            textBox1.ForeColor = colorDialog1.Color;
        }
    }
}
```

<img src="https://raw.githubusercontent.com/wqcblog/picgo-image/master/2022/05/20220520_1653016026.gif" alt="4-1Z4011J224118" style="zoom:80%;" />

## FontDialog：字体对话框控件

字体对话框 (FontDialog) 用于设置在界面上显示的字体，与 Word 中设置字体的效果类似，能够设置字体的大小以及显示的字体样式等。

<img src="https://raw.githubusercontent.com/wqcblog/picgo-image/master/2022/05/20220520_1653016522.gif" alt="4-1Z402091G3146" style="zoom:80%;" />

```cs
public partial class FontDialogForm : Form
{
    public FontDialogForm()
    {
        InitializeComponent();
    }
    //“改变文本字体”按钮的单击事件
    private void button1_Click(object sender, EventArgs e)
    {
        //显示字体对话框
        DialogResult dr = fontDialog1.ShowDialog();
        //如果在对话框中单击“确认”按钮，则更改文本框中的字体
        if (dr == DialogResult.OK)
        {
            textBox1.Font = fontDialog1.Font;
        }
    }
}
```

## OpenFileDialog和SaveFileDialog：打开文件与保存文件

文件对话框（FileDialog）主要包括文件浏览对话框，以及用于查找、打开、保存文件的功能，与 Windows 中的文件对话框类似。

<img src="https://raw.githubusercontent.com/wqcblog/picgo-image/master/2022/05/20220520_1653017046.gif" alt="4-1Z402101K1946" style="zoom:80%;" />

```cs
public partial class FileDialogForm : Form
{
    public FileDialogForm()
    {
        InitializeComponent();
    }
    //打开文件
    private void button1_Click(object sender, EventArgs e)
    {
        DialogResult dr = openFileDialog1.ShowDialog();
        //获取所打开文件的文件名
        string filename = openFileDialog1.FileName;
        if(dr==System.Windows.Forms.DialogResult.OK && !string.IsNullOrEmpty(filename))
        {
            StreamReader sr = new StreamReader(filename);
            textBox1.Text = sr.ReadToEnd();
            sr.Close();
        }
    }
    //保存文件
    private void button2_Click(object sender, EventArgs e)
    {
        DialogResult dr = saveFileDialog1.ShowDialog();
        string filename = saveFileDialog1.FileName;
        if(dr==System.Windows.Forms.DialogResult.OK && !string.IsNullOrEmpty(filename))
        {
            StreamWriter sw = new StreamWriter(filename, true, Encoding.UTF8);
            sw.Write(textBox1.Text);
            sw.Close();
        }
    }
}
```

## RichTextBox：富文本框控件

读取文本信息时需要保留原有的文本格式，这时候就不能使用普通的文本控件 (TextBox) 了，而需要使用富文本框控件 (RichTextBox) 来完成。

RichTextBox 控件在使用时与 TextBox 控件是非常类似的，但其对于读取多行文本更有优势，它可以处理特殊格式的文本。

此外，在 RichTextBox 控件中还提供了文件加载和保存的方法，不需要使用文件流即可完成对文件的读写操作。

```cs
public partial class FileDialogForm : Form
{
    public FileDialogForm()
    {
        InitializeComponent();
    }
    //打开文件
    private void button1_Click(object sender, EventArgs e)
    {
        DialogResult dr = openFileDialog1.ShowDialog();
        //获取打开文件的文件名
        string filename = openFileDialog1.FileName;
        if(dr==System.Windows.Forms.DialogResult.OK && !string.IsNullOrEmpty(filename))
        {
            richTextBox1.LoadFile(filename, RichTextBoxStreamType.PlainText);
        }
    }
    //保存文件
    private void button2_Click(object sender, EventArgs e)
    {
        DialogResult dr = saveFileDialog1.ShowDialog();
        //获取所保存文件的文件名
        string filename = saveFileDialog1.FileName;
        if(dr==System.Windows.Forms.DialogResult.OK && !string.IsNullOrEmpty(filename))
        {
            richTextBox1.SaveFile(filename, RichTextBoxStreamType.PlainText);
        }
    }
}
```

