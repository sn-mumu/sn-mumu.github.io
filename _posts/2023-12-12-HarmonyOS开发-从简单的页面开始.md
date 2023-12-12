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

![img](https://alliance-communityfile-drcn.dbankcdn.com/FileServer/getFile/cmtyPub/103/404/958/0260086000103404958.20221102114256.28802753566728898498304998943977:50001231000000:2800:FE20D3B6F279BC1450B3B4D172874836DEA69202B2A75554D752FA3EB9D671BC.png)

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

## 设置光标位置

可以使用TextInputController动态设置光位置，下面的示例代码使用TextInputController的caretPosition方法，将光标移动到了第二个字符后。

```
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

![img](https://alliance-communityfile-drcn.dbankcdn.com/FileServer/getFile/cmtyPub/103/404/958/0260086000103404958.20221102115317.35282590037382074976290780085823:50001231000000:2800:CC4870F4F8F97CC04C6F11016FD88D50323C29F810F9D24B1C43E38E82A2BDEB.gif)

## 获取输入文本

我们可以给TextInput设置onChange事件，输入文本发生变化时触发回调，下面示例代码中的value为实时获取用户输入的文本信息。

```
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

![img](https://alliance-communityfile-drcn.dbankcdn.com/FileServer/getFile/cmtyPub/103/404/958/0260086000103404958.20221102115338.69547694354764992883763360730107:50001231000000:2800:BA25C8408214B902ECBD215DABB7916AED4E7FB78B8F280799A83B607AA7C909.gif)

# 5 Button

Button组件主要用来响应点击操作，可以包含子组件。下面的示例代码实现了一个“登录按钮”：

```
Button('登录', { type: ButtonType.Capsule, stateEffect: true })
  .width('90%')
  .height(40)
  .fontSize(16)
  .fontWeight(FontWeight.Medium)
  .backgroundColor('#007DFF')
```

效果图如下：

![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/10/202312121612888.png)

## 设置按钮样式

type用于定义按钮样式，示例代码中ButtonType.Capsule表示胶囊形按钮；stateEffect用于设置按钮按下时是否开启切换效果，当状态置为false时，点击效果关闭，默认值为true。

我们可以设置多种样式的Button，除了Capsule可以以设置Normal和Circle：

- Capsule：胶囊型按钮（圆角默认为高度的一半）。

  ![img](https://alliance-communityfile-drcn.dbankcdn.com/FileServer/getFile/cmtyPub/103/404/958/0260086000103404958.20221102115404.84177998820036585609688095430669:50001231000000:2800:24CD642B18947F8B32434F65CD7E2C91A4DFF48A1E4017520A161AEDA3ED30F0.png)

- Circle：圆形按钮。

  ![img](https://alliance-communityfile-drcn.dbankcdn.com/FileServer/getFile/cmtyPub/103/404/958/0260086000103404958.20221102115412.39174965009162190745462310963210:50001231000000:2800:60EFB66EA62ED61D60B28BCFB7C3F290964FDB1893DD428AF61531F600F69D3D.png)

- Normal：普通按钮（默认不带圆角）。

  ![img](https://alliance-communityfile-drcn.dbankcdn.com/FileServer/getFile/cmtyPub/103/404/958/0260086000103404958.20221102115421.19102972822842807234366477791218:50001231000000:2800:A1E76374955D7EDDC93058ED603C1232459C8CD2AFBF7B4EC96D650A6D80AF3F.png)

## 设置按钮点击事件

可以给Button绑定onClick事件，每当用户点击Button的时候，就会回调执行onClick方法，调用里面的逻辑代码。

```
Button('登录', { type: ButtonType.Capsule, stateEffect: true })
  ...
  .onClick(() => {
  // 处理点击事件逻辑
  })
```

## 包含子组件

Button组件可以包含子组件，让您可以开发出更丰富多样的Button，下面的示例代码中Button组件包含了一个Image组件：

```
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

![img](https://alliance-communityfile-drcn.dbankcdn.com/FileServer/getFile/cmtyPub/103/404/958/0260086000103404958.20221102115439.25988537294891479063435944038115:50001231000000:2800:7FB346C30BFCECCBDB87006E52813894BCCD0DCF5041B524F5B5C59F851D0DAA.png)

# 6 LoadingProgress

LoadingProgress组件用于显示加载进展，比如应用的登录界面，当我们点击登录的时候，显示的“正在登录”的进度条状态。LoadingProgress的使用非常简单，只需要设置颜色和宽高就可以了。

```
LoadingProgress()
  .color(Color.Blue)
  .height(60)
  .width(60)
```

效果图如下：

![img](https://alliance-communityfile-drcn.dbankcdn.com/FileServer/getFile/cmtyPub/103/404/958/0260086000103404958.20221102115448.33909926886752830021497148543403:50001231000000:2800:98D471A991979097448D44923AF569FB44492907377352F8752E055DDECD23C2.gif)

# 7 使用资源引用类型

Resource是资源引用类型，用于设置组件属性的值。推荐大家优先使用Resource类型，将资源文件（字符串、图片、音频等）统一存放于resources目录下，便于开发者统一维护。同时系统可以根据当前配置加载合适的资源，例如，开发者可以根据屏幕尺寸呈现不同的布局效果，或根据语言设置提供不同的字符串。

例如下面的这段代码，直接在代码中写入了字符串和数字这样的硬编码。

```
Button('登录', { type: ButtonType.Capsule, stateEffect: true })
  .width(300)
  .height(40)
  .fontSize(16)
  .fontWeight(FontWeight.Medium)
  .backgroundColor('#007DFF')
```

我们可以将这些硬编码写到entry/src/main/resources下的资源文件中。

在string.json中定义Button显示的文本。

```
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

```
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

```
{
  "color": [
    {
      "name": "button_color",
      "value": "#1890ff"
    }
  ]
}
```

然后在Button组件通过“$r('app.type.name')”的形式引用应用资源。app代表应用内resources目录中定义的资源；type代表资源类型（或资源的存放位置），可以取“color”、“float”、“string”、“plural”、“media”；name代表资源命名，由开发者定义资源时确定。

```
Button($r('app.string.login_text'), { type: ButtonType.Capsule })
  .width($r('app.float.button_width'))
  .height($r('app.float.button_height'))
  .fontSize($r('app.float.login_fontSize'))
  .backgroundColor($r('app.color.button_color'))
```

# 8 参考资料

常用基础的组件的更多使用方法可以参考：

- [Text](https://developer.harmonyos.com/cn/docs/documentation/doc-references/ts-basic-components-text-0000001333720953)
- [Image](https://developer.harmonyos.com/cn/docs/documentation/doc-references/ts-basic-components-image-0000001281001226)
- [TextInput](https://developer.harmonyos.com/cn/docs/documentation/doc-references/ts-basic-components-textinput-0000001333321201)
- [Button](https://developer.harmonyos.com/cn/docs/documentation/doc-references/ts-basic-components-button-0000001281480682)
- [LoadingProgress](https://developer.harmonyos.com/cn/docs/documentation/doc-references/ts-basic-components-loadingprogress-0000001281361106)

引用资源类型的使用可以参考：

- [资源访问](https://developer.harmonyos.com/cn/docs/documentation/doc-guides/resource-categories-and-access-0000001435940589)