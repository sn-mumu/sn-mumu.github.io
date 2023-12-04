---
title: HarmonyOS开发 ArkTS基础知识
author: mumu
date: 2023-12-04 13:55:00 +0800
categories: [华为,HarmonyOS]
tags: [华为,HarmonyOS,ArkTS基础知识]
pin: false
---

# ArkTS基础知识

## ArkUI开发框架![ArkUI开发框架](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/10/20231204224952.png)

## ArkTS声明式开发范式

- **装饰器**

用来装饰类、结构体、方法以及变量，赋予其特殊的含义，如上述示例中 @Entry 、 @Component 、 @State 都是装饰器。具体而言， @Component 表示这是个自定义组件； @Entry 则表示这是个入口组件； @State 表示组件中的状态变量，此状态变化会引起 UI 变更。

- **自定义组件**

可复用的 UI 单元，可组合其它组件，如上述被 @Component 装饰的 struct Hello。

- **UI 描述**

声明式的方式来描述 UI 的结构，如上述 build() 方法内部的代码块。

- **内置组件**

框架中默认内置的基础和布局组件，可直接被开发者调用，比如示例中的 Column、Text、Divider、Button。

- **事件方法**

用于添加组件对事件的响应逻辑，统一通过事件方法进行设置，如跟随在Button后面的onClick()。

- **属性方法**

用于组件属性的配置，统一通过属性方法进行设置，如fontSize()、width()、height()、color() 等，可通过链式调用的方式设置多项属性。

从UI框架的需求角度，ArkTS在TS的类型系统的基础上，做了进一步的扩展：**定义了各种装饰器、自定义组件和UI描述机制**，再配合UI开发框架中的UI内置组件、事件方法、属性方法等共同构成了应用开发的主体。在应用开发中，除了UI的结构化描述之外，还有一个重要的方面：**状态管理**。如上述示例中，用 @State 装饰过的变量 myText ，包含了一个基础的状态管理机制，即 myText 的值的变化会自动触发相应的 UI 变更 （Text组件）。**ArkUI 中进一步提供了多维度的状态管理机制**。和 UI 相关联的数据，不仅可以在组件内使用，还可以在不同组件层级间传递，比如父子组件之间，爷孙组件之间，也可以是全局范围内的传递，还可以是跨设备传递。另外，从数据的传递形式来看，可分为**只读的单向传递和可变更的双向传递**。开发者可以灵活的利用这些能力来实现数据和 UI 的联动。

总体而言，ArkUI开发框架通过扩展成熟语言、结合语法糖或者语言原生的元编程能力、以及UI组件、状态管理等方面设计了统一的UI开发范式，结合原生语言能力共同完成应用开发。这些构成了当前ArkTS基于TS的主要扩展。

**ArkUI完整的开发范式可参考这里：**

https://developer.harmonyos.com/cn/docs/documentation/doc-guides/arkui-overview-0000001281480754

![ArkTS声明式开发范式](https://alliance-communityfile-drcn.dbankcdn.com/FileServer/getFile/cmtyPub/103/404/958/0260086000103404958.20221102104418.97167608875006783664909326988909:50001231000000:2800:B0A2A3474E494DE620CBF58C0DBB2F835EBDAC07C87E2376865D3CA5ED979896.png)

## 介绍

> 本课程使用声明式语法和组件化基础知识，搭建一个可刷新的排行榜页面。在排行榜页面中，使用循环渲染控制语法来实现列表数据渲染，使用@Builder创建排行列表布局内容，使用装饰器@State、@Prop、@Link来管理组件状态。最后我们点击系统返回按键，来学习自定义组件生命周期函数。完成效果如图所示：
>
> ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/10/20231204231311.gif)

## 相关概念

### 1.渲染控制语法：

- [条件渲染](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/arkts-rendering-control-ifelse-0000001524177637-V3)：使用if/else进行条件渲染。

  ```typescript
  Column() {
     if (this.count > 0) {
         Text('count is positive')
     }
  }
  ```

- [循环渲染](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/arkts-rendering-control-foreach-0000001524537153-V3)：开发框架提供循环渲染（ForEach组件）来迭代数组，并为每个数组项创建相应的组件。

  ```typescript
  ForEach(
    arr: any[], // 用于迭代的数组
    itemGenerator: (item: any, index?: number) => void, // 生成子组件的lambda函数
    keyGenerator?: (item: any, index?: number) => string // 用于给定数组项生成唯一且稳定的键值
  )
  ```

### 2.组件状态管理装饰器和@Builder装饰器：

组件状态管理装饰器用来管理组件中的状态，它们分别是：@State、@Prop、@Link。

