---
title: Java程序设计精讲十 第九章 Swing组件
author: mumu
date: 2023-03-25 18:15:00 +0800
categories: [sunlands,Java程序设计精讲十 第九章 Swing组件]
tags: [sunlands,Java程序设计]
pin: false
---

# Java程序设计精讲十 第九章 Swing组件[^1]

## 1 组合框与列表

### 1.1. 组合框

#### 1.1.1. 定义

组合框(JComboBox)是一个下拉式菜单，它有两种形式∶不可编辑的和可编辑的。

默认是不可编辑的，可以通过<font color='red' style='background-color:' size=''>setEditable (true)</font>方法将其设置为可编辑的。

![image-20230325182151109](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/20230325182151.png)

#### 1.1.2. 构造方法

- JComboBox( ):创建一个没有任何可选项的默认组合框。
- JComboBox(Object [ ] items):根据Object 数组创建组合框，Object数组的元素即为组合框中的可选项。

#### 1.1.3. 方法

- void addltem(Object anObject):在末尾位置添加新的可选项。
- object getltemAt(int index):返回指定索引序号index处的可选项。
- int getltemCount():返回列表中的项数。
- void insertltemAt(Object anObject , int index):在index指定的位置添加新的可选项anObject。
- int getSelectedIlndex():返回列表中与给定项匹配的第一个选项的索引序号。
- Object getSelectedltem():返回当前所选项。
- void removeAllltems():删除所有可选项。
- void removeltem(Object anObject):删除由anObject指定的可选项。
- void removeltemAt(int anIndex):删除由anIndex指定处的可选项。

#### 1.1.4. 事件

- ActionListener:用户输入项目后按〈Enter〉键，对应的接口
- ltemListener:用户选定项目，对应的接口。
- 不过用户的一次选择操作，会引发两个ltemEvent事件，因此通常使用ActionListener处理比较方便。

### 1.2. 列表

#### 1.2.1. 定义

列表(JList)是可供用户进行选择的一系列可选项。

#### 1.2.2. 构造方法

- JList():构造一个空列表。
- JList(Object [ ] listData):构造一个列表，列表的可选项由对象数组listData指定。
- JList( Vector <?> listData):构造一个列表，使其显示指定Vector中的元素。

#### 1.2.3. 方法

- public int getSelectedIndex( )︰返回所选项第一次出现的索引;如果没有所选项，则返回-1。
- public Object getSelectedValue( ):返回所选的第一个值，如果选择为空，则返回null。
- public void setVisibleRowCount(int visibleRowCount):设置不使用滚动条可以在列表中显示的首选行数。
- setModel (ListModel model):设置新的ListModel。

#### 1.2.4. 事件

> ListSelectionEvent ：当用户在列表上进行选择时将引发此事件。

在JList中提供了addListSelectionListener (ListSelectionListener listener)方法，用于注册对应的事件侦听程序。在ListSelectionListener接口中，只包含一个方法:

- public void valueChanged( ListSelectionEvent e);当列表的当前选项发生变化时，将会调用该方法。

#### 1.2.5. 滚动功能

列表对象本身并不带滚动条，但是当列表可选项较多时，可以将列表对象放入JScrollPane中以提供滚动功能。

#### 1.2.6. 支持单项选择和多项选择

使用List中定义的`setSelectionMode (int selectionMode)`设置，其中int型参数selectionMode可以是下列常量∶

- ListSelectionModel.SINGLE_SELECTION:只能进行单项选择。
- ListSelectionModeL.SINGLE_INTERVAL_SELECTION:可多项选择，但多个选项必须是连续的。
- ListSelectionModel.MULTIPLE_INTERVAL_SELECTION:<font color='red' style='background-color:' size=''>可多项选择，多个选项可以是间断的</font>，这是选择模式的默认值。

## 2 文本组件

> 在Swing中的文本组件:
>
> - 文本域(JTextField) ;
> - 口令输入域(JPasswordField) ;
> - 文本区(JTextArea)等。
>
> 文本组件父类:<font color='red' style='background-color:' size=''>JTextComponent</font>

### 2.1. JTextComponent

#### 2.1.1. JTextComponent所共有的一些方法︰

