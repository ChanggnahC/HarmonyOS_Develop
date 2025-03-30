# 鸿蒙NEXT开发实战教程：仿抖音短视频
今天的实战教程是简单模仿一下抖音短视频，主要是首页部分的内容，先看效果图:
![image](https://github.com/user-attachments/assets/f2c87784-15c2-47cc-86ba-6ddcde9bea80)


下面为大家讲解这个项目的详细教程。

tabbar

Tabbar的难点在于中间有个发布按钮，思路是我们可以在tabbar里加个判断，中间按钮使用图片，其余按钮使用文字。相关代码如下：

```
@State arr: Array<string> =  ['首页', '朋友', '发布', '消息', '我'];

  @Builder TabBuilder(index: number) {
    Column() {
          if (index === 2) {
            Image($rawfile('add.png'))
              .width($r('app.float.width_small'))
          } else {
            Text(`${this.arr[index]}`)
              .fontColor(this.currentIndex === index ? Color.Red : Color.Gray)
              .fontSize(17)
              .fontWeight(500)
          }
    }
    .width('100%')
    .height('100%')
    .alignItems(HorizontalAlign.Center)
    .justifyContent(FlexAlign.Center)
    .backgroundColor(Color.Black)
  }
```
导航栏

这里的导航栏是透明的，而且不占用空间，覆盖在视频之上，所以就不能使用系统的Navigation，我们要自定义一个导航栏。导航栏的内容有两边的图片按钮，还有中间可以滑动的菜单列表，实现代码如下：

```
Row(){
        Row(){
          Image($r('app.media.l_more'))
            .width(25)
            .height(25)
        }
        .height(56)
        .width(this.screenWidth/7)
        .justifyContent(FlexAlign.Center)
        .alignItems(VerticalAlign.Center)

        YLTabbar()
          .width(5*this.screenWidth/7)

        Row(){
          Image($r('app.media.find'))
            .width(25)
            .height(25)
        }
        .width(this.screenWidth/7)
        .height(56)
        .width(this.screenWidth/7)
        .justifyContent(FlexAlign.Center)
        .alignItems(VerticalAlign.Center)

      }
      .margin({top:this.topHeight})
      .height(56)
```


播放视频

本项目使用的视频资源是本地文件，存放在rawfile文件夹下，鸿蒙系统提供了Video组件来播放视频，具体使用方法如下：

```
Video({
          src: this.videoResource,
          controller: this.controller
        })
          .height('100%')
          .autoPlay(true)
          .controls(false)
          .objectFit(ImageFit.Cover)
          .loop(true)
```


视频翻页

幽蓝君起初尝试过使用很多组件来实现翻页效果，比如List、Grid等等，最后发现swiper组件的效果最好，也最简单，只是要进行一些属性上的设置，比如动画曲线、循环模式等等，具体代码如下：

```
Swiper(this.swiperController){
        SingleRow({videoResource:$rawfile('video1.mp4')})
        SingleRow({videoResource:$rawfile('video2.mp4')})
        SingleRow({videoResource:$rawfile('video3.mp4')})
      }
      .index(videoIndex) // 设置当前在容器中显示的子组件的索引值
      .width('100%')
      .height('100%')
      .autoPlay(false)
      .indicator(false)
      .loop(true)
      .duration(200) // 子组件切换的动画时长
      .cachedCount(0)
      .vertical(true)
      .itemSpace(0)
      /**
       * 弹性曲线产生自然的弹簧效果，四个参数分别对应附着在弹簧上的对象的初始速度、附着在弹簧上的对象的质量、单位形变量所需弹力的大小、
       * 弹簧在振动过程中的减震力，使得弹簧振幅逐渐减小直至停止在平衡位置
       */
      .curve(curves.interpolatingSpring(-1, 1, 328, 34))
      .onChange((index) => {
        this.index = index;
        this.playBoo = true;
        videoIndex = index;
      })
```


以上就是本项目中的一些难点
