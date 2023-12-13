---
title: HarmonyOS开发 从简单的页面开始
author: mumu
date: 2023-12-12 16:00:00 +0800
categories: [华为,HarmonyOS]
tags: [华为,HarmonyOS,从简单的页面开始]
pin: false
---

# 从简单的页面开始

## 常用基础组件

### 1 组件介绍

组件（Component）是界面搭建与显示的最小单位，HarmonyOS ArkUI声明式开发范式为开发者提供了丰富多样的UI组件，我们可以使用这些组件轻松的编写出更加丰富、漂亮的界面。

组件根据功能可以分为以下五大类：<font color='red' style='background-color:' size=''>基础组件、容器组件、媒体组件、绘制组件、画布组件</font>。其中<font color='cornflowerblue' style='background-color:' size=''>基础组件是视图层的基本组成单元，包括Text、Image、TextInput、Button、LoadingProgress等</font>，例如下面这个常用的登录界面就是由这些基础组件组合而成。

![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131034284.png)

下面我们将分别介绍这些常用基础组件的使用。

### 2 Text

<font color='cornflowerblue' style='background-color:' size=''>Text组件用于在界面上展示一段文本信息，可以包含子组件Span</font>。

#### 文本样式

针对包含文本元素的组件，例如<font color='cornflowerblue' style='background-color:' size=''>Text、Span、Button、TextInput</font>等，可使用<font color='cornflowerblue' style='background-color:' size=''>fontColor、fontSize、fontStyle、 fontWeight、fontFamily</font>这些文本样式，分别设置文本的颜色、大小、样式、粗细以及字体，文本样式的属性如下表：

| 名称       | 参数类型                       | 描述                                                         |
| ---------- | ------------------------------ | ------------------------------------------------------------ |
| fontColor  | ResourceColor                  | 设置文本颜色。                                               |
| fontSize   | Length \| Resource             | 设置文本尺寸，Length为number类型时，使用fp单位。             |
| fontStyle  | FontStyle                      | 设置文本的字体样式。默认值：FontStyle.Normal。               |
| fontWeight | number \| FontWeight \| string | 设置文本的字体粗细，number类型取值[100, 900]，取值间隔为100，默认为400，取值越大，字体越粗。string类型仅支持number类型取值的字符串形式，例如“400”，以及“bold”、“bolder”、“lighter”、“regular”、“medium”，分别对应FontWeight中相应的枚举值。默认值：FontWeight.Normal。 |
| fontFamily | string \| Resource             | 设置文本的字体列表。使用多个字体，使用“，”进行分割，优先级按顺序生效。例如：“Arial，sans-serif”。 |

下面示例代码中包含两个Text组件，第一个使用的是默认样式，第二个给文本设置了一些文本样式。

```typescript
@Entry
@Component
struct TextDemo {
  build() {
    Row() {
      Column() {
        Text('HarmonyOS')
        Text('HarmonyOS')
          .fontColor(Color.Blue)
          .fontSize(20)
          .fontStyle(FontStyle.Italic)
          .fontWeight(FontWeight.Bold)
          .fontFamily('Arial')
      }
      .width('100%')
    }
    .backgroundColor(0xF1F3F5)
    .height('100%')
  }
}
```

效果图如下：

![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/10/202312121618666.png)

除了通用属性和文本样式设置，下面列举了一些Text组件的常用属性的使用。

#### 设置文本对齐方式

使用textAlign属性可以设置文本的对齐方式，示例代码如下：

```typescript
Text('HarmonyOS')
  .width(200)
  .textAlign(TextAlign.Start)
  .backgroundColor(0xE6F2FD)
```

<font color='red' style='background-color:' size=''>textAlign参数类型为TextAlign</font>，定义了以下几种类型：

- <font color='red' style='background-color:' size=''>Start</font>（默认值）：水平对齐首部。

  ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/10/202312121619097.png)

- <font color='red' style='background-color:' size=''>Center</font>：水平居中对齐。

  ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/10/202312121612567.png)

- <font color='red' style='background-color:' size=''>End</font>：水平对齐尾部。

  ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/10/202312121619251.png)

#### 设置文本超长显示

当文本内容较多超出了Text组件范围的时候，您可以使用<font color='red' style='background-color:' size=''>textOverflow</font><font color='cornflowerblue' style='background-color:' size=''>设置文本截取方式</font>，需配合`maxLines`使用，单独设置不生效，maxLines用于设置文本显示最大行数。下面的示例代码将textOverflow设置为Ellipsis ，它将显示不下的文本用 “...” 表示：

```typescript
Text('This is the text content of Text Component This is the text content of Text Component')
  .fontSize(16)
  .maxLines(1)
  .textOverflow({overflow:TextOverflow.Ellipsis})
  .backgroundColor(0xE6F2FD) 
```

效果图如下：

![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312121620024.png)

#### 设置文本装饰线

使用<font color='red' style='background-color:' size=''>decoration</font><font color='cornflowerblue' style='background-color:' size=''>设置文本装饰线样式及其颜色</font>，大家在浏览网页的时候经常可以看到装饰线，例如带有下划线超链接文本。<font color='cornflowerblue' style='background-color:' size=''>decoration包含type和color两个参数</font>，其中type用于设置装饰线样式，参数类型为`TextDecorationTyp`，color为可选参数。

下面的示例代码给文本设置了下划线，下划线颜色为黑色：

```typescript
Text('HarmonyOS')
  .fontSize(20)
  .decoration({ type: TextDecorationType.Underline, color: Color.Black })
  .backgroundColor(0xE6F2FD)
```

效果图如下：

![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312121621639.png)

<font color='red' style='background-color:' size=''>TextDecorationTyp</font>包含以下几种类型：

- <font color='red' style='background-color:' size=''>None</font>：不使用文本装饰线。

  ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/10/202312121612555.png)

- <font color='red' style='background-color:' size=''>Overline</font>：文字上划线修饰。

  ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312121621953.png)

- <font color='red' style='background-color:' size=''>LineThrough</font>：穿过文本的修饰线。

  ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312121621460.png)

- <font color='red' style='background-color:' size=''>Underline</font>：文字下划线修饰。

  ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312121621656.png)

