---
title: HarmonyOS开发 构建更加丰富的页面
author: mumu
date: 2023-12-14 22:30:00 +0800
categories: [华为,HarmonyOS]
tags: [华为,HarmonyOS,构建更加丰富的页面]
pin: false
---

# 构建更加丰富的页面

> 本课程是基于HarmonyOS 3.1及以上版本的新技术和特性所推出的系列化课程，每个课程单元里面都包含视频、Codelab、文章和习题，帮助您快速掌握HarmonyOS的应用开发； 通过本章节的学习，你可以了解组件状态管理的相关知识点，并进一步的学习一些常用的组件如video和弹窗，来构建更加丰富的页面。

## 管理组件状态

### 概述

在应用中，界面通常都是动态的。如图1所示，在子目标列表中，当用户点击目标一，目标一会呈现展开状态，再次点击目标一，目标一呈现收起状态。界面会根据不同的状态展示不一样的效果。

**图1** 展开/收起目标项
![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/10/20231214222800.gif)

ArkUI作为一种声明式UI，具有状态驱动UI更新的特点。当用户进行界面交互或有外部事件引起状态改变时，状态的变化会触发组件自动更新。所以在ArkUI中，我们只需要通过一个变量来记录状态。当改变状态的时候，ArkUI就会自动更新界面中受影响的部分。

![点击放大](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/20231214223725.png)

ArkUI框架提供了多种管理状态的装饰器来修饰变量，使用这些装饰器修饰的变量即称为状态变量。

在组件范围传递的状态管理常见的场景如下：

| **场景**               | **装饰器**                                                   |
| ---------------------- | ------------------------------------------------------------ |
| 组件内的状态管理       | <font color='red' style='background-color:' size=''>@State</font> |
| 从父组件单向同步状态   | <font color='red' style='background-color:' size=''>@Prop</font> |
| 与父组件双向同步状态   | <font color='red' style='background-color:' size=''>@Link</font> |
| 跨组件层级双向同步状态 | <font color='red' style='background-color:' size=''>@Provide和@Consume</font> |

在组件内使用@State装饰器来修饰变量，可以使组件根据不同的状态来呈现不同的效果。若当前组件的状态需要通过其父组件传递而来，此时需要使用@Prop装饰器；若是父子组件状态需要相互绑定进行双向同步，则需要使用@Link装饰器。使用@Provide和@Consume装饰器可以实现跨组件层级双向同步状态。

在实际应用开发中，应用会根据需要封装数据模型。<font color='red' style='background-color:' size=''>如果需要观察嵌套类对象属性变化，需要使用@Observed和@ObjectLink装饰器</font>，因为上述表格中的装饰器只能观察到对象的第一层属性变化。@Observed和@ObjectLink装饰器的具体使用方法可参考[@Observed装饰器和@ObjectLink装饰器：嵌套类对象属性变化](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/arkts-observed-and-objectlink-0000001473697338-V3?catalogVersion=V3)。

另外，当状态改变，需要<font color='red' style='background-color:' size=''>对状态变化进行监听做一些相应的操作时，可以使用@Watch装饰器来修饰状态</font>。

### 组件内的状态管理：<font color='red' style='background-color:' size=''>@State</font>

实际开发中由于交互，组件的内容呈现可能产生变化。当需要在组件内使用状态来控制UI的不同呈现方式时，可以使用@State装饰器。以任务管理应用为例，当点击子目标列表的其中一项，列表项会展开。当再次点击同一项，列表项会收起。所以，对于某一个列表项来说，它的呈现方式会受列表项是否展开这个状态影响。

**图2** 展开/收起目标项
![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/20231214233320.gif)

将是否展开这个状态定义为isExpanded变量，当其值为false表示目标项收起，值为true时表示目标项展开。

此时，需要使用@State装饰器修饰isExpanded，使其成为目标项内部的状态变量。通过@State装饰后，框架内部会建立数据与视图间的绑定，

当isExpanded状态变化时，目标项会随之展开或收起。

**图3** 定义是否展开状态
![点击放大](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/20231214233337.png)

其具体实现只要用@State修饰isExpanded变量，定义是否展开状态。然后通过条件渲染，实现是否显示进度调整面板和列表项的高度变化。最后，监听列表项的点击事件，在onClick回调中改变isExpanded状态。

这样就实现了对相同列表项点击时，列表项的展开和收起功能。当用户反复点击同一个列表项时，组件内的isExpanded状态变化，列表项会自动更新。

```typescript
@Component
export default struct TargetListItem {
  @State isExpanded: boolean = false;
  ...

  build() {
    ...
      Column() {
        ...
        if (this.isExpanded) {
          Blank()
          ProgressEditPanel(...)
        }
      }
      .height(this.isExpanded ? $r('app.float.expanded_item_height')                  
      : $r('app.float.list_item_height'))
      .onClick(() => {
        ...
             this.isExpanded = !this.isExpanded;
        ...
       })
    ...
  }
}
```

## 从父组件单向同步状态：<font color='red' style='background-color:' size=''>@Prop</font>



