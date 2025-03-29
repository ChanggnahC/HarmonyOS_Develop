# 鸿蒙Next开发实战教程：实现抖音长按快速评论特效
开篇点题，今天玩点花的。

不知道大家有没有发现，抖音上的评论键长按会弹出一排表情框用于快速评论，不过现在鸿蒙原生版的抖音还没有这个功能，今天幽蓝君就小试牛刀，在鸿蒙上做一下这个功能，也是应一位友友的私信要求。

不过幽蓝君学艺不精，水平有限，只能模仿个三分像，仅供大家参考。

话不多说，直接看成品效果图：
![image](https://github.com/user-attachments/assets/fda95771-d250-4a23-aa8b-093a3015fa8c)

实话实说，真的是有点难，昨晚搞到零点一刻，一度崩溃。关于这个功能的实现过程，还听幽蓝君细细道来。

首先你看这个弹出框，它并不存在于页面之中，因为在弹出之后整个页面是有蒙版的，除了它之外所有按键不能点击，所以它其实是一个弹框，我选择使用CustomDialogController来实现。

那么问题来了，CustomDialogController覆盖在页面上方，怎么做才能让这一排表情看起来像是从评论按钮里弹出呢？

幽蓝君的实现思路是这样的，将弹框的起始位置设置在评论按钮上，弹框的尺寸尤其是宽度设置为0，在动画的过程中，让弹框的x坐标不断左移，同时宽度增加，这样看起来就像是从评论按钮中滑出来。

相关代码如下：

```
Flex({ direction: FlexDirection.Row,justifyContent:FlexAlign.SpaceEvenly,alignItems:ItemAlign.Center }) {
})
.width(this.menuWidth)
.height(60)
.opacity(this.alpha)
.position({ x: this.menuX })
.backgroundColor('rgb(22,22,22)')
.animation({ duration: 300, curve: Curve.Linear, iterations: 1 })
.onAppear(()=>{  
  this.menuX = 0  this.menuWidth = 200
})
```
弹出动画做完了，内容部分很简单，就几个表情，下面要做的事情是长按表情放大，不仅长按会放大，长按之后滑动到表情上也会放大，所以我判断这是一个组合手势，长按手势和滑动手势的组合。

这个组合手势也有前后顺序，只有长按开始之后的滑动才会有放大效果。

所以我的实现逻辑是这样的，给单个表情添加长按放大手势，给整个弹框添加触摸手势onTouch，根据它返回的坐标确定当前的表情，为了提高精度，我对坐标进行了判断，只有滑动到表情中间才会触发放大效果。相关代码如下：

```
//表情长按
.gesture(  
  LongPressGesture({ repeat: false })    
  .onAction((event: GestureEvent) => {      
    this.bigIndex = index      
    this.longPress = true   
   })    
)


//长按之后滑动
.onTouch((event?: TouchEvent) => {  
  if(event && this.longPress){    
    if(event.touches[0].x > 0 && event.touches[0].x < 200){      
      if(event.touches[0].y > 25 && event.touches[0].y < 35){        
        let ys = event.touches[0].x%50        
        if(ys > 20 && ys < 40){          
           this.bigIndex = Math.floor(event.touches[0].x/50)        
         }     
       }    
     }  
   }
 })

```

接下来，最难的部分来了，点击表情进行评论的时候，除了弹框消失以外，它还有一个发射的效果，发射一串表情到评论按钮里，这玩意怎么实现呢。

首先想到的是路径动画motionPath，最终也确实是用它实现，不过过程没那么容易，因为我们这个效果是出场的时候不触发，退出的时候触发动画。但是motionPath这个东西并不支持像按钮点击主动触发，只能通过尺寸或者位置的变化来触发，官方管这叫设计哲学。。。

我的实现思路是这样的，在点击某个表情之后，在对应的位置创建三个相同的表情，然后依次发射出去，但是又怎么实现依次发射的效果呢。使用计时器吗，效果并不好，最后发现在animation上设置不同的动画时间可以实现想要的效果：

```
Text(this.emoList[this.toggleIndex])  
  .fontSize(this.toggleSize)  
  .motionPath({ path: this.animatePath, from: 0.0, to: 1.0, rotatable: false })  
  .position({x:this.toggleX,y:0})  
  .animation({ duration: this.duration, curve: Curve.EaseIn, iterations: 1 })
```


以上就是今天内容的整个实现过程，其实它从技术角度上并不难，难的是实现思路和方法，这就好比华为推出三折叠手机之后很多友商也要推出三折叠了，在这之前你怎么不造呢，因为华为的专利没有公布。