### 3 Image

<font color='red' style='background-color:' size=''>Image组件用来渲染展示图片</font>，它可以让界面变得更加丰富多彩。只需要给Image组件设置图片地址、宽和高，图片就能加载出来，示例如下：

```typescript
Image($r("app.media.icon"))
  .width(100)
  .height(100)
```

效果图如下：

![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312121622679.png)

#### 设置缩放类型

为了使图片在页面中有更好的显示效果，有时候需要对图片进行缩放处理。您可以使用<font color='red' style='background-color:' size=''>objectFit</font>属性设置图片的缩放类型，<font color='cornflowerblue' style='background-color:' size=''>objectFit的参数类型为ImageFit</font>。

现有原始图片如下：

![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312121624773.png)

将图片加载到Image组件，设置宽高各100，设置objectFit为Cover（默认值），设置图片背景色为灰色0xCCCCCC。示例代码如下：

```typescript
Image($r("app.media.image2"))
  .objectFit(ImageFit.Cover)
  .backgroundColor(0xCCCCCC)
  .width(100)
  .height(100) 
```

效果图如下：

![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/10/202312121612279.png)

<font color='red' style='background-color:' size=''>ImageFit</font>包含以下几种类型：

- <font color='red' style='background-color:' size=''>Contain</font>：保持宽高比进行缩小或者放大，使得图片完全显示在显示边界内。

  ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312121624493.png)

- <font color='red' style='background-color:' size=''>Cover</font>（默认值）：保持宽高比进行缩小或者放大，使得图片两边都大于或等于显示边界。

  ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312121624127.png)

- <font color='red' style='background-color:' size=''>Auto</font>：自适应显示。

  ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312121624494.png)

- <font color='red' style='background-color:' size=''>Fill</font>：不保持宽高比进行放大缩小，使得图片充满显示边界。

  ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312121624321.png)

- <font color='red' style='background-color:' size=''>ScaleDown</font>：保持宽高比显示，图片缩小或者保持不变。

  ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312121624596.png)

- <font color='red' style='background-color:' size=''>None</font>：保持原有尺寸显示。

  ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312121624495.png)

#### 加载网络图片

比如浏览新闻的时候，图片一般从网络加载而来，Image组件支持加载网络图片，将图片地址换成网络图片地址进行加载。

```typescript
Image('https://www.example.com/xxx.png')
```

<font color='cornflowerblue' style='background-color:' size=''>为了成功加载网络图片，您需要在module.json5文件中申明网络访问权限</font>。

```json
{
    "module" : {
        "requestPermissions":[
           {
             "name": "ohos.permission.INTERNET"
           }
        ]
    }
}
```

> ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/10/202312121612213.png)
>
> 应用访问网络需要申请<font color='red' style='background-color:' size=''>ohos.permission.INTERNET</font>权限，因为HarmonyOS提供了一种访问控制机制即应用权限，用来保证这些数据或功能不会被不当或恶意使用。关于应用权限的的详细信息开发者可以参考：[访问控制](https://developer.harmonyos.com/cn/docs/documentation/doc-guides/accesstoken-overview-0000001333641125)。

### 4 TextInput

<font color='red' style='background-color:' size=''>TextInput组件用于输入单行文本，响应输入事件</font>。TextInput的使用也非常广泛，例如应用登录账号密码、发送消息等。和Text组件一样，TextInput组件也支持文本样式设置，下面的示例代码实现了一个简单的输入框：

```typescript
TextInput()
  .fontColor(Color.Blue)
  .fontSize(20)
  .fontStyle(FontStyle.Italic)
  .fontWeight(FontWeight.Bold)
  .fontFamily('Arial') 
```

效果图如下：

![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312121630275.png)

#### 设置输入提示文本

当我们平时使用输入框的时候，往往会有一些提示文字。例如登录账号的时候会有“请输入账号”这样的文本提示，当用户输入内容之后，提示文本就会消失，这种提示功能使用<font color='red' style='background-color:' size=''>placeholder</font>属性就可以轻松的实现。您还可以使用<font color='cornflowerblue' style='background-color:' size=''>placeholderColor和placeholderFont分别设置提示文本的颜色和样式</font>，示例代码如下：

```typescript
TextInput({ placeholder: '请输入帐号' })
  .placeholderColor(0x999999)
  .placeholderFont({ size: 20, weight: FontWeight.Medium, family: 'cursive', style: FontStyle.Italic })
```

效果图如下：

![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312121631603.gif)

#### 设置输入类型

可以使用<font color='red' style='background-color:' size=''>type</font>属性来设置输入框类型。例如密码输入框，一般输入密码的时候，为了用户密码安全，内容会显示为“......”，针对这种场景，将type属性设置为<font color='cornflowerblue' style='background-color:' size=''>InputType.Password</font>就可以实现。示例代码如下：

```typescript
TextInput({ placeholder: '请输入密码' })
  .type(InputType.Password)
```

效果图如下：

![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312121632272.gif)

<font color='red' style='background-color:' size=''>type</font>的参数类型为<font color='red' style='background-color:' size=''>InputType</font>，包含以下几种输入类型：

- <font color='red' style='background-color:' size=''>Normal</font>：基本输入模式。支持输入数字、字母、下划线、空格、特殊字符。

  ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312121632709.png)

- <font color='red' style='background-color:' size=''>Password</font>：密码输入模式。

  ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312121632970.png)

- <font color='red' style='background-color:' size=''>Email</font>：e-mail地址输入模式。

  ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/10/202312121612071.png)

- <font color='red' style='background-color:' size=''>Number</font>：纯数字输入模式。

  ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312121633860.png)

#### 设置光标位置

可以使用<font color='red' style='background-color:' size=''>TextInputController</font>动态设置光位置，下面的示例代码使用TextInputController的caretPosition方法，将光标移动到了第二个字符后。

```typescript
@Entry
@Component
struct TextInputDemo {
  controller: TextInputController = new TextInputController()
 
  build() {
    Column() {
      TextInput({ controller: this.controller })
      Button('设置光标位置')
        .onClick(() => {
          this.controller.caretPosition(2)
        })
    }
    .height('100%')
    .backgroundColor(0xE6F2FD)
  }
}
```

