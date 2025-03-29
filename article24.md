# HarmonyOS Next实战教程：实现中间凹陷的异形tabbar
今天要和大家分享的实战案例是实现中间凹陷的tabar
![image](https://github.com/user-attachments/assets/12703f91-1339-49aa-87c3-84f0f6cd8daf)

前些天在做墨迹天气的时候看到了这种异形的tabbar，看起来比较有挑战性，因为鸿蒙版的墨迹天气app还没有这个东西，我决定尝试做一下。

系统的Tabs肯定是不行了，我们需要自定义。

难度直接拉满，直接做最难的部分，就是这个中间有凹陷的矩形，这怎么整呢？

![image](https://github.com/user-attachments/assets/b67cd206-5b9f-4170-8220-85d35e84582d)


我们一点一点来，先不管矩形，先尝试画一条上边的曲线，这里需要使用三次贝塞尔曲线路径方法bezierCurveTo。

要画贝塞尔曲线，就要知道线上的坐标，尤其是曲线部分，坐标越多越好。

那么问题来了，直线部分的坐标很容易知道，曲线部分的坐标是多少呢？

我们需要做一道几何题，看一下这个曲线，它像是一个半圆又不像是一个半圆。我把它分解了一下，可以看到它是由三个同样大小的圆的三个部分拼凑起来，这三个圆的排列也比较有规律。

![image](https://github.com/user-attachments/assets/adb935ce-8cee-4b3a-828f-4ca068de7bf1)


我们根据笛卡尔坐标系，计算出圆上的坐标点，越多越好。

我把这段曲线分为相等的4段，每一段都获取24个坐标点：

```
//精确度
let accuracy = 24
let finalP:Point[] = []
for (let index = 0;  index <=  j1_x; index+=(j1_x/accuracy)) {
  let pp = this.calculateYFromX(circle1.x,circle1.y,circleR,start_left_x + index,true)
  finalP.push(pp)
}
for (let index = j1_x;  index <=  middleSize/2 + j1_x; index+=(j1_x/(accuracy*2))) {
  let pp = this.calculateYFromX(circle2.x,circle2.y,circleR,start_left_x + index,false)
  finalP.push(pp)
}
for (let index = middleSize/2  + j1_x;  index <=  middleSize; index+=(j1_x/accuracy)) {
  let pp = this.calculateYFromX(circle3.x,circle3.y,circleR,start_left_x + index,true)
  finalP.push(pp)
}

```

这样曲线部分的坐标就有了。

现在我们开始画线，先画左边的直线：

```
@State padding_top:number = 6
this.context.lineTo(start_left_x,this.padding_top)
```


这里说一下y坐标为什么不是0，因为这个tabbar上方还有一些阴影，要把阴影这一部分留出来。

再画曲线部分：

```
for(let i = 1;i < finalP.length - 2;i+=3){
  this.context.bezierCurveTo(
    finalP[i].x,finalP[i].y,
    finalP[i+1].x,finalP[i+1].y,
    finalP[i+2].x,finalP[i+2].y,
  );
}
```


再画右边的直线：

```
this.context.lineTo(this.screen_width,this.padding_top)

``

同样的把其他三个边也都画线形成闭环。

接下来我们就可以进行填充：

```
this.context.fillStyle = '#ffffff'
this.context.stroke();
this.context.fill()
```


刚才说了tabbar有阴影，很多同学不知道贝塞尔曲线可以设置阴影，下面给大家示范一下：

```
this.context.shadowOffsetY = 0
this.context.shadowColor = '#949494'
this.context.shadowBlur = 12
this.context.stroke();


```
这样就实现了沿着曲线的阴影，非常完美。

现在我们要在画布上添加切换页面的按钮，很明显要使用层叠布局，而且中间的按钮要给它特殊处理一下

```
@State tabList:TabItem[] = [
  {image:$r('app.media.tb00'),selectImage:$r('app.media.tb01'),title:'首页'},
  {image:$r('app.media.tb10'),selectImage:$r('app.media.tb11'),title:'发现'}
]
ForEach(this.tabItems,(item:TabItem,index)=>{
  if(index == this.tabItems.length/2){
    Image($r('app.media.middle'))
      .width(60)
      .height(60)
      .borderRadius(30)
      .offset({y:-30})
      .borderWidth(4)
      .borderColor(Color.White)
      .borderStyle(BorderStyle.Solid)
      .shadow({
        radius: 20,
        color: Color.Gray,
        offsetX: 0,
        offsetY: 0
      })
      .onClick(()=>{
        this.currentIndex = index
        this.tabClick(-1)
      })
  }
  Column({space:4}){
    Image(this.currentIndex == index? item.selectImage:item.image)
      .width(22)
      .height(22)
      .objectFit(ImageFit.Contain)
    Text(item.title)
      .fontSize(13)
      .fontColor( this.currentIndex == index? this.selectedFontColor:this.fontColor)
  }
  .alignItems(HorizontalAlign.Center)
  .onClick(()=>{
    this.currentIndex = index
    this.tabClick(index)
  })
  // .backgroundColor(Color.Black)
})

```

现在一个完整的tabbar样式就完成了，接下来要用它替换系统的tabbar并且实现页面的切换，虽然说是替换掉系统tabbar，但是还是要使用它。你会发现在系统的tabbar中不设置tabBar属性的话页面底部就会是一片空白：
![image](https://github.com/user-attachments/assets/bbd09f7f-7e92-4e3e-a851-2567ec50249d)

正好可以将我们的tabbar放在那，你还可以使用barHeight属性调整高度：

```
Stack({alignContent:Alignment.Bottom}){
  Tabs({ barPosition: BarPosition.End, controller: this.tabsController }) {
    TabContent() {
      Page1()
    }
    TabContent() {
      Page2()
    }
    TabContent() {
      Page3()
    }
    TabContent() {
      Page3()
    }
  }
  .backgroundColor(Color.White)
  .barHeight(64)
  .onChange((index) => {
    this.currentIndex = index
  })
  .animationDuration(1)
  .scrollable(false)
  
  BottomBar({tabItems:this.tabList,currentIndex:this.currentIndex,tabClick:(index)=>{
    if(index == -1){
      router.pushUrl({
        url:'pages/Page3'
      })
    }else {
      this.tabsController.changeIndex(index)
    }
  }})
}
.height('100%')
.width('100%')
```


这样我们就成功实现了异形的自定义tabbar，非常完美。