- String getSelectedText():从文本组件中提取被选中的文本内容。
- String getText():从文本组件中提取所有文本内容。
- String getText(int offs, int len):从文本组件中提取指定范围的文本内容。
- void select(int selectionStart , int selectionEnd):在文本组件中选中指定的起始和结束位置之间的文本内容。
- void selectAll():在文本组件中选中所有文本内容。
- void SetEditable(boolean b);设置为可编辑或不可编辑状态。
- void setText(String t):设置文本组件中的文本内容。
- void setDocument(Document doc):设置文本组件的文档。
- void copy():复制选中的文本到剪贴板。
- void cut():剪切选中的文本到剪贴板。
- void paste():将剪贴板的内容粘贴到当前位置。

JComponent类中常用方法︰

- public boolean requestFocuslnWindow( ):请求当前组件获得输入焦点.

### 2.2. 文本域

> <font color='red' style='background-color:' size=''>文本域</font>:是一个<font color='red' style='background-color:' size=''>单行</font>>的文本输入框，可用于输入少量文本。

#### 2.2.1. 常用的构造方法

- JTextField():构造一个空文本域。
- JTextField( int columns):构造一个具有指定列数的空文本域，int型参数columns指定文本域的列数。
- JTextField( String text):构造一个显示指定初始字符串的又本域，String型参数text指定要显示的初始字符串。
- JTextField( String text, int columns):构造一个具有指定列数并显示指定初始字符串的文本域，String型参数text指定要显示的初始字符串，int型参数columns指定文本域的列数。

#### 2.2.2. 常用方法

- void addActionListener (ActionListener l):添加指定的操作侦听器，接收操作事件。

- void removeActionListener (ActionListener l):移除指定的操作侦听器，不再接收操作事件。

- void setFont (Font f):设置当前字体。

- void setHorizontalAlignment (int alignment):设置文本的水平对齐方式。

  有效值包括：JTextField.LEFT、JTextField. CENTER、JTextField. RIGHT、JTextField.LEADING和JTextField.TRAILING。

+ int getColumns ():返回文本域的列数。

### 2.3. 文本区

> <font color='red' style='background-color:' size=''>文本区</font>∶是一个<font color='red' style='background-color:' size=''>多行多列</font>的文本输入框。

#### 2.3.1. 常用的构造方法

- JTextArea():构造一个空文本区。
- JTextArea( String text):构造一个显示指定初始字符串的文本区，String型参数text指定要显示的初始字符串。
- JTextArea(int rows, int columns):构造一个具有指定行数和列数的空文本，int型参数rows和columns分别指定文本区的行数和列数。
- JTextArea(String text, int rows,int columns):构造一个具有指定行数和列数并显示指定初始字符串的文本区，String型参数text指定要显示的初始字符串，int型参数rows和columns指定文本区的行数和列数。

#### 2.3.2. 方法

除了前面提到的JTextComponent类中的常用方法外一还有:

- void append( String str):将指定文本str追加到文本区。
- void insert( String str , int pos):将指定文本str插入到文本区的特定位置pos处。
- void replaceRange( String str , int start , int end):用指定文本str替换文本区中从起始位置start到结尾位置end的内容。

> Tips:tipping_hand_man:：
>
> 1. 与文本域类似，构造方法中指定的行数和列数只是希望的数值，文本区的大小仍然是由布局管理器决定的。
>    文本区本身不带滚动条，由于文本区内显示的内容通常比较多，因此一般将其放入滚动窗格JScrollPane中。
>
>    ```java
>    JTextArea ta = new JTextArea( );
>    JScrollPane jsp = new JScrollPane( ta) ;//给文本区添加滚动条
>    ```

#### 2.3.3. 事件

可以为文本区注册普通的事件侦听程序，当需要识别用户“输入完成”时，通常要在文本区旁放置一个“Apply”或“Commit”之类的按钮。

> 可以为文本区注册普通的事件侦听程序，但是由于文本区中可输入的文本是多行的，用户按〈Enter〉键的结果只是向缓冲区输入一个字符，并不能表示输入的结束。因此，当需要识别用户“输入完成”时，通常要在文本区旁放置一个“Apply”或“Commit”之类的按钮。

程序9.3文本域及文本区示例

程序9.3创建了两个文本区和两个按钮，当用户单击“Copy”按钮时，第一个文本区中选中的内容（或全部内容，选中的内容用红色表示）将被添加到第二个文本区中;当用户单击“Clear”按钮时，第二个文本区中的内容将被清空。用户可在第一个文本区中进行编辑，但第二个文本区被设置为不可编辑的，不能进行输人，只能用来显示信息。