效果图如下：

![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131035200.gif)

#### 获取输入文本

我们可以给TextInput设置onChange事件，输入文本发生变化时触发回调，下面示例代码中的value为实时获取用户输入的文本信息。

```typescript
@Entry
@Component
struct TextInputDemo {
  @State text: string = ''
 
  build() {
    Column() {
      TextInput({ placeholder: '请输入账号' })
        .caretColor(Color.Blue)
        .onChange((value: string) => {
          this.text = value
        })
      Text(this.text)
    }
    .alignItems(HorizontalAlign.Center)
    .padding(12)
    .backgroundColor(0xE6F2FD)
  }
}
```

效果图如下：

![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131035188.gif)

### 5 Button

<font color='red' style='background-color:' size=''>Button组件主要用来响应点击操作，可以包含子组件</font>。下面的示例代码实现了一个“登录按钮”：

```typescript
Button('登录', { type: ButtonType.Capsule, stateEffect: true })
  .width('90%')
  .height(40)
  .fontSize(16)
  .fontWeight(FontWeight.Medium)
  .backgroundColor('#007DFF')
```

效果图如下：

![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/10/202312121612888.png)

#### 设置按钮样式

<font color='red' style='background-color:' size=''>type用于定义按钮样式</font>，示例代码中<font color='red' style='background-color:' size=''>ButtonType.Capsule</font>表示胶囊形按钮；stateEffect用于设置按钮按下时是否开启切换效果，当状态置为false时，点击效果关闭，默认值为true。

我们可以设置多种样式的Button，除了Capsule可以以设置Normal和Circle：

- **<font color='red' style='background-color:' size=''>Capsule</font>**：胶囊型按钮（圆角默认为高度的一半）。

  ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131035204.png)

- **<font color='red' style='background-color:' size=''>Circle</font>**：圆形按钮。

  ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131036084.png)

- **<font color='red' style='background-color:' size=''>Normal</font>**：普通按钮（默认不带圆角）。

  ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131036654.png)

#### 设置按钮点击事件

<font color='red' style='background-color:' size=''>可以给Button绑定onClick事件</font>，每当用户点击Button的时候，就会回调执行onClick方法，调用里面的逻辑代码。

```typescript
Button('登录', { type: ButtonType.Capsule, stateEffect: true })
  ...
  .onClick(() => {
  // 处理点击事件逻辑
  })
```

#### 包含子组件

<font color='red' style='background-color:' size=''>Button组件可以包含子组件</font>，让您可以开发出更丰富多样的Button，下面的示例代码中Button组件包含了一个Image组件：

```typescript
Button({ type: ButtonType.Circle, stateEffect: true }) {
  Image($r('app.media.icon_delete'))
    .width(30)
    .height(30)
}
.width(55)
.height(55)
.backgroundColor(0x317aff)
```

效果图如下：

![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131036933.png)

### 6 LoadingProgress

<font color='red' style='background-color:' size=''>LoadingProgress组件用于显示加载进展</font>，比如应用的登录界面，当我们点击登录的时候，显示的“正在登录”的进度条状态。LoadingProgress的使用非常简单，只需要设置颜色和宽高就可以了。

```typescript
LoadingProgress()
  .color(Color.Blue)
  .height(60)
  .width(60)
```

效果图如下：

![0260086000103404958.20221102115448.33909926886752830021497148543403](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131047089.gif)

### 7 使用资源引用类型

<font color='red' style='background-color:' size=''>Resource是资源引用类型</font>，用于设置组件属性的值。推荐大家优先使用Resource类型，将资源文件（字符串、图片、音频等）统一存放于resources目录下，便于开发者统一维护。同时系统可以根据当前配置加载合适的资源，例如，开发者可以根据屏幕尺寸呈现不同的布局效果，或根据语言设置提供不同的字符串。

例如下面的这段代码，直接在代码中写入了字符串和数字这样的硬编码。

```typescript
Button('登录', { type: ButtonType.Capsule, stateEffect: true })
  .width(300)
  .height(40)
  .fontSize(16)
  .fontWeight(FontWeight.Medium)
  .backgroundColor('#007DFF')
```

我们可以将这些硬编码写到entry/src/main/resources下的资源文件中。

在string.json中定义Button显示的文本。

```json
{
  "string": [
    {
      "name": "login_text",
      "value": "登录"
    }
  ]
} 
```

在float.json中定义Button的宽高和字体大小。

```json
{
  "float": [
    {
      "name": "button_width",
      "value": "300vp"
    },
    {
      "name": "button_height",
      "value": "40vp"
    },
    {
      "name": "login_fontSize",
      "value": "18fp"
    }
  ]
}
```

在color.json中定义Button的背景颜色。

```json
{
  "color": [
    {
      "name": "button_color",
      "value": "#1890ff"
    }
  ]
}
```

然后在Button组件通过`<font color='red' style='background-color:' size=''>$r('app.type.name')</font>”的形式引用应用资源。app代表应用内<font color='cornflowerblue' style='background-color:' size=''>resources目录中定义的资源；type代表资源类型（或资源的存放位置），可以取“color”、“float”、“string”、“plural”、“media”；name代表资源命名，由开发者定义资源时确定</font>。

```typescript
Button($r('app.string.login_text'), { type: ButtonType.Capsule })
  .width($r('app.float.button_width'))
  .height($r('app.float.button_height'))
  .fontSize($r('app.float.login_fontSize'))
  .backgroundColor($r('app.color.button_color'))