当子组件中的状态依赖从父组件传递而来时，需要使用@Prop装饰器，@Prop修饰的变量可以和其父组件中的状态建立单向同步关系。<font color='red' style='background-color:' size=''>当父组件中状态变化时，该状态值也会更新至@Prop修饰的变量；对@Prop修饰的变量的修改不会影响其父组件中的状态</font>。

**图4** 列表的编辑模式
![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/20231214233433.gif)

如图4所示，在目标管理应用中，当用户点击子目标列表的“编辑”文本，列表进入编辑模式，点击取消，列表退出编辑模式。

整个列表是自定义组件TargetList，顶部是文本显示区域，主要是Text组件，底部是一个Button组件。中间区域则是用来显示每个目标项，目标项是自定义组件TargetListItem。

从图中可以看出，TargetListItem是TargetList的子组件。TargetList是TargetListItem父组件。

**图5** TargetList和TargetListItem
![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/20231214233632.png)

对于父组件TargetList，其顶部显示的文本和底部按钮会随编辑模式的变化而变化，因此父组件拥有编辑模式状态。

对于子组件TargetListItem，其最右侧是否预留位置和显示勾选框也会随编辑模式变化，因此子组件也拥有编辑模式状态。

但是是否进入编辑模式，其触发点是在用户点击列表的“编辑”或取消按钮，状态变化的源头仅在于父组件TargetList。当父组件TargetList中的编辑模式变化时，子组件TargetListItem的编辑模式状态需要随之变化。

**图6** 从父组件单向同步isEditMode状态
![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/20231214233639.png)

在父组件TargetList中可以定义一个是否进入编辑模式的状态，即用@State修饰isEditMode。<font color='red' style='background-color:' size=''>@State修饰的变量不仅是组件内部的状态，也可以作为子组件单向或双向同步的数据源</font>。ArkUI提供了@Prop装饰器，@Prop修饰的变量可以和其父组件中的状态建立单向同步关系，所以用@Prop修饰子组件TargetListItem中的isEditMode变量。

在父组件TargetList中，用@State修饰isEditMode，定义编辑模式状态。然后利用条件渲染实现根据是否进入编辑模式，显示不同的文本和按钮。同时，在父组件中需要在用户点击时改变状态，触发界面更新。

当点击“编辑”事件发生时，进入编辑模式，显示取消、全选文本和勾选框，同时显示删除按钮；当点击“取消”事件发生时，退出编辑模式，显示“编辑”文本和“添加子目标”按钮。

```typescript
@Component
export default struct TargetList {
  @State isEditMode: boolean = false;
  ...
  build() {
    Column() {
      Row() {
        ...
          if (this.isEditMode) {
            Text($r('app.string.cancel_button'))
              .onClick(() => {
                this.isEditMode = false;
                ...
               })
               ...
            Text($r('app.string.select_all_button'))...
            Checkbox()...
          } else {
            Text($r('app.string.edit_button'))
              .onClick(() => {
                this.isEditMode = true;
              })
              ...
          }
        ...
      }
      ...
      List({ space: CommonConstants.LIST_SPACE }) {
        ForEach(this.targetData, (item: TaskItemBean, index: number) => {
          ListItem() {
            TargetListItem({
              isEditMode: this.isEditMode,
              ...
            })
          }
        }, (item, index) => JSON.stringify(item) + index)
      }
      ...
      if (this.isEditMode) {
        Button($r('app.string.delete_button'))
      } else {
        Button($r('app.string.add_task'))
      }
    }
    ...
  }
}
```

在子组件TargetListItem中，使用@Prop修饰子组件的isEditMode变量，定义子组件的编辑模式状态。然后同样根据是否进入编辑模式，控制目标项最右侧是否预留位置和显示勾选框。

```typescript
@Component
export default struct TargetListItem {
   @Prop isEditMode: boolean;
   ...
       Column() {
        ...
       }
       .padding({
        ...
        right: this.isEditMode ? $r('app.float.list_edit_padding') 
               : $r('app.float.list_padding')
       })
       ...

       if (this.isEditMode) {
        Row() {
           Checkbox()...
        }
       }
  ...
}
```

最后，最关键的一步就是要在父组件中使用子组件时，将父组件的编辑模式状态this.isEditMode传递给子组件的编辑模式状态isEditMode。

```typescript
@Component
export default struct TargetList {
  @State isEditMode: boolean = false;
  ...
  build() {
    Column() {
      ...
      List({ space: CommonConstants.LIST_SPACE }) {
        ForEach(this.targetData, (item: TaskItemBean, index: number) => {
          ListItem() {
            TargetListItem({
              isEditMode: this.isEditMode,
              ...
            })
          }
        }, (item, index) => JSON.stringify(item) + index)
      }
      ...
    }
    ...
  }
}
```

## 与父组件双向同步状态：<font color='red' style='background-color:' size=''>@Link</font>



若是父子组件状态需要相互绑定进行双向同步时，可以使用@Link装饰器。<font color='red' style='background-color:' size=''>父组件中用于初始化子组件@Link变量的必须是在父组件中定义的状态变量</font>。