- [@State](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/arkts-state-0000001474017162-V3)装饰的变量是组件内部的状态数据，当这些状态数据<font color='red' style='background-color:' size=''>被修改时</font>，将会调用所在组件的<font color='red' style='background-color:' size=''>build方法进行UI刷新</font>。
- [@Prop](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/arkts-prop-0000001473537702-V3)与@State有相同的语义，但初始化方式不同。@Prop装饰的变量必须使用其父组件提供的@State变量进行初始化，允许组件内部修改@Prop变量，但更改不会通知给父组件，即<font color='red' style='background-color:' size=''>@Prop属于单向数据绑定</font>。
- [@Link](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/arkts-link-0000001524297305-V3)装饰的变量可以和父组件的@State变量建立双向数据绑定，需要注意的是：<font color='red' style='background-color:' size=''>@Link变量不能在组件内部进行初始化</font>。
- [@Builder](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/arkts-builder-0000001524176981-V3)装饰的方法用于定义组件的声明式UI描述，在一个自定义组件内快速生成多个布局内容。

@State、@Prop、@Link三者关系如图所示：

![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/10/20231204231439.png)

### 3.组件生命周期函数：

[自定义组件的生命周期函数](https://developer.harmonyos.com/cn/docs/documentation/doc-references-V3/arkts-custom-component-lifecycle-0000001482395076-V3)用于通知用户该自定义组件的生命周期，这些回调函数是私有的，在运行时由开发框架在特定的时间进行调用，不能从应用程序中手动调用这些回调函数。 右图是自定义组件生命周期的简化图示：

![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/10/20231204231829.png)

需要注意的是，部分生命周期回调函数<font color='red' style='background-color:' size=''>仅对@Entry修饰的自定义组件生效，它们分别是：onPageShow、onPageHide、onBackPress。</font>

## 完整示例

[gitee源码地址](https://gitee.com/harmonyos/codelabs/tree/master/RankingDemo)

### 代码结构解读

本篇Codelab只对核心代码进行讲解，对于完整代码，我们会在源码下载或gitee中提供。

No Preview

```
├──entry/src/main/ets               // 代码区    
│  ├──common                        // 公共文件目录
│  │  └──constants                  
│  │     └──Constants.ets           // 常量
│  ├──entryability
│  │  └──EntryAbility.ts            // 应用的入口
│  ├──model                         
│  │  └──DataModel.ets              // 模拟数据
│  ├──pages
│  │  └──RankPage.ets               // 入口页面
│  ├──view                          // 自定义组件目录
│  │  ├──ListHeaderComponent.ets
│  │  ├──ListItemComponent.ets
│  │  └──TitleComponent.ets
│  └──viewmodel        
│     ├──RankData.ets               // 实体类
│     └──RankViewModel.ets          // 视图业务逻辑类
└──entry/src/main/resources	    // 资源文件目录

```

### 使用@Link封装标题组件

在TitleComponent文件中，首先使用struct对象创建自定义组件，然后使用@Link修饰器管理TitleComponent组件内的状态变量isRefreshData，状态变量isRefreshData值发生改变后，通过@Link装饰器通知页面刷新List中的数据。

```typescript
// TitleComponent.ets
...
@Component
export struct TitleComponent {
  @Link isRefreshData: boolean; // 判断是否刷新数据
  @State title: Resource = $r('app.string.title_default');

  build() {
    Row() {
      ...
      Row() {
        Image($r('app.media.loading'))
          .height(TitleBarStyle.IMAGE_LOADING_SIZE)
          .width(TitleBarStyle.IMAGE_LOADING_SIZE)
          .onClick(() => {
            this.isRefreshData = !this.isRefreshData;
          })
      }
      .width(TitleBarStyle.WEIGHT)
      .height(WEIGHT)
      .justifyContent(FlexAlign.End)
    }
   ...
  }
}
```



![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/10/20231204232304.png)

### 封装列表头部样式组件

在ListHeaderComponent文件中，我们使用常规成员变量来设置自定义组件ListHeaderComponent的widthValue和paddingValue。

```typescript
// ListHeaderComponent.ets
...
@Component
export struct ListHeaderComponent {
  paddingValue: Padding | Length = 0;
  widthValue: Length = 0;

  build() {
    Row() {
      Text($r('app.string.page_number'))
        .fontSize(FontSize.SMALL)
        .width(ListHeaderStyle.LAYOUT_WEIGHT_LEFT)
        .fontWeight(ListHeaderStyle.FONT_WEIGHT)
        .fontColor($r('app.color.font_description'))
      Text($r('app.string.page_type'))
        .fontSize(FontSize.SMALL)
        .width(ListHeaderStyle.LAYOUT_WEIGHT_CENTER)
        .fontWeight(ListHeaderStyle.FONT_WEIGHT)
        .fontColor($r('app.color.font_description'))
      Text($r('app.string.page_vote'))
        .fontSize(FontSize.SMALL)
        .width(ListHeaderStyle.LAYOUT_WEIGHT_RIGHT)
        .fontWeight(ListHeaderStyle.FONT_WEIGHT)
        .fontColor($r('app.color.font_description'))
    }
    .width(this.widthValue)
    .padding(this.paddingValue)
  }
}
```

![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/10/20231204232848.png)

### 创建ListItemComponent

为了体现@Prop单向绑定功能，我们在ListItemComponent组件中添加了一个@Prop修饰的字段isSwitchDataSource，当通过点击改变ListItemComponent组件中isSwitchDataSource状态时，ListItemComponent作为List的子组件，并不会通知其父组件List刷新状态。

在代码中，我们使用@State管理ListItemComponent中的 isChange 状态，当用户点击ListItemComponent时，ListItemComponent组件中的文本颜色发生变化。我们使用条件渲染控制语句，创建的圆型文本组件。

```typescript
// ListItemComponent.ets
...
@Component
export struct ListItemComponent {
  index?: number;
  private name?: Resource;
  @Prop vote: string = '';
  @Prop isSwitchDataSource: boolean = false;
  // 判断是否改变ListItemComponent字体颜色
  @State isChange: boolean = false;

  build() {
    Row() {
      Column() {
        if (this.isRenderCircleText()) {
          if (this.index !== undefined) {
            this.CircleText(this.index);
          }
        } else {
          Text(this.index?.toString())
            .lineHeight(ItemStyle.TEXT_LAYOUT_SIZE)
            .textAlign(TextAlign.Center)
            .width(ItemStyle.TEXT_LAYOUT_SIZE)
            .fontWeight(FontWeight.BOLD)
            .fontSize(FontSize.SMALL)
        }
      }
      .width(ItemStyle.LAYOUT_WEIGHT_LEFT)
      .alignItems(HorizontalAlign.Start)

      Text(this.name)
        .width(ItemStyle.LAYOUT_WEIGHT_CENTER)
        .fontWeight(FontWeight.BOLDER)
        .fontSize(FontSize.MIDDLE)
        .fontColor(this.isChange ? ItemStyle.COLOR_BLUE : ItemStyle.COLOR_BLACK)
      Text(this.vote)
        .width(ItemStyle.LAYOUT_WEIGHT_RIGHT)
        .fontWeight(FontWeight.BOLD)
        .fontSize(FontSize.SMALL)
        .fontColor(this.isChange ? ItemStyle.COLOR_BLUE : ItemStyle.COLOR_BLACK)
    }
    .height(ItemStyle.BAR_HEIGHT)
    .width(WEIGHT)
    .onClick(() => {
      this.isSwitchDataSource = !this.isSwitchDataSource;
      this.isChange = !this.isChange;
    })
  }
  ...
}

```

![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/10/20231204233120.gif)



### 创建RankList

为了简化代码，提高代码的可读性，我们使用@Builder描述排行列表布局内容，使用循环渲染组件ForEach创建ListItem。

```typescript
// RankPage.ets
...
  build() {
    Column() {
      // 顶部标题组件
      TitleComponent({ isRefreshData: $isSwitchDataSource, title: TITLE })
      // 列表头部样式
      ListHeaderComponent({
        paddingValue: { 
          left: Style.RANK_PADDING,
          right: Style.RANK_PADDING 
        },
        widthValue: Style.CONTENT_WIDTH
      })
        .margin({ 
          top: Style.HEADER_MARGIN_TOP,
          bottom: Style.HEADER_MARGIN_BOTTOM 
        })
      // 列表区域
      this.RankList(Style.CONTENT_WIDTH)
    }
    .backgroundColor($r('app.color.background'))
    .height(WEIGHT)
    .width(WEIGHT)
  }

  @Builder RankList(widthValue: Length) {
    Column() {
      List() {
        ForEach(this.isSwitchDataSource ? this.dataSource1 : this.dataSource2,
          (item: RankData, index?: number) => {
            ListItem() {
              ListItemComponent({ index: (Number(index) + 1), name: item.name, vote: item.vote,
                isSwitchDataSource: this.isSwitchDataSource
              })
            }
          }, (item: RankData) => JSON.stringify(item))
      }
      .width(WEIGHT)
      .height(Style.LIST_HEIGHT)
      .divider({ strokeWidth: Style.STROKE_WIDTH })
    }
    .padding({ 
      left: Style.RANK_PADDING,
      right: Style.RANK_PADDING 
    })
    .borderRadius(Style.BORDER_RADIUS)
    .width(widthValue)
    .alignItems(HorizontalAlign.Center)
    .backgroundColor(Color.White)
  }
...
```

![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/10/20231204233348.png)



### 使用自定义组件生命周期函数

我们通过点击系统导航返回按钮来演示onBackPress回调方法的使用，在指定的时间段内，如果满足退出条件，onBackPress将返回false，系统默认关闭当前页面。否则，提示用户需要再点击一次才能退出，同时onBackPress返回true，表示用户自己处理导航返回事件。

```typescript
// RankPage.ets
... 
@Entry
@Component
struct RankPage {
  ...
  onBackPress() {
    if (this.isShowToast()) {
      prompt.showToast({
        message: $r('app.string.prompt_text'),
        duration: TIME
      });
      this.clickBackTimeRecord = new Date().getTime();
      return true;
    }
    return false;
  }
  ...
}
```

## 总结

您已经完成了本次Codelab的学习，并了解到以下知识点：

1. 条件渲染、循环渲染语法的使用。
2. @State、@Prop、@Link修饰器的使用。
3. @Builder修饰器的使用。
4. 自定义组件生命周期函数onBackPress的调用。