```

### 8 参考资料

常用基础的组件的更多使用方法可以参考：

- [Text](https://developer.harmonyos.com/cn/docs/documentation/doc-references/ts-basic-components-text-0000001333720953)
- [Image](https://developer.harmonyos.com/cn/docs/documentation/doc-references/ts-basic-components-image-0000001281001226)
- [TextInput](https://developer.harmonyos.com/cn/docs/documentation/doc-references/ts-basic-components-textinput-0000001333321201)
- [Button](https://developer.harmonyos.com/cn/docs/documentation/doc-references/ts-basic-components-button-0000001281480682)
- [LoadingProgress](https://developer.harmonyos.com/cn/docs/documentation/doc-references/ts-basic-components-loadingprogress-0000001281361106)

引用资源类型的使用可以参考：

- [资源访问](https://developer.harmonyos.com/cn/docs/documentation/doc-guides/resource-categories-and-access-0000001435940589)

## Column&Row组件的使用

### 1 概述

一个丰富的页面需要很多组件组成，那么，我们如何才能让这些组件有条不紊地在页面上布局呢？这就需要借助容器组件来实现。

容器组件是一种比较特殊的组件，它可以包含其他的组件，而且按照一定的规律布局，帮助开发者生成精美的页面。容器组件除了放置基础组件外，也可以放置容器组件，通过多层布局的嵌套，可以布局出更丰富的页面。

ArkTS为我们提供了丰富的容器组件来布局页面，本文将以构建登录页面为例，介绍Column和Row组件的属性与使用。

![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131729417.png)

### 2 组件介绍

#### 布局容器概念

<font color='red' style='background-color:' size=''>线性布局容器表示按照垂直方向或者水平方向排列子组件的容器</font>，ArkTS提供了Column和Row容器来实现线性布局。

- <font color='red' style='background-color:' size=''>Column</font>表示沿<font color='red' style='background-color:' size=''>垂直方向</font>布局的容器。
- <font color='red' style='background-color:' size=''>Row</font>表示沿<font color='red' style='background-color:' size=''>水平方向</font>布局的容器。

#### 主轴和交叉轴概念

在布局容器中，默认存在两根轴，分别是主轴和交叉轴，这两个轴始终是相互垂直的。不同的容器中主轴的方向不一样的。

- **<font color='red' style='background-color:' size=''>主轴</font>**：在<font color='red' style='background-color:' size=''>Column</font>容器中的子组件是按照从上到下的垂直方向布局的，其主轴的方向是<font color='red' style='background-color:' size=''>垂直方向</font>；在<font color='red' style='background-color:' size=''>Row</font>容器中的组件是按照从左到右的水平方向布局的，其主轴的方向是<font color='red' style='background-color:' size=''>水平方向</font>。

图2-1 Column容器&Row容器主轴

![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131731702.png)

- **<font color='red' style='background-color:' size=''>交叉轴</font>**：<font color='red' style='background-color:' size=''>与主轴垂直相交的轴线</font>，如果主轴是垂直方向，则交叉轴就是水平方向；如果主轴是水平方向，则交叉轴是垂直方向。

图2-2 Column容器&Row容器交叉轴

![img](https://alliance-communityfile-drcn.dbankcdn.com/FileServer/getFile/cmtyPub/103/404/958/0260086000103404958.20221102115920.30331924582664597826254415316555:50001231000000:2800:3531A948E66A8AEFBC7ACD4C60C8412D31B655FF90C52DCA0F06B4E8C6B1DE16.png)

#### 属性介绍

了解布局容器的主轴和交叉轴，主要是为了让大家更好地理解子组件在主轴和交叉轴的排列方式。

接下来，我们将详细讲解Column和Row容器的两个属性<font color='red' style='background-color:' size=''>justifyContent和alignItems</font>。

| 属性名称                                                     | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| <font color='red' style='background-color:' size=''>justifyContent</font> | 设置子组件在<font color='red' style='background-color:' size=''>主轴</font>方向上的对齐格式。 |
| <font color='red' style='background-color:' size=''>alignItems</font> | 设置子组件在<font color='red' style='background-color:' size=''>交叉轴</font>方向上的对齐格式。 |

1. 主轴方向的对齐（justifyContent）

   子组件在主轴方向上的对齐使用justifyContent属性来设置，其参数类型是[FlexAlign](https://developer.harmonyos.com/cn/docs/documentation/doc-references/ts-appendix-enums-0000001281201130#ZH-CN_TOPIC_0000001281201130__flexalign)。<font color='red' style='background-color:' size=''>FlexAlign</font>定义了以下几种类型：

   + **<font color='red' style='background-color:' size=''>Start</font>**：元素在<font color='red' style='background-color:' size=''>主轴方向首端对齐</font>，第一个元素与行首对齐，同时后续的元素与前一个对齐。![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131734729.png)
   + **<font color='red' style='background-color:' size=''>Center</font>**：元素在<font color='red' style='background-color:' size=''>主轴方向中心对齐</font>，第一个元素与行首的距离以及最后一个元素与行尾距离相同。![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131729178.png)
   + <font color='red' style='background-color:' size=''>End</font>：元素在<font color='red' style='background-color:' size=''>主轴方向尾部对齐</font>，最后一个元素与行尾对齐，其他元素与后一个对齐。![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131735314.png)
   + **<font color='red' style='background-color:' size=''>SpaceBetween</font>**：元素<font color='red' style='background-color:' size=''>在主轴方向均匀分配弹性元素，相邻元素之间距离相同</font>。 第一个元素与行首对齐，最后一个元素与行尾对齐。![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131736336.png)
   + **<font color='red' style='background-color:' size=''>SpaceAround</font>**：元素<font color='red' style='background-color:' size=''>在主轴方向均匀分配弹性元素，相邻元素之间距离相同。 第一个元素到行首的距离和最后一个元素到行尾的距离是相邻元素之间距离的一半</font>。![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131729155.png)
   + **<font color='red' style='background-color:' size=''>SpaceEvenly</font>**：元素<font color='red' style='background-color:' size=''>在主轴方向等间距布局</font>，无论是相邻元素还是边界元素到容器的间距都一样。![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131738996.png)

2. 交叉轴方向的对齐（alignItems)

   子组件在交叉轴方向上的对齐方式使用alignItems属性来设置。

   Column容器的主轴是垂直方向，交叉轴是水平方向，其参数类型为HorizontalAlign（水平对齐），<font color='red' style='background-color:' size=''>HorizontalAlign</font>定义了以下几种类型：

   + **<font color='red' style='background-color:' size=''>Start</font>**：设置子组件在水平方向上按照<font color='red' style='background-color:' size=''>起始端对齐</font>。

     ![image-20231213174054923](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131740988.png)

   + **<font color='red' style='background-color:' size=''>Center</font>**（默认值)：设置子组件在<font color='red' style='background-color:' size=''>水平方向上居中对齐</font>。
   
     ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131742163.png)
   
   + **<font color='red' style='background-color:' size=''>End</font>**：设置子组件在<font color='red' style='background-color:' size=''>水平方向上按照末端对齐</font>。
   
     ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131743563.png)
   
   Row容器的主轴是水平方向，交叉轴是垂直方向，其参数类型为<font color='red' style='background-color:' size=''>VerticalAlign</font>（垂直对齐)，VerticalAlign定义了以下几种类型：

   + **<font color='red' style='background-color:' size=''>Top</font>**：设置子组件在<font color='red' style='background-color:' size=''>垂直方向上居顶部对齐</font>。
     ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131745381.png)

   + **<font color='red' style='background-color:' size=''>Center</font>**（默认值）：设置子组件<font color='red' style='background-color:' size=''>在竖直方向上居中对齐</font>。

     ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131729071.png)

   + **<font color='red' style='background-color:' size=''>Bottom</font>**：设置子组件在<font color='red' style='background-color:' size=''>竖直方向上居底部对齐</font>。

     ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131746491.png)

#### 接口介绍

接下来，我们介绍Column和Row容器的接口。

| 容器组件 | 接口                                      |
| -------- | ----------------------------------------- |
| Column   | Column(value?:{space?: string \| number}) |
| Row      | Row(value?:{space?: string \| number})    |

Column和Row容器的接口都有一个<font color='red' style='background-color:' size=''>可选参数space，表示子组件在主轴方向上的间距</font>。

效果如下：

![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131729104.png)

### 3 组件使用

我们来具体讲解如何高效的使用Column和Row容器组件来构建这个登录页面。

当我们从设计同学那拿到一个页面设计图时，我们需要对页面进行拆解，先确定页面的布局，再分析页面上的内容分别使用哪些组件来实现。

我们仔细分析这个登录页面。在静态布局中，组件整体是从上到下布局的，因此构建该页面可以使用Column来构建。在此基础上，我们可以看到有部分内容在水平方向上由几个基础组件构成，例如页面中间的短信验证码登录与忘记密码以及页面最下方的其他方式登录，那么构建这些内容的时候，可以在Column组件中嵌套Row组件，继而在Row组件中实现水平方向的布局。

![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131746359.png)

根据上述页面拆解，在Column容器里，依次是Image、Text、TextInput、Button等基础组件，还有两组组件是使用Row容器组件来实现的，主要代码如下：

```typescript
@Entry
@Component
export struct LoginPage {
  build() {
    Column() {
      Image($r('app.media.logo'))
        ...
      Text($r('app.string.login_page'))
        ...
      Text($r('app.string.login_more'))
        ...
      TextInput({ placeholder: $r('app.string.account') })
        ...
      TextInput({ placeholder: $r('app.string.password') })
        ...
      Row() {
        Text($r(…)) 
        Text($r(…)) 
      }
      Button($r('app.string.login'), { type: ButtonType.Capsule, stateEffect: true })
        ...
      Row() {
        this.imageButton($r(…))
        this.imageButton($r(…))
        this.imageButton($r(…))
      }
      ...
    }
    ...
  }
}
```

我们详细看一下使用Row容器的两组组件。

两个文本组件展示的内容是按水平方向布局的，使用两端对齐的方式。这里我们使用Row容器组件，并且需要配置主轴上（水平方向）的对齐格式justifyContent为FlexAlign.SpaceBetween（两端对齐）。

```typescript
Row() {
  Text($r(…)) 
  Text($r(…)) 
  }
  .justifyContent(FlexAlign.SpaceBetween)
  .width('100%')
