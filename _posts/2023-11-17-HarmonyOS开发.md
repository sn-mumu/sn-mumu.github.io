---
title: HarmonyOS开发
author: mumu
date: 2023-11-17 15:10:00 +0800
categories: [华为,HarmonyOS]
tags: [华为,HarmonyOS]
pin: false
---

# HarmonyOS开发

## 1 运行Hello World

### 1.1. 了解基本工程目录

#### 1.1.1. 工程级目录

工程的目录结构如下。

![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/10/202311171511860.png)

1. AppScope中存放应用全局所需要的资源文件。

   + AppScope>resources>base

     ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/10/202311171515180.png)

     + element文件夹主要存放公共的字符串、布局文件等资源
     + media存放全局公共的多媒体资源文件

   + AppScope>app.json5是应用的全局的配置文件，用于存放应用公共的配置信息

     ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/10/202311171519294.png)

     - bundleName是包名。
     - vendor是应用程序供应商。
     - versionCode是用于区分应用版本。
     - versionName是版本号。
     - icon对应于应用的显示图标。
     - label是应用名

2. entry是应用的主模块，存放HarmonyOS应用的代码、资源等。

   ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/10/202311171516136.png)

   + entry>src目录中主要包含总的main文件夹，单元测试目录ohosTest，以及模块级的配置文件

     + main文件夹中

       + ets文件夹用于存放ets代码

         ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/10/202311171517842.png)

         + entryability存放ability文件，用于当前ability应用逻辑和生命周期管理
         + pages存放UI界面相关代码文件，初始会生成一个Index页面

       + resources文件存放模块内的多媒体及布局文件等，module.json5文件为模块的配置文件

         ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/10/202311171518840.png)

         + src/main/resources/base/profile/main_pages.json文件保存的是页面page的路径配置信息，所有需要进行路由跳转的page页面都要在这里进行配置。

           ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/10/202311171524104.png)

       + entry>src>main>module.json5是模块的配置文件，包含当前模块的配置信息

         ![img](https://raw.githubusercontent.com/sn-mumu/cloud-storage/main/PicGo/2023/10/202311171521482.png)

         其中module对应的是模块的配置信息，一个模块对应一个打包后的hap包，hap包全称是HarmonyOS Ability Package，其中包含了ability、第三方库、资源和配置文件。其具体属性及其描述可以参照下表1

         | 属性                | 描述                                                         |
         | ------------------- | ------------------------------------------------------------ |
         | name                | 该标签标识当前module的名字，module打包成hap后，表示hap的名称，标签值采用字符串表示（最大长度31个字节），该名称在整个应用要唯一。 |
         | type                | 表示模块的类型，类型有三种，分别是entry、feature和har。      |
         | srcEntry            | 当前模块的入口文件路径。                                     |
         | description         | 当前模块的描述信息。                                         |
         | mainElement         | 该标签标识hap的入口ability名称或者extension名称。只有配置为mainElement的ability或者extension才允许在服务中心露出。 |
         | deviceTypes         | 该标签标识hap可以运行在哪类设备上，标签值采用字符串数组的表示。 |
         | deliveryWithInstall | 标识当前Module是否在用户主动安装的时候安装，表示该Module对应的HAP是否跟随应用一起安装。- true：主动安装时安装。- false：主动安装时不安装。 |
         | installationFree    | 标识当前Module是否支持免安装特性。- true：表示支持免安装特性，且符合免安装约束。- false：表示不支持免安装特性。 |
         | pages               | 对应的是main_pages.json文件，用于配置ability中用到的page信息。 |
         | abilities           | 是一个数组，存放当前模块中所有的ability元能力的配置信息，其中可以有多个ability。 |

         对于abilities中每一个ability的属性项，其描述信息如下表2

         | 属性                  | 描述                                                         |
         | --------------------- | ------------------------------------------------------------ |
         | name                  | 该标签标识当前ability的逻辑名，该名称在整个应用要唯一，标签值采用字符串表示（最大长度127个字节）。 |
         | srcEntry              | ability的入口代码路径。                                      |
         | description           | ability的描述信息。                                          |
         | icon                  | ability的图标。该标签标识ability图标，标签值为资源文件的索引。该标签可缺省，缺省值为空。如果ability被配置为MainElement，该标签必须配置。 |
         | label                 | ability的标签名。                                            |
         | startWindowIcon       | 启动页面的图标。                                             |
         | startWindowBackground | 启动页面的背景色。                                           |
         | visible               | ability是否可以被其他应用程序调用，true表示可以被其它应用调用， false表示不可以被其它应用调用。 |
         | skills                | 标识能够接收的意图的action值的集合，取值通常为系统预定义的action值，也允许自定义。 |
         | entities              | 标识能够接收的Want的Action值的集合，取值通常为系统预定义的action值，也允许自定义。 |
         | actions               | 标识能够接收Want的Entity值的集合。                           |

     + ohosTest是单元测试目录

     + build-profile.json5是模块级配置信息，包括编译构建配置项

     + hvigorfile.ts文件是模块级构建脚本

     + oh-package.json5是模块级依赖配置信息文件

3. oh_modules是工程的依赖包，存放工程依赖的源文件。

4. build-profile.json5是工程级配置信息，包括签名、产品配置等。

5. hvigorfile.ts是工程级编译构建任务脚本，hvigor是基于任务管理机制实现的一款全新的自动化构建工具，主要提供任务注册编排，工程模型管理、配置管理等核心能力。

6. oh-package.json5是工程级依赖配置文件，用于记录引入包的配置信息。