# HarmonyOS NEXT实战：高仿墨迹天气开发手记
老余说3月份的神秘产品是为纯血鸿蒙而生的一款全新形态的手机，别人想象不到的手机产品，这次的保密工作真是非常到位，让人十分期待。
闲言少叙，今天为大家分享新年的第一个实战项目，高仿墨迹天气
这个项目中有一些复杂的动效和曲线，对于新手友友来说可能会有一点难，不过没关系，下面幽蓝君把项目中的难点一一详解：
▍ 下拉动画
下拉动画主要在于页面的上半部分，看似简单其实暗藏玄机，导航栏和整个页面融为一体，下拉刷新时，图片前景部分跟随页面向下移动，而图片的背景部分则像是一种拉伸的效果。向上滑动时，整个图片又保持不动，和页面脱离开来。
![image](https://github.com/user-attachments/assets/4c20cbc9-f00c-40c4-b2d1-a81db38d608a)

所以导航栏部分直接自定义一个透明的，这里相对简单。
到了图片部分，我的实现方案是将背景颜色和前景图片分开来添加，先添加一个渐变色的背景：

![image](https://github.com/user-attachments/assets/bfc83a94-c58f-416c-9618-08c12471cf06)

它的代码是这样的：

```
Row(){
}
.width('100%')
.height(450 + this.refreshOffset)
.linearGradient({  
   direction: GradientDirection.Bottom, // 渐变方向  
   repeating: false, // 渐变颜色是否重复  
   colors: [[0x9ab7dd, 0] ,[0xb2c5cd, 0.4] ,[0xcbd2bc,1]] // 数组末尾元素占比小于1时满足重复着色效果
 })
```
在下拉刷新时背景有一个跟随拉伸的效果，所以他的高度里设置了一个动态变量refreshOffset，在下拉刷新组件Refresh的回调方法onOffsetChange中获取偏移量赋值就行了。

注意，上面这个背景颜色部分只是一个拉伸效果，它并不跟随下拉刷新向下移动，所以这个页面中它是唯一不在Refresh组件中的组件。

我想说的是，现在开始我们要添加一个Refresh组件，将其他元素都写在其中，包括刚刚说到的前景图片。

```
Refresh({ refreshing: $$this.isRefreshing }){       
  Row(){      
    Image($r('app.media.weather_back'))        
      .width('100%')        
      .height(210)        
      .objectFit(ImageFit.Fill)    
   }    
   .alignItems(VerticalAlign.Bottom)    
   .width('100%')    
   .height(450)
   
   。。。
}

```

这样的话基本实现了下拉刷新时背景拉伸，前景向下移动的效果。

接下来，在向上滑动页面时这个图片整体又会固定在页面上方，不跟随内容移动。内容部分很显然是一个List组件，所以图片部分不在List组件中，并且和List是层叠的布局。

所以在Refresh组件中添加Stack组件，将前景图片和List放在其中，这个复杂的动画基本就完成了。给大家放个代码结构图：
![image](https://github.com/user-attachments/assets/fd56b32f-c94f-47da-8cd5-50bb5cfe4b33)


▍ 24小时温度曲线

我相信有相当一部分友友想到要用原生实现这样的曲线和动效的瞬间是崩溃的，因为我们习惯了使用Echarts实现各种图表。
![image](https://github.com/user-attachments/assets/0419d093-0582-4660-8a5b-c3b43d76dd5d)


没关系，万丈高楼平地起，我们一点一点来。

首先添加一个画布组件：

```
Canvas(this.context)  
  .width('100%')  
  .height(100)  
  .backgroundColor(Color.Green)  
  .onReady(() => {  
  
  })

```

接下来我们要在画布的onReady方法下作画了。先随便画一个坐标点，以坐标（10,10）为例：

```
this.context.lineWidth = 4
this.context.arc(10, 10, 2, 0, 360)
this.context.stroke()
```

接下来上点难度，画一条贝塞尔曲线，我们这里使用三次贝塞尔曲线路径，也就是通过三个坐标点：

```
this.context.moveTo(10, 10)
this.context.bezierCurveTo(20, 100, 200, 100, 200, 20)
this.context.stroke()
```

现在是不是有一点思路了？我们如果把24个小时温度转换成坐标点连接起来是不是就能实现这条曲线？

现在要想办法把24个小时的温度均匀的分布在画布上，x轴直接24等分，y轴要计算出最大温差，然后再等分就可以了。转换坐标点的具体代码如下：

```
let points:PointClass[] = []
for(let i = 0;i < temps.length ;i++){
  let y_pointValue = temps[i]
  let y_point = 100 - (y_pointValue - lowestTemp) *tempUnit
  let x_point = i*xUnit + xUnit/2
  let point:PointClass = {x:x_point,y:y_point}
  points.push(point)
}
```


不出意外我们将得到24个均匀分布的坐标，把它们连接起来是不是就能实现项目中的曲线？还不行，为了得到一条平滑的曲线，我们需要更多更多的坐标，所以把它们进行一些处理：

```
const cp: Point[] = [];
const bezierPoints: Point[] = [];
bezierPoints.push({x:points[0].x,y:points[0].y});
for (let i = 1; (i + 2) < points.length; i++) {
  const bezierItem = points[i];
  cp[0] = {x:bezierItem.x, y:bezierItem.y};
  cp[1] = {x:bezierItem.x + (points[i + 1].x - points[i - 1].x) / 6, y:bezierItem.y + ( points[i + 1].y -  points[i - 1].y) / 6};
  cp[2] = {x:points[i + 1].x + (points[i].x -  points[i + 2].x) / 6, y:points[i + 1].y + ( points[i].y -  points[i + 2].y) / 6};
  cp[3] = {x:points[i + 1].x, y:points[i + 1].y};
  bezierPoints.push(cp[1], cp[2], cp[3]);
}
```


使用三次贝塞尔曲线将这些坐标连接：

```
//填充颜色
this.timerContext.fillStyle = BasicTool.LINE_COLOR
//线条颜色
this.timerContext.strokeStyle = BasicTool.LINE_COLOR
//线条宽度
this.timerContext.lineWidth = 2
this.timerContext.moveTo(time_total[0].x,time_total[0].y);
for(let i = 1;i < time_total.length - 2;i+=3){
  this.timerContext.bezierCurveTo(
    time_total[i].x,time_total[i].y,
    time_total[i+1].x,time_total[i+1].y,
    time_total[i+2].x,time_total[i+2].y,
  );
}
this.timerContext.stroke();
```


一条平滑的24小时温度曲线就完成了。

![image](https://github.com/user-attachments/assets/36048a54-1eaf-4a9e-8b92-3ac9a4f83ca0)

接下来，曲线上还有一个浮标，跟随曲线滑动。这个不难，我们只要让浮标圆点的位置在曲线坐标上就行了嘛，坐标我们已经有了，需要做的事情就是在滑动页面的时候移动浮标就可以了。

scroll组件滑动的时候会返回一个偏移量，我们根据偏移量计算相应的贝塞尔曲线坐标，然后修改浮标位置：

```
let time_unit2 = BasicTool.SCREEN_WIDTH*2/this.timeLinePoints.length
let currentPosition2 = this.timeScroller.currentOffset().xOffset/time_unit2
this.currentScrollIndex =  Math.ceil(currentPosition2*(BasicTool.SCREEN_WIDTH*2/425))
if(this.currentScrollIndex >= this.timeLinePoints.length){
  this.currentScrollIndex = this.timeLinePoints.length - 1
}
```


现在这个复杂的曲线动效就基本实现了。

▍ 15天预报曲线

![image](https://github.com/user-attachments/assets/69abe8cd-d68a-4b1c-b286-fe351a3731b0)

15天天气预报部分比较难的也是曲线部分，但是曲线我们已经会画了，不一样的就是这里多了圆点，我们在曲线上再添加一些点就行了，简单：

```
for (let i = 0;i < hightPoints.length; i++) {
  if(!(i ==0 || i == hightPoints.length -1)){
    this.context.beginPath() // 开始绘制路径
    this.context.lineWidth = 4 // 线条宽度
    this.context.strokeStyle = BasicTool.LINE_COLOR // 描边颜色
    this.context.fillStyle = BasicTool.LINE_COLOR // 描边颜色
    this.context.font = `12px sans-serif`
    this.context.arc(hightPoints[i].x, hightPoints[i].y, 2, 0, 360)
    this.context.stroke() 
  }
}
```


以上就是本项目的难点解析，欢迎您关注本站，获取更多鸿蒙实战技术分享。