```

其他登录方式的三个按钮也是按水平方向布局的，同样使用Row容器组件。这里按钮的间距是一致的，我们可以通过配置可选参数space来设置按钮间距，使子组件间距一致。

```typescript
Row({ space: CommonConstants.LOGIN_METHODS_SPACE }) {
  this.imageButton($r(…))
  this.imageButton($r(…))
  this.imageButton($r(…))
}
```

至此，你已经完成这个登录页面的简单布局实现了。你可以参考[《常用组件与布局》](https://developer.huawei.com/consumer/cn/codelabsPortal/carddetails/tutorials_ArkTSComponents)这个Codelab验证一下页面效果。

另外，你也可以通过[《常用布局容器对齐方式》](https://developer.huawei.com/consumer/cn/codelabsPortal/carddetails/tutorials_LayoutAlignETS)这个Codelab来深入学习Column、Row、Flex、Stack容器组件的对齐方式，掌握更多布局容器的使用方法。

### 4 参考链接

- Column组件的相关API参考：[Column组件](https://developer.harmonyos.com/cn/docs/documentation/doc-references/ts-container-column-0000001333641085)。
- Row组件的相关API参考：[Row组件](https://developer.harmonyos.com/cn/docs/documentation/doc-references/ts-container-row-0000001281480714)。

## List组件和Grid组件的使用

### 简介

在我们常用的手机应用中，经常会见到一些数据列表，如设置页面、通讯录、商品列表等。下图中两个页面都包含列表，“首页”页面中包含两个网格布局，“商城”页面中包含一个商品列表。

![点击放大](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131749973.png)

上图中的列表中都包含一系列相同宽度的列表项，连续、多行呈现同类数据，例如图片和文本。常见的列表有线性列表（List列表）和网格布局（Grid列表）：

![点击放大](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131749966.png)

为了帮助开发者构建包含列表的应用，ArkUI提供了List组件和Grid组件，开发者使用List和Grid组件能够很轻松的完成一些列表页面。

### List组件的使用

#### List组件简介

<font color='red' style='background-color:' size=''>List是很常用的滚动类容器组件</font>，<font color='cornflowerblue' style='background-color:' size=''>一般和子组件ListItem一起使用，List列表中的每一个列表项对应一个ListItem组件</font>。

![点击放大](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131750424.png)

#### 使用ForEeach渲染列表

列表往往由多个列表项组成，所以我们需要在List组件中使用多个ListItem组件来构建列表，这就会导致代码的冗余。使用循环渲染（ForEach）遍历数组的方式构建列表，可以减少重复代码，示例代码如下：

```typescript
@Entry
@Component
struct ListDemo {
  private arr: number[] = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