**图7** 切换目标项
![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/20231214234145.gif)

在目标管理应用中，当用户点击同一个目标，目标项会展开或者收起。当用户点击不同的目标项时，除了被点击的目标项展开，同时前一次被点击的目标项会收起。

如图7所示，当目标一展开时，点击目标三，目标三会展开，同时目标一会收起。再点击目标一时，目标一展开，同时目标三会收起。

从目标一切换到目标三的流程中，关键在于最后目标一的收起，当点击目标三时，目标一需要知道点击了目标三，目标一才会收起。

**图8** 子目标列表目标项位置索引
![点击放大](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/20231214234559.png)

在子目标列表中，每个列表项都有其位置索引值index属性，表示目标项在列表中的位置。index从0开始，即第一个目标项的索引值为0，第二个目标项的索引值为1，以此类推。此外，clickIndex用来记录被点击的目标项索引。当点击目标一时，clickIndex为0，点击目标三时，clickIndex为2。

在父组件子目标列表和每个子组件目标项中都拥有clickIndex状态。当目标一展开时，clickIndex为0。此时点击目标三，目标三的clickIndex变为2，只要其父组件子目标列表感知到clickIndex状态变化，同时将此变化传递给目标一。目标一的clickIndex即可同步改变为2，即目标一感知到此时点击了目标三。

**图9** 与父组件双向同步clickIndex状态
![点击放大](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/12/20231214234947.png)

将列表和目标项对应到列表组件TargetList和列表项TargetListItem。首先，需要在父组件TargetList中定义clickIndex状态。

若此时子组件中的clickIndex用@Prop装饰器修饰，当子组件中clickIndex变化时，父组件无法感知，因为@Prop装饰器建立的是从父组件到子组件的单向同步关系。

ArkUI提供了@Link装饰器，用于与父组件双向同步状态。当子组件TargetListItem中的clickIndex用@Link修饰，可与父组件TargetList中的clickIndex建立双向同步关系。

```typescript
@Component
export default struct TargetList {
  @State clickIndex: number = CommonConstants.DEFAULT_CLICK_INDEX;
  ...
             TargetListItem({
               clickIndex: $clickIndex,
              ...
             })
  ...
}
```

首先，在父组件TargetList中用@State装饰器定义点击的目标项索引状态。然后，在子组件TargetListItem中用@Link装饰器定义clickIndex，当点击目标项时，clickIndex更新为当前目标索引值。

完成在父子组件中定义状态后，最关键的就是要建立父子组件的双向关联关系。在父组件中使用子组件时，将父组件的clickIndex传递给子组件的clickIndex。其中父组件的clickIndex加上$表示传递的是引用。

```
@Componentexport default struct TargetListItem {  @Link @Watch('onClickIndexChanged') clickIndex: number;  @State isExpanded: boolean = false  ...
  onClickIndexChanged() {    if (this.clickIndex != this.index) {      this.isExpanded = false;    }  }
  build() {    ...       Column() {        ...       }       .onClick(() => {        ...           this.clickIndex = this.index;        ...       })    ...  }}
```

当目标一感知到点击了目标三时，还需要将目标一收起，切换列表项的功能才是完整的。此时，目标一感知到clickIndex变为2，需要判断与目标一本身的位置索引值0不相等，从而将目标一收起。此时，就需要用到ArkUI中监听状态变化@Watch的能力。用@Watch修饰的状态，当状态发生变化时，会触发声明时定义的回调。

我们给TargetListItem的中的clickIndex状态加上@Watch("onClickIndexChanged")。这表示需要监听clickIndex状态的变化。当clickIndex状态变化时，将触发onClickIndexChanged回调：如果点击的列表项索引不等于当前列表项索引，则将isExpanded状态置为false，从而收起该目标项。

## 跨组件层级双向同步状态：@Provide和@Consume



![img](https://alliance-communityfile-drcn.dbankcdn.com/FileServer/getFile/cmtyPub/011/111/111/0000000000011111111.20231212100608.71049668491557943564051136365840:50001231000000:2800:B05B185D667CB73847F664FC7F3A6DB6CD9C42C76898D8A40A67A4E93445372D.png?needInitFileName=true?needInitFileName=true)

跨组件层级双向同步状态是指@Provide修饰的状态变量自动对提供者组件的所有后代组件可用，后代组件通过使用@Consume装饰的变量来获得对提供的状态变量的访问。@Provide作为数据的提供方，可以更新其子孙节点的数据，并触发页面渲染。@Consume在感知到@Provide数据的更新后，会触发当前自定义组件的重新渲染。

使用@Provide的好处是开发者不需要多次将变量在组件间传递。@Provide和@Consume的具体使用方法请参见开发指南：[@Provide装饰器和@Consume装饰器：与后代组件双向同步](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/arkts-provide-and-consume-0000001473857338-V3?catalogVersion=V3)。

## 参考



更多状态管理场景和相关知识请参考开发指南：[状态管理](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/arkts-state-management-overview-0000001524537145-V3?catalogVersion=V3)。