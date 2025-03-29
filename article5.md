# 鸿蒙Next开发基础动画教程
在移动互联网时代，App的使用体验非常重要，比如布局的变化、页面的切换、弹窗的显示和隐藏都要是平顺的，丝滑的，这就需要用到动画。鸿蒙提供了很多种动画的方式，今天为大家一一分享。

布局更新动画

尺寸、位置等的变化都属于布局更新，鸿蒙提供了属性动画和显示动画两种方式。下面通过一个案例进行说明：
![image](https://github.com/user-attachments/assets/d43d29f4-09ce-4985-a435-043db65c4844)

界面中有一个粉色的Text组件，宽高都是100，需求是点击button让Text组件的宽变成200,在没有动画的情况下，代码是这样的：

```
Button('click')
  .width(60)
  .height(60)
  .margin({top:50})
  .onClick(()=>{
      this.textWidth = 200
   })
```
执行效果很生硬：
![image](https://github.com/user-attachments/assets/b752201b-90cf-43fc-ba8a-8190226a80c1)

然后我们使用显示动画的方式给它加一个动画效果：

```
animateTo({duration:1000,curve:Curve.EaseIn},()=>{
    this.textWidth = 200
})
```
其中设置了动画执行时间1000毫秒，曲线为EaseIn，效果如下：
![image](https://github.com/user-attachments/assets/f47c0dc4-6f89-4b59-91d0-52a0c1ec14c7)


这样就很丝滑。

还可以使用属性动画来达到同样的效果，所谓属性动画，就是把animation添加到Text的属性当中：

```
Text('text')
   .height(100)
   .width(this.textWidth)
   .animation({duration:1000,curve:Curve.EaseIn})
   
   .backgroundColor(Color.Pink)
   .fontSize(20)
   .textAlign(TextAlign.Center)
   
   Button('click')
          .width(60)
          .height(60)
          .margin({top:50})
          .onClick(()=>{
            this.textWidth = 200
          })
```
要注意的是，animation只对它上面的属性有效，比如我们修改width一定要把width放到animation前面，不然是无效的，比如下面的代码

```
//无效
Text('text')
   .animation({duration:1000,curve:Curve.EaseIn})
   .height(100)
   .width(this.textWidth)
```


组件插入和删除

组件的插入和删除动画通常使用transition，transition可以指定组件的入场和出场方式、位置、尺寸、角度、透明度等等

```
Image($r('app.media.mountain')).width(200).height(200)
    //入场
    .transition({ type: TransitionType.Insert, translate: { x: 200, y: -200 } })
    //出场
    .transition({ type: TransitionType.Delete, opacity: 0, scale: { x: 0, y: 0 } })
```
上面代码指定了入场时组件由正常位置为原点，x为200，y为-200的地方平移入场。出场时尺寸变为0，透明度变为0。下面我把它放到完整代码中执行一下：

```
if(this.flag){
          Image($r('app.media.image')).width(200).height(200)
            .transition({ type: TransitionType.Insert, translate: { x: 200, y: -200 } })
            .transition({ type: TransitionType.Delete, opacity: 0, scale: { x: 0, y: 0 } })
        }
        Button('click')
          .width(60)
          .height(60)
          .margin({top:50})
          .onClick(()=>{
            this.flag = true
          })

```
![image](https://github.com/user-attachments/assets/37a741b0-a83b-4b38-a5ba-dd3a096df4fb)

发现并没有动画效果，为啥呢？

因为虽然指定了动画的出场和入场方式，但是并没有指定动画的完成时间，所以再给flag加一个显示动画

```
animateTo({ duration: 1000 }, () => {
      this.flag = !this.flag;
 })
```
看下效果：
![image](https://github.com/user-attachments/assets/8c907b9f-2bf7-4eb9-9ce5-56a4e1ae532e)


完美。

今天分享就到这里，感谢阅读。