  build() {
    Column() {
      List({ space: 10 }) {
        ForEach(this.arr, (item: number) => {
          ListItem() {
            Text(`${item}`)
              .width('100%')
              .height(100)
              .fontSize(20)
              .fontColor(Color.White)
              .textAlign(TextAlign.Center)
              .borderRadius(10)
              .backgroundColor(0x007DFF)
          }
        }, item => item)
      }
    }
    .padding(12)
    .height('100%')
    .backgroundColor(0xF1F3F5)
  }
}
```

效果图如下：

![点击放大](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131752133.png)

#### 设置列表分割线

<font color='red' style='background-color:' size=''>List组件子组件ListItem之间默认是没有分割线的</font>，部分场景子组件ListItem间需要设置分割线，这时候您可以使用List组件的<font color='red' style='background-color:' size=''>divider</font>属性。divider属性包含四个参数：

- **<font color='red' style='background-color:' size=''>strokeWidth</font>**: 分割线的<font color='red' style='background-color:' size=''>线宽</font>。

- **<font color='red' style='background-color:' size=''>color</font>**: 分割线的<font color='red' style='background-color:' size=''>颜色</font>。

- **<font color='red' style='background-color:' size=''>startMargin</font>**：分割线<font color='red' style='background-color:' size=''>距离列表侧边起始端的距离</font>。

- **<font color='red' style='background-color:' size=''>endMargin</font>**: 分割线<font color='red' style='background-color:' size=''>距离列表侧边结束端的距离</font>。

  ![点击放大](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131749061.png)

#### List列表滚动事件监听

List组件提供了一系列事件方法用来<font color='red' style='background-color:' size=''>监听列表的滚动</font>，您可以根据需要，监听这些事件来做一些操作：

- **<font color='red' style='background-color:' size=''>onScroll</font>**：列表滑动时触发，返回值scrollOffset为滑动偏移量，scrollState为当前滑动状态。
- **<font color='red' style='background-color:' size=''>onScrollIndex</font>**：列表滑动时触发，返回值分别为滑动起始位置索引值与滑动结束位置索引值。
- **<font color='red' style='background-color:' size=''>onReachStart</font>**：列表到达起始位置时触发。
- **<font color='red' style='background-color:' size=''>onReachEnd</font>**：列表到底末尾位置时触发。
- **<font color='red' style='background-color:' size=''>onScrollStop</font>**：列表滑动停止时触发。

使用示例代码如下：

```typescript
List({ space: 10 }) {
  ForEach(this.arr, (item) => {
    ListItem() {
      Text(`${item}`)
        ...
    }
  }, item => item)
}
.onScrollIndex((firstIndex: number, lastIndex: number) => {
  console.info('first' + firstIndex)
  console.info('last' + lastIndex)
})
.onScroll((scrollOffset: number, scrollState: ScrollState) => {
  console.info('scrollOffset' + scrollOffset)
  console.info('scrollState' + scrollState)
})
.onReachStart(() => {
  console.info('onReachStart')
})
.onReachEnd(() => {
  console.info('onReachEnd')
})
.onScrollStop(() => {
  console.info('onScrollStop')
})
```

#### 设置List排列方向

List组件里面的列表项<font color='red' style='background-color:' size=''>默认是按垂直方向排列</font>的，如果您想让列表沿水平方向排列，您可以将List组件的<font color='red' style='background-color:' size=''>listDirection</font>属性设置为<font color='red' style='background-color:' size=''>Axis.Horizonta</font>l。

**<font color='red' style='background-color:' size=''>listDirection</font>**参数类型是[Axis](https://developer.harmonyos.com/cn/docs/documentation/doc-references-V3/ts-appendix-enums-0000001478061741-V3?catalogVersion=V3#ZH-CN_TOPIC_0000001478061741__axis)，定义了以下两种类型：

- **<font color='red' style='background-color:' size=''>Vertical</font>**（默认值）：子组件ListItem在List容器组件中<font color='red' style='background-color:' size=''>呈纵向排列</font>。

  ![点击放大](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131755965.png)

- **<font color='red' style='background-color:' size=''>Horizontal</font>**：子组件ListItem在List容器组件中<font color='red' style='background-color:' size=''>呈横向排列</font>。

  ![点击放大](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131749717.png)

### Grid组件的使用

#### Grid组件简介

Grid组件为网格容器，是一种<font color='red' style='background-color:' size=''>网格列表，由“行”和“列”分割的单元格所组成</font>，通过指定“项目”所在的单元格做出各种各样的布局。Grid组件一般和子组件GridItem一起使用，Grid列表中的每一个条目对应一个GridItem组件。

![点击放大](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131756389.png)

#### 使用ForEach渲染网格布局

和List组件一样，Grid组件也可以使用ForEach来渲染多个列表项GridItem，我们通过下面的这段示例代码来介绍Grid组件的使用。

```typescript
@Entry
@Component
struct GridExample {
  // 定义一个长度为16的数组
  private arr: string[] = new Array(16).fill('').map((_, index) => `item ${index}`);

