# HarmonyOS NEXT开发实战教程-记账app
今天分享的实战教程是一款记账app，最近分享的项目都是纯页面，没有服务端，没有数据接口，因为鸿蒙开发主要就是写页面，都是前端嘛。如果有友友想要完整的项目可以找幽蓝君定制，想学服务端开发的话幽蓝君也可以写。

话不多说，看一下记账app的效果图：

![image](https://github.com/user-attachments/assets/3ea9a6e4-596c-4119-9414-00b9db38bb19)

下面为大家分享本项目的开发教程，其实主要是首页部分，第二个页面比较简单。

幽蓝君每次都要重复一下，我们拿到一个项目或者一个页面，要先分析它的大体结构然后再着手写代码。

首页部分主要是导航栏和一个List组件，导航栏部分比较简单就不啰嗦了，直接看主要内容部分。

最上面是一个余额卡片，首先注意它的背景是一个渐变色，这里可以用图片，也可以设置背景颜色，我用的背景颜色，渐变颜色的属性是radialGradient，下面是渐变颜色的相关代码：

```
Column(){

}
.radialGradient({
  //坐标
   center: [0, 220],
   //半径
   radius: 400,
   //重复
   repeating: false,
   //颜色
   colors: [[0x5D65EA, 0.0],[0x5D65EA, 0.6], [0x9C64FE, 1]] // 数组末尾元素占比小于1时满足重复着色效果
})

```

积分兑换部分有两个等宽的按钮，按照幽蓝君以前写iOS的思维习惯计算每一个的宽度，但是在鸿蒙里是不需要的，使用弹性布局就可以很快速的实现：

```
Flex({wrap:FlexWrap.NoWrap,direction:FlexDirection.Row}){
              Row(){
                Image($r('app.media.jf'))
                  .width(28)
                  .height(28)
                  .objectFit(ImageFit.Auto)

                Text('积分兑换')
                  .fontColor('#475869')
                  .fontSize(14)
                  .margin({left:10})
              }
              .width('100%')
              .height(48)
              .borderRadius(24)
              .backgroundColor(Color.White)
              .alignItems(VerticalAlign.Center)
              .justifyContent(FlexAlign.Center)
              Row(){
                Image($r('app.media.jf'))
                  .width(28)
                  .height(28)
                  .objectFit(ImageFit.Auto)
                Text('积分兑换')
                  .fontColor('#475869')
                  .fontSize(14)
                  .margin({left:10})
              }
              .width('100%')
              .height(48)
              .borderRadius(24)
              .backgroundColor(Color.White)
              .alignItems(VerticalAlign.Center)
              .justifyContent(FlexAlign.Center)
              .margin({left:13})
            }
            .width('100%')
```


VIP卡部分有个左右滑动效果，这个可以使用scroll组件，相关代码如下：

```
Scroll(){
              Row({space:15}){
                CardItem()
                CardItem()
                CardItem()
              }
            }
            .scrollBar(BarState.Off)
            .scrollable(ScrollDirection.Horizontal)

```

以上就是这个项目的一些难点，其他的没什么好说的，很简单。
