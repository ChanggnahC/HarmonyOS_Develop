# 一个伟大的鸿蒙组件：万能加载动画
大家在日常开发中肯定少不了使用各种加载动画和提示弹窗，比如网络加载动画，加载或操作成功的提示弹窗，失败弹窗，表单输入提示弹窗等等。

这些在iOS和安卓上都有非常著名非常成熟的开源框架，所以幽蓝君想做一款属于鸿蒙的开源框架，并且我要用一行代码实现它。先看效果图：
![image](https://github.com/user-attachments/assets/7e113ae5-fb75-4e7e-bb37-6727d0f8073e)


这里给大家提供了四种模式的弹窗，分别是加载动画弹窗、文字提示弹窗、成功提示弹窗和失败提示弹窗。这款组件我给它命名为YLLoadingHUD。

引入方式：

1、后台发送‘鸿蒙加载动画’获取项目源码。将ets文件夹下的YLLoadingHUD文件夹拖到自己的项目中。

2、在需要使用的文件中引入：

import YLHud, { HUDClass, HUDMode } from '../YLLoadingHUD/YLLoadingHUD'
3、定义一个对象变量

```
@State hudItem:HUDClass = {
    show:false,
    mode:HUDMode.loading,
    string:'loading'
  }
```
4、添加动画组件：

YLHud({hudItem:$hudItem})
展示方式：

当你需要弹窗时，只需要设置

this.hudItem.show = true
同理，想让弹窗消失时设置

this.hudItem.show = false
模式介绍：

1、默认模式：HUDMode.loading

![image](https://github.com/user-attachments/assets/e399936d-3e11-421d-9a5a-66735d7d1678)

你可以这样设置使用默认模式：

```
this.hudItem.mode = HUDMode.loading
this.hudItem.string = "loading"
this.hudItem.show = true
```
弹窗大小会根据string的内容自适应，string不是必填项,当string传空时，不显示文字，效果图如下：
![image](https://github.com/user-attachments/assets/7b103934-bce0-40f2-a0dd-252849543ed2)


2、文字提示弹窗：HUDMode.string
![image](https://github.com/user-attachments/assets/89c51d1c-6992-45b1-9a53-ff88d730eefc)

使用方法：

```
this.hudItem.mode = HUDMode.string
this.hudItem.string = "This is a tip"
this.hudItem.show = true
```


此模式string不能为空，且弹窗不会自动消失。

3、成功提示弹窗：HUDMode.success
![image](https://github.com/user-attachments/assets/c124ecb4-a36f-4064-8bb5-49078378ced0)

使用方法：

```
this.hudItem.mode = HUDMode.success
this.hudItem.string = "success"
this.hudItem.show = true
```


此模式2秒会自动隐藏，string非必填。

3、失败提示弹窗：HUDMode.error
![image](https://github.com/user-attachments/assets/419cf585-bc9c-44b7-95e3-35bc7f75cc4a)

使用方法：

```
this.hudItem.mode = HUDMode.error
this.hudItem.string = "error"
this.hudItem.show = true
```
此模式2秒会自动隐藏，string非必填。

完整demo

```
import YLHud, { HUDClass, HUDMode } from '../YLLoadingHUD/YLLoadingHUD'

@Entry
@Component
struct Index {

  @State message: string = 'YLLoadingHUD'
  @State hudItem:HUDClass = {
    show:false,
    mode:HUDMode.loading,
    string:'loading'
  }

  build() {
    Stack(){
      Row() {
        Flex({direction:FlexDirection.Column,justifyContent:FlexAlign.End,alignItems:ItemAlign.Center}) {
          Text(this.message)
            .fontSize(40)
            .fontWeight(FontWeight.Bold)

          Button("loading")
            .margin({top:100})
            .onClick(()=>{
              this.hudItem.mode = HUDMode.loading
              this.hudItem.string = ""
              this.hudItem.show = true
            })
          Button("string")
            .margin({top:20})
            .onClick(()=>{
              this.hudItem.mode = HUDMode.string
              this.hudItem.string = "This is a tip"
              this.hudItem.show = true

            })

          Button("success")
            .margin({top:20})
            .onClick(()=>{
              this.hudItem.mode = HUDMode.success
              this.hudItem.string = "success"
              this.hudItem.show = true
            })
          Button("error")
            .margin({top:20})
            .onClick(()=>{
              this.hudItem.mode = HUDMode.error
              this.hudItem.string = "error"
              this.hudItem.show = true

            })
          Button("dismiss")
            .margin({top:20})
            .onClick(()=>{
              this.hudItem.show = false
            })
        }
        .width('100%')
      }
      .height('100%')
      YLHud({hudItem:$hudItem})
    }
  }
}
```
如果觉得好用，希望大家口口相传，幽蓝君一定再接再厉，做大做强。