  build() {
    Column() {
      Grid() {
        ForEach(this.arr, (item: string) => {
          GridItem() {
            Text(item)
              .fontSize(16)
              .fontColor(Color.White)
              .backgroundColor(0x007DFF)
              .width('100%')
              .height('100%')
              .textAlign(TextAlign.Center)
          }
        }, item => item)
      }
      .columnsTemplate('1fr 1fr 1fr 1fr')
      .rowsTemplate('1fr 1fr 1fr 1fr')
      .columnsGap(10)
      .rowsGap(10)
      .height(300)
    }
    .width('100%')
    .padding(12)
    .backgroundColor(0xF1F3F5)
  }
}
```

示例代码中创建了16个GridItem列表项。同时设置<font color='red' style='background-color:' size=''>columnsTemplate的值为'1fr 1fr 1fr 1fr'，表示这个网格为4列，将Grid允许的宽分为4等分，每列占1份</font>；<font color='red' style='background-color:' size=''>rowsTemplate的值为'1fr 1fr 1fr 1fr'，表示这个网格为4行，将Grid允许的高分为4等分，每行占1份</font>。这样就构成了一个4行4列的网格列表，然后使用<font color='red' style='background-color:' size=''>columnsGap设置列间距为10vp</font>，使用r<font color='red' style='background-color:' size=''>owsGap设置行间距也为10vp</font>。示例代码效果图如下：

![点击放大](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131820265.png)

上面构建的网格布局使用了固定的行数和列数，所以构建出的网格是不可滚动的。然而有时候因为内容较多，我们通过滚动的方式来显示更多的内容，就需要一个<font color='red' style='background-color:' size=''>可以滚动的网格布局</font>。我们只需要设置<font color='red' style='background-color:' size=''>rowsTemplate和columnsTemplate</font>中的一个即可。

将示例代码中GridItem的高度设置为固定值，例如100；<font color='red' style='background-color:' size=''>仅设置columnsTemplate属性，不设置rowsTemplate属性，就可以实现Grid列表的滚动</font>：

```typescript
Grid() {
  ForEach(this.arr, (item: string) => {
    GridItem() {
      Text(item)
        .height(100)
        ...
    }
  }, item => item)
}
.columnsTemplate('1fr 1fr 1fr 1fr')
.columnsGap(10)
.rowsGap(10)
.height(300)
```

此外，<font color='red' style='background-color:' size=''>Grid像List一样也可以使用onScrollIndex来监听列表的滚动</font>。

#### 列表性能优化

开发者在使用长列表时，如果直接采用循环渲染方式，会一次性加载所有的列表元素，从而导致页面启动时间过长，影响用户体验，推荐通过以下方式来进行列表性能优化：

[使用数据懒加载](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/ui-ts-performance-improvement-recommendation-0000001477981001-V3#ZH-CN_TOPIC_0000001477981001__推荐使用数据懒加载)

[设置list组件的宽高](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/ui-ts-performance-improvement-recommendation-0000001477981001-V3#ZH-CN_TOPIC_0000001477981001__设置list组件的宽高)

### 参考链接

1. List组件的相关API参考：[List组件](https://developer.harmonyos.com/cn/docs/documentation/doc-references-V3/ts-container-list-0000001477981213-V3?catalogVersion=V3)。
2. Grid组件的相关API参考：[Grid组件](https://developer.harmonyos.com/cn/docs/documentation/doc-references-V3/ts-container-grid-0000001478341161-V3?catalogVersion=V3)。
3. 循环渲染（ForEach）：[循环渲染](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/arkts-rendering-control-0000001427744548-V3?catalogVersion=V3#ZH-CN_TOPIC_0000001427744548__循环渲染)。

## Tabs组件的使用

### 概述

在我们常用的应用中，经常会有视图内容切换的场景，来展示更加丰富的内容。比如下面这个页面，点击底部的页签的选项，可以实现“首页”和“我的”

<font color='red' style='background-color:' size=''>两个内容视图的切换</font>。

![点击放大](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131802548.png)

ArkUI开发框架提供了一种页签容器组件Tabs，开发者通过Tabs组件可以很容易的实现内容视图的切换。页签容器Tabs的形式多种多样，不同的页面设计页签不一样，可以把页签设置在底部、顶部或者侧边。

![点击放大](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131802954.png)

本文将详细介绍Tabs组件的使用。

### Tabs组件的简单使用

<font color='red' style='background-color:' size=''>Tabs组件仅可包含子组件TabContent，每一个页签对应一个内容视图即TabContent组件</font>。下面的示例代码构建了一个简单的页签页面：

```typescript
@Entry
@Component
struct TabsExample {
  private controller: TabsController = new TabsController()

  build() {
    Column() {
      Tabs({ barPosition: BarPosition.Start, controller: this.controller }) {
        TabContent() {
          Column().width('100%').height('100%').backgroundColor(Color.Green)
        }
        .tabBar('green')

        TabContent() {
          Column().width('100%').height('100%').backgroundColor(Color.Blue)
        }
        .tabBar('blue')

        TabContent() {
          Column().width('100%').height('100%').backgroundColor(Color.Yellow)
        }
        .tabBar('yellow')

        TabContent() {
          Column().width('100%').height('100%').backgroundColor(Color.Pink)
        }
        .tabBar('pink')
      }
      .barWidth('100%') // 设置TabBar宽度
      .barHeight(60) // 设置TabBar高度
      .width('100%') // 设置Tabs组件宽度
      .height('100%') // 设置Tabs组件高度
      .backgroundColor(0xF5F5F5) // 设置Tabs组件背景颜色
    }
    .width('100%')
    .height('100%')
  }
}
```

效果图如下：

![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131804054.png)

上面示例代码中，Tabs组件中包含4个子组件TabContent，通过TabContent的tabBar属性设置TabBar的显示内容。使用通用属性width和height设置了Tabs组件的宽高，使用barWidth和barHeight设置了TabBar的宽度和高度。

![点击放大](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131810840.png)

> **<font color='red' style='background-color:' size=''>说明：</font>**
>
> + TabContent组件不支持设置通用宽度属性，其宽度默认撑满Tabs父组件。
> + TabContent组件不支持设置通用高度属性，其高度由Tabs父组件高度与TabBar组件高度决定。

#### 设置TabBar布局模式

因为Tabs的布局模式<font color='red' style='background-color:' size=''>默认是Fixed</font>的，所以Tabs的页签是不可滑动的。当页签比较多的时候，可能会导致页签显示不全，将布局模式设置为Scrollable的话，可以实现页签的滚动。

<font color='red' style='background-color:' size=''>Tabs的布局模式</font>有Fixed（默认）和Scrollable两种：

- **<font color='red' style='background-color:' size=''>BarMode.Fixed</font>**：所有TabBar平均分配barWidth宽度（纵向时平均分配barHeight高度）,页签不可滚动，效果图如下：

  ![点击放大](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131820853.png)

- **<font color='red' style='background-color:' size=''>BarMode.Scrollable</font>**：每一个TabBar均使用实际布局宽度，超过总长度（横向Tabs的barWidth，纵向Tabs的barHeight）后可滑动。

  ![点击放大](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131802878.png)

- 当页签比较多的时候，可以滑动页签，下面的示例代码将barMode设置为BarMode.Scrollable，实现了可滚动的页签：

```typescript
@Entry
@Component
struct TabsExample {
  private controller: TabsController = new TabsController()