## 3 菜单组件

> 菜单也是最常用的GUI组件之一，Swing 包中提供了多种菜单组件，包括`JMenuBar ,JMenultem、JMenu、JCheckBoxMenultem、JRadioButtonMenultem和 JPopupMenu`等。菜单有下拉式菜单和弹出式菜单两种，本节介绍下拉式菜单，这是最常用的一类菜单。

### 3.1. 菜单栏及菜单

#### 3.1.1. 概念

菜单栏是窗口中的主菜单，用来包容一组菜单。菜单是最基本的下拉菜单用来包容一组菜单项或子菜单。

#### 3.1.2. 菜单栏的构造方法(只有一种)——<font color='red' style='background-color:' size=''>JMenuBar()</font>

> 菜单栏只有一种构造方法，即JMenuBar()。JFrame、JApplet和JDialog等类中都定义了setJMenuBar(JMenuBar menu)方法，可以把菜单栏放到窗口的上方，

```java
JFrame frame = new JFrame("Menu Demo”);//菜单窗口标题是”Menu Demo"
JMenuBar mb = new JMenuBar( );//创建菜单栏
frame.setJMenuBar(mb);//放到框架的上方
```

JMenuBar上也可以注册一些事件侦听程序，但通常情况下，对于JMenuBar 上的用户事件，都不进行处理。

#### 3.1.3. 菜单的构造方法

- JMenu( ):构造没有文本的新菜单。
- JMenu(String s):构造具有指定标签的新菜单，String型参数s指定了菜单上的文本。
- JMenu(Strings, boolean b):构造具有指定标签的新菜单，指示该菜单是否可以分离。

***示例：***

```java
// 创建两个菜单的示例:
JMenuBar menubar = new JMenuBar( );//创建菜单栏
JMenu menu1= new JMenu("File”);//创建菜单File
JMenu menu2 = new JMenu("Edit"”);//创建菜单Edit
menubar. add( menu1 );//加入到菜单中
menubar. add (menu2);
```

### 3.2. 菜单项

> 如果将整个菜单系统看作是一棵树，那么菜单项就是这棵树的叶子，是菜单系统的最下面一级。常用的菜单项构造方法有以下几种。
>
> 菜单项中还可以含有菜单项,组成嵌套的菜单。

#### 3.2.1. 菜单项的构造方法

- JMenultem():创建不带有设置文本或图标的菜单项。
- JMenultem(lcon icon):创建一个只显示图标的菜单项，图标由lcon型参数icon指定。
- JMenultem(String text):创建一个只显示文本的菜单项，文本由String型参数text指定。
- JMenultem(String text,Icon icon):创建一个同时显示文本和图标的菜单项，文本由String型参数text指定，图标由lcon型参数icon指定。
- JMenultem(String text, int mnemonic):创建一个显示文本并且有快捷键的菜单项，文本由String型参数text指定，快捷键由int型参数mnemonic指定。

