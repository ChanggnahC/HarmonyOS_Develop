# 鸿蒙Next开发实战案例--简易画板
友友们，一别几天，甚是想念。今天继续和大家分享鸿蒙开发实战案例。

今天要做的是一个简易画板，先看效果图。
![image](https://github.com/user-attachments/assets/79e4dbbf-ed85-46ff-b59b-ad9d1f1e71d5)


首先添加一个画板组件，并添加一个touch方法：

```
Canvas(this.context)

          .width('100%')

          .height('100%')

          .backgroundColor('#F5DC62')

          .onTouch((event)=>{

          

          })
```
然后创建一个CanvasRenderingContext2D对象和path2d对象：

```
private settings: RenderingContextSettings = new RenderingContextSettings(true);

private context: CanvasRenderingContext2D = new CanvasRenderingContext2D(this.settings);

private path2Db: Path2D = new Path2D()
```
接下来定义两个坐标用来记录手指滑过的痕迹：

```
private pointX1:number

 private pointY1:number



 private pointX2:number

 private pointY2:number
```
然后我们要做的是记录下光标或手指移动的轨迹并进行绘制。在手指刚按下屏幕时，给path2d对象一个起点：

```
if (event.type === TouchType.Down) {



              this.path2Db.moveTo(event.touches[0].x, event.touches[0].y);



              this.pointX1 = pointX

              this.pointY1 = pointY



              this.pointX2 = pointX

              this.pointY2 = pointY

            }
```
当手指移动时，使用贝塞尔曲线在两点之间绘制出圆滑的曲线：

```
if (event.type === TouchType.Move) {

              let curX = (pointX + this.pointX1)/2

              let curY = (pointY + this.pointY1)/2

              this.path2Db.bezierCurveTo(this.pointX1,this.pointY1,this.pointX2,this.pointY2,curX,curY)

              this.context.stroke(this.path2Db)

            }
```
看下效果：
![image](https://github.com/user-attachments/assets/c6e8dd77-c756-4043-bf97-913ac051e95e)

你还可以设置画笔的粗细和颜色：

//画笔粗细

this.context.lineWidth = 5

//画笔颜色

this.context.strokeStyle = '#0000ff'

![image](https://github.com/user-attachments/assets/49667b7a-fc14-4ad0-b742-7a3a71606b55)