  build() {
    Column() {
      Tabs({ barPosition: BarPosition.Start, controller: this.controller }) {
        TabContent() {
          Column()
            .width('100%')
            .height('100%')
            .backgroundColor(Color.Green)
        }
        .tabBar('green')

        TabContent() {
          Column()
            .width('100%')
            .height('100%')
            .backgroundColor(Color.Blue)
        }
        .tabBar('blue')

        ...

      }
      .barMode(BarMode.Scrollable)
      .barWidth('100%')
      .barHeight(60)
      .width('100%')
      .height('100%')
    }
  }
}
```

#### 设置TabBar位置和排列方向

<font color='red' style='background-color:' size=''>Tabs组件页签默认显示在顶部</font>，某些场景下您可能希望Tabs页签出现在底部或者侧边，您可以使用Tabs组件接口中的参数<font color='red' style='background-color:' size=''>barPosition</font>设置页签位置。此外页签显示位置还与vertical属性相关联，<font color='red' style='background-color:' size=''>vertical属性用于设置页签的排列方向</font>，<font color='cornflowerblue' style='background-color:' size=''>当vertical的属性值为false（默认值）时页签横向排列，为true时页签纵向排列</font>。

<font color='red' style='background-color:' size=''>barPosition</font>的值可以设置为BarPosition.Start（默认值）和BarPosition.End：

- **<font color='red' style='background-color:' size=''>BarPosition.Start</font>**

  vertical属性方法设置为false（默认值）时，页签位于容器顶部。

  ```typescript
  Tabs({ barPosition: BarPosition.Start }) {
    ...
  }
  .vertical(false) 
  .barWidth('100%') 
  .barHeight(60)  
  ```

  效果图如下：

  ![点击放大](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131802068.png)

  vertical属性方法设置为true时，页签位于容器左侧。

  ```typescript
  Tabs({ barPosition: BarPosition.Start }) {
    ...
  }
  .vertical(true) 
  .barWidth(100) 
  .barHeight(200)  
  ```

  效果图如下：

  ![点击放大](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131802726.png)

- **<font color='red' style='background-color:' size=''>BarPosition.End</font>**

  vertical属性方法设置为false时，页签位于容器底部。

  ```typescript
  Tabs({ barPosition: BarPosition.End }) {
    ...
  }
  .vertical(false) 
  .barWidth('100%') 
  .barHeight(60)
  ```

  效果图如下：

  ![点击放大](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131816677.png)

  vertical属性方法设置为true时，页签位于容器右侧。

  ```typescript
  Tabs({ barPosition: BarPosition.End}) {
    ...
  }
  .vertical(true) 
  .barWidth(100) 
  .barHeight(200)
  ```

  效果图如下：

  ![点击放大](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131816603.png)

#### 自定义TabBar样式

TabBar的默认显示效果如下所示：

![点击放大](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131816561.png)

往往开发过程中，UX给我们的设计效果可能并不是这样的，比如下面的这种底部页签效果：

![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/202312131816701.png)

<font color='red' style='background-color:' size=''>TabContent的tabBar属性除了支持string类型，还支持使用@Builder装饰器修饰的函数</font>。您可以使用@Builder装饰器，构造一个生成自定义TabBar样式的函数，实现上面的底部页签效果，示例代码如下：

```typescript
@Entry
@Component
struct TabsExample {
  @State currentIndex: number = 0;
  private tabsController: TabsController = new TabsController();

  @Builder TabBuilder(title: string, targetIndex: number, selectedImg: Resource, normalImg: Resource) {
    Column() {
      Image(this.currentIndex === targetIndex ? selectedImg : normalImg)
        .size({ width: 25, height: 25 })
      Text(title)
        .fontColor(this.currentIndex === targetIndex ? '#1698CE' : '#6B6B6B')
    }
    .width('100%')
    .height(50)
    .justifyContent(FlexAlign.Center)
    .onClick(() => {
      this.currentIndex = targetIndex;
      this.tabsController.changeIndex(this.currentIndex);
    })
  }

  build() {
    Tabs({ barPosition: BarPosition.End, controller: this.tabsController }) {
      TabContent() {
        Column().width('100%').height('100%').backgroundColor('#00CB87')
      }
      .tabBar(this.TabBuilder('首页', 0, $r('app.media.home_selected'), $r('app.media.home_normal')))

      TabContent() {
        Column().width('100%').height('100%').backgroundColor('#007DFF')
      }
      .tabBar(this.TabBuilder('我的', 1, $r('app.media.mine_selected'), $r('app.media.mine_normal')))
    }
    .barWidth('100%')
    .barHeight(50)
    .onChange((index: number) => {
      this.currentIndex = index;
    })
  }
}
```

示例代码中将barPosition的值设置为BarPosition.End，使页签显示在底部。使用@Builder修饰TabBuilder函数，生成由Image和Text组成的页签。同时也给Tabs组件设置了TabsController控制器，当点击某个页签时，调用changeIndex方法进行页签内容切换。

最后还需要给Tabs添加onChange事件，Tab页签切换后触发该事件，这样当我们左右滑动内容视图的时候，页签样式也会跟着改变。

### 参考

- Tabs组件的更多属性和参数的使用，可以参考API：[Tabs](https://developer.harmonyos.com/cn/docs/documentation/doc-references-V3/ts-container-tabs-0000001478181433-V3?catalogVersion=V3)。
- @Builder装饰器的使用，可以参考：[@Builder](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/arkts-dynamic-ui-elememt-building-0000001427584592-V3?catalogVersion=V3#ZH-CN_TOPIC_0000001427584592__builder)。