快捷键也可以在菜单项被创建之后，通过setMnemonic ( char mnemonic)方法进行设置。在类中还定义了一个 setAccelerator (KeyStroke keyStroke）方法，使用该方法可以为菜单项设置加速键，例如，下面命令首先创建了一个菜单项，然后为其设置快捷键和加速键。

**分隔线：**

```java
JMenultem menultem = new JMenultem(" Open...");
menultem.setMnemonic( KeyEvent. VK_O);
menultem.setAccelerator(KeyStroke.getKeyStroke(KeyEvent.VK_1,ActionEvent. ALT_MASK) );
```

在Menu类中定义有addSeparator()和 insertSeparator( int index)方法，通过这些方法，可以在某个菜单的各个菜单项间加入分隔线。此外，在javax. swing 包中还定义了一个JSeparator类,也可以通过如下命令在菜单项间加入分隔线。

```java
menu1.addSpeparator();
menu1.add(new JSeparator());
```

#### 3.2.2. 方法

- setMnemonic (char mnemonic):为菜单设置快捷键。
- setAccelerator (KeyStroke keystroke):为菜单项设置加速键。

#### 3.2.3. 事件

当菜单中的菜单项被选中时，将会引发一个ActionEvent事件，因此通常需要为菜单项注册ActionListener，以便对事件做出响应。
```java
menultem.addActionListener(this);//注册侦听器
```

### 3.3. 复选菜单项和单选菜单项

![image-20230327163513967](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/202303271635103.png)

#### 3.3.1. 复选菜单项的构造方法

- JCheckBoxMenultem():创建一个未设置文本或图标、最初也未选定的复选框菜单项。
- JCheckBoxMenultem (lcon icon ):创建一个有图标、最初未被选定的复选框菜单项。
- JCheckBoxMenultem(String text):创建一个带文本、最初未被选定的复选框菜单项。
- JCheckBoxMenultem(String text , boolean b):创建具有指定文本和选择状态的复选框菜单项。
- JCheckBoxMenultem(String text,Icon icon):创建具有指定文本和图标、最初未被选定的复选框菜单项。
- JCheckBoxiMenultem(String text, lcon icon, boolean b):创建具有指定文本、图标和选择状态的复选框菜单项。

```java
JCheckBoxMenuItem mil = new JCheckBoxMenultem ("Persistent");//显示Persistent、初态为未选中
JCheckBoxMenultem mi2 = new JCheckBoxMenultem (“transient”，true);//显示 transient、初态为选中
```

#### 3.3.2. 单选菜单项的构造方法

- JRadioButtonMenultem():创建一个未设置文本或图标的单选按钮菜单项。
- JRadioButtonMenultem( lcon icon):创建一个带图标的单选按钮菜单项。
- JRadioButtonMenultem(lcon icon , boolean selected):创建一个具有指定图标和选择状态的单选按钮菜单项，但无文本。
- JRadioButtonMenultem(String text):创建一个带文本的单选按钮菜单项。
- JRadioButtonMenultem(String text,boolean selected);:创建一个具有指定文本和选择状态的单选按钮菜单项。
- JRadioButtonMenultem(String text , lcon icon):创建一个具有指定文本和图标的单选按钮菜单项。
- JRadioButtonMenultem(String text, lcon icon, boolean selected):创建一个具有指定的文本、图标和选择状态的单选按钮菜单项。

#### 3.3.3. 事件

当菜单项的状态发生改变时，会引发 ItemEvent 事件，可以使用ItemListener中的 item-StateChanged()对此事件进行响应。

#### 3.3.4. 建立菜单系统过程

首先创建一个菜单栏，并通过<font color='red' style='background-color:' size=''>setMenuBar()</font>方法将其放入某个框架中。

然后创建若干个菜单，通过add()方法将它们加入菜单栏中。
最后创建各个菜单项，通过add()方法将它们加入不同的菜单中。

## 4 对话框

### 4.1. 对话框

#### 4.1.1. 概念

对话框是一个临时的可移动窗口，且要依赖于其他窗口，当它所依赖的窗口消失或最小化时，对话框也将消失。当窗口还原时，对话框会自动恢复。一般地，要先创建一个窗口类，再创建一个对话框类,且让对话框依附于窗口。
对话框分为强制型和非强制型两种。强制型对话框被关闭之前，其他窗口无法接收任何形式的输入，也就是该对话过程不能中断，这样的窗口也称为模式窗口。非强制型对话框可以中断对话过程，去响应对话框之外的事件。

#### 4.1.2. 构造方法

> Dialog型或Frame型的参数指定了对话框的拥有者，也就是它的依赖窗口

- JDialog( Dialog owner):创建一个没有标题但将指定的对话框作为其所有者的无模式对话框。
- JDialog( Dialog owner,boolean modal):创建一个没有标题但有指定所有者的对话框，boolean型参数modal指定对话框是有模式或无模式。
- JDialog( Dialog owner,String title):创建一个具有指定标题和指定所有者的无模式对话框。
- JDialog( Dialog owner ,String title,boolean modal):创建一个具有指定标题和指定所有者的对话框，boolean型参数modal指定对话框是有模式或无模式。
- JDialog( Frame owner):创建一个没有标题但将指定的框架作为其所有者的无模式对话框。
- JDialog( Frame owner,boolean modal):创建一个没有标题但有指定所有者的对话框，boolean型参数modal指定对话框是有模式或无模式。
- JDialog( Frame owner, String title):创建一个具有指定标题和指定所有者框架的无模式对话框。
- JDialog( Frame owner,String title , boolean modal):创建一个具有指定标题和指定所有者框架的对话框，boolean型参数modal指定对话框是有模式或无模式。

```java
JDialog dialog = new JDialog(frame,"Dialog" ,true);//例如︰创建一个标题为“Dialog”的模式对话框，该对话框为框架frame所拥有。
```

#### 4.1.3. 方法

- setVisible(boolean b):将对话框 显示/隐藏 出来。

对话框可对各种窗口事件进行侦听，例如激活窗口和关闭窗口等。与框架类似，对话框也是顶层容器,可以向对话框的内容窗格中添加各种组件。

### 4.2. 标准对话框

> JDialog类通常用于创建自定义的对话框，除此之外，在Swing中还提供了用于显示标准对话框的JOptionPane类。

在JOptionPane类中定义了多个showXxxDialog形式的静态方法，可以分为以下4种类型：

| showConfirmDialog | 确认对话框，显示问题，要求用户进行确认 (yes/no/ cancel)。 |
| ----------------- | --------------------------------------------------------- |
| showInputDialog   | 输入对话框，提示用户进行输入。                            |
| showMessageDialog | 信息对话框，显示信息，告知用户发生了什么情况。            |
| showOptionDialog  | 选项对话框，显示选项，要求用户进行选择。                  |

除了showOptionDialog 之外，其他3种方法都定义有若干个不同格式的同名方法，

例如 `showMessageDialog`有3个同名方法。

- showMessageDialog( Component parentComponent，Object message) 。
- showMessageDialog( Component parentComponent，Object message，String title，int messageType)。
- showMessageDialog ( Component parentComponent，0bject message，String title，int messageType，Icon icon)。

这些形如showXxxDialog方法的参数大同小异，不外平以下几种类型。

- Component parentComponent:对话框的父窗口对象，其屏幕坐标将决定对话框的显示位置;此参数也可以为null，表示采用默认的Frame作为父窗口，此时对话框将设置在屏幕的正中。

- String title:对话框的标题。

- Object message:显示在对话框中的描述信息。该参数通常是一个 String对象，但也可以是一个图标、一个组件或者一个对象数组。

- int optionType:对话框上按钮的类型，可以为以下常量：

  - DEFAULT_OPTION
  - YES_NO_OPTION
  - YES_NO_CANCEL_OPTIONOK_CANCEL_OPTION

- int messageType:对话框所传递的信息类型。

  可以为以下常量：

  - ERROR_MESSAGE 
  - INFORMATION_MESSAGE 
  - WARNINGMESSAGE
  - QUESTION_MESSAGE
  - PLAIN_MESSAGE

  除 PLAIN_MESSAGE之外，其他每种类型都对应于一个默认图标。

  - ![image-20230328151916689](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/202303281519791.png)

除此之外,也可以通过`options`参数指定其他形式。

- Object[] options:对话框上的选项。在输人对话框中,通常以组合框形式显示。在选项对话框中,则是指按钮的选项类型。该参数通常是一个String 数组,也可以是图标或组件数组。
- lcon icon:对话框上显示的装饰性图标,如果没有指定,则根据messageType参数显示默认图标。
- Obiect initialValue:初始选项或输入值。

### 4.3. 文件对话框

> 文件对话框:专门用于对文件(或目录)进行浏览和选择的对话框。
>
> ![image-20230328153504888](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/202303281536446.png)

#### 4.3.1. 构造方法

- JFileChooser():构造一个指向用户默认目录的文件对话框。
- JFileChooser(File currentDirectory):使用给定的File作为路径来构造一个文件对话框。
- JFileChooser(String currentDirectoryPath):构造一个使用给定路径的文件对话框

#### 4.3.2. 方法(显示）

> 刚刚创建的文件对话框是不可见的，可以调用以下方法将其显示出来。

- showOpenDialog (Component parent):弹出一个“打开”文件对话框。
- showSaveDialog( Component parent):弹出一个“保存”文件对话框。

Component型参数指定文件对话框的“父组件”。且决定了文件对话框的显示位置，如果该参数为null，则文件对话框显示在屏幕正中。

#### 4.3.3. 事件

对于文件对话框中的事件，一般都无需进行处理。当用户进行文件选择之后,可以通过<font color='red' style='background-color:' size=''>`getSelectedFile()`</font>方法取得用户所选择的文件。



[^1]: [尚德机构](https://xt.shuhanfenglin.com/)