# 如何在鸿蒙Next开发中实现一个带箭头的菜单
之前在微信项目中有过这么一个组件，但是项目太大没有办法详细讲解，今天就专门写一篇文章做一个带箭头的菜单。
![image](https://github.com/user-attachments/assets/38d2d70f-1b72-4083-8274-99cca67bd85c)

这个菜单可以分为两个部分，三角形和列表，我们先从简单的开始，先做列表。

```
List(){
            ListItem(){
              MenuRow({imagePath:'/images/menu_group.png',titleString:'发起群聊'})
            }
            ListItem(){
              MenuRow({imagePath:'/images/menu_add.png',titleString:'添加朋友'})
            }
            ListItem(){
              MenuRow({imagePath:'/images/menu_scan.png',titleString:'扫一扫'})
            }
            ListItem(){
              MenuRow({imagePath:'/images/menu_purchase.png',titleString:'收付款'})
            }
          }
          .edgeEffect(EdgeEffect.None)
          .borderRadius(5)
          .padding({left:12})
          .divider({ strokeWidth: 1, color: 'rgb(130,130,130)', startMargin: 20, endMargin: 0 }) // 每行之间的分界线
          .backgroundColor('rgb(76,76,76)')
          .width(130)
          .height(180)


@Component
struct MenuRow {
 @Prop imagePath:string
 @Prop titleString:string
  build(){
    Flex({direction:FlexDirection.Row,alignItems:ItemAlign.Center}){
      Image(this.imagePath)
        .width(21)
        .height(21)

      Text(this.titleString)
        .fontColor(Color.White)
        .fontSize(18)
        .margin({left:8})
    }
    .height(44)
  }
}
```
列表完成了：

![image](https://github.com/user-attachments/assets/f2767652-bffd-457d-a004-b5587e3f8190)

接下来用Path画一个三角形：

```
Path()
  .width(20)
  .height(20)
  .commands('M30 0 L60 60 L0 60 Z')
  .fill('rgb(76,76,76)')
  .stroke('rgb(76,76,76)')
```


这段代码中比较难理解是commands参数：

```
.commands('M30 0 L60 60 L0 60 Z')

```

意思是从坐标（30，0）开始，绘制一条到（60，60）的直线，然后从（60，60）绘制一条到（0，60）的直线，再绘制一条从（0，60）到（30，0）的直线，形成一个封闭的三角形。看下效果：
![image](https://github.com/user-attachments/assets/74fc1e1b-a829-466e-8329-54e9263ed6ee)


三角形已经绘制出来了，给它调整一下位置：

.margin({left:90})
![image](https://github.com/user-attachments/assets/bcc4b371-221d-4f44-a9fd-468fc0dcd556)

完成。

完整的代码如下：

```
@Entry
@Component
struct TestPage {
  @State menuOpacity:number = 1

  build() {
    Row() {
      Column() {
        
        Flex({direction:FlexDirection.Column,justifyContent:FlexAlign.Center}){
          Path()
            .width(20)
            .height(20)
            .commands('M30 0 L60 60 L0 60 Z')
            .fill('rgb(76,76,76)')
            .stroke('rgb(76,76,76)')
            .margin({left:90})

          List(){
            ListItem(){
              MenuRow({imagePath:'/images/menu_group.png',titleString:'发起群聊'})
            }
            ListItem(){
              MenuRow({imagePath:'/images/menu_add.png',titleString:'添加朋友'})
            }
            ListItem(){
              MenuRow({imagePath:'/images/menu_scan.png',titleString:'扫一扫'})
            }
            ListItem(){
              MenuRow({imagePath:'/images/menu_purchase.png',titleString:'收付款'})
            }
          }
          .edgeEffect(EdgeEffect.None)
          .borderRadius(5)
          .padding({left:12})
          .divider({ strokeWidth: 1, color: 'rgb(130,130,130)', startMargin: 20, endMargin: 0 }) // 每行之间的分界线
          .backgroundColor('rgb(76,76,76)')
          .width(130)
          .height(180)

        }
        .width(130)
        .height(200)
      }
      .width('100%')
    }
    .height('100%')
  }
}

@Component
struct MenuRow {
  imagePath:string
  titleString:string
  build(){
    Flex({direction:FlexDirection.Row,alignItems:ItemAlign.Center}){
      Image(this.imagePath)
        .width(21)
        .height(21)

      Text(this.titleString)
        .fontColor(Color.White)
        .fontSize(18)
        .margin({left:8})
    }
    .height(44)
  }
}
```
