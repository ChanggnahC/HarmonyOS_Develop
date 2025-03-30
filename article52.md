# HarmonyOS NEXT实战教程：菜谱App
随着鸿蒙系统5.0的发布，兼容的机型越来越多，对于开发者来说机会也越来越多，大家不要气馁，学习鸿蒙肯定会有用武之地，我们要做的就是做好准备。

今天跟大家分享实战教程是一个菜谱App。
![image](https://github.com/user-attachments/assets/9b632d22-3324-4619-899a-2b3c075c8aa3)

首页这个页面可能会让初学者望而生畏，看起来比较复杂。但是仔细分析一下并不太难。幽蓝君再啰嗦一句，大家看到一个项目或者一个页面，不要拿过来就写，如果你不是特别熟悉，就先分析一下它大体是一个什么样的架构，分析明白再着手写代码。
![image](https://github.com/user-attachments/assets/2bbed6d7-50fb-402b-a716-5d175144cc5c)


这个页面首先是一个导航栏加一个列表组件List，列表中又包含了许多的样式。那我们就先把导航栏写出来：

```
@Builder NavigationMenu(){
    Flex({direction:FlexDirection.Row,wrap:FlexWrap.NoWrap,alignItems:ItemAlign.Center}){
      Search({placeholder:'想吃点什么？'})
        .placeholderColor('#999999')
        .fontColor("#4a4a4a")
        .width('100%')
        .height(35)

      Stack({alignContent:Alignment.TopEnd}){
        Image($r('app.media.msg'))
          .width(24)
          .height(24)
          .objectFit(ImageFit.Auto)
          .margin({left:10})

        Text('3')
          .width(14)
          .height(14)
          .backgroundColor('#FF4F4F')
          .fontColor(Color.White)
          .textAlign(TextAlign.Center)
          .fontSize(8)
          .borderRadius(7)
          .borderColor(Color.White)
          .borderWidth(2)
          .borderStyle(BorderStyle.Solid)
          .margin({left:2,top:-2})
      }

    }
    .width('100%')
    .height(40)
    .padding({left:12,right:12})
  }

```

接下来分析下内容区域，由上而下，大家可以发现主要就两种样式：轮播图和网格列表，轮播图比较简单，以最上面的轮播图为例，分享一下相关代码：

![image](https://github.com/user-attachments/assets/adadb8ba-5d5e-4b17-94cb-3df106eac8fa)

```
Swiper(){
            Stack({alignContent:Alignment.Bottom}){
              Image($r('app.media.banner_1'))
                .width('100%')
                .height(260)
                .borderRadius(8)

              Column({space:5}){
                Text('麻辣鲜香·剁椒豆腐鱼')
                  .fontColor(Color.White)
                  .fontSize(18)
                Row(){
                  Row(){
                    Image($r('app.media.banner_icon'))
                      .width(16)
                      .height(16)
                      .borderRadius(8)
                    Text('爱做饭的老王')
                      .fontColor(Color.White)
                      .fontSize(12)
                      .margin({left:3})
                  }

                  Image($r('app.media.banner_like'))
                    .width(20)
                    .height(17)
                }
                .width('100%')
                .justifyContent(FlexAlign.SpaceBetween)
              }
              .alignItems(HorizontalAlign.Start)
              .padding({left:12,right:12})
              .margin({bottom:10})
            }
            .width('100%')
            .height(260)
            .onClick(()=>{
              router.pushUrl({
                url:'pages/menustep'
              })
            })
          }
```


热门活动的轮播图更加简单，不再赘述了，这样我们就只剩下网格类的样式了。

网格组件Grid的用法和List类似，不过它可以使用rowsTemplate和columnsTemplate来控制元素大小来滚动方向，这一点大家可以多多实践。以分类部分为例，向大家介绍下Grid的用法：


```
Grid(){
            ForEach(this.functionList,(item:FunctionItem,index)=>{
              GridItem(){
                Column(){
                  Image(item.cover)
                    .height(70)
                    .width(60)
                  Text(item.title)
                    .fontColor('rgb(51,51,51)')
                    .fontSize(12)
                }
                .height(150)
                .alignItems(HorizontalAlign.Center)
              }
            })
          }
          .rowsTemplate('1fr 1fr')
          .columnsTemplate('1fr 1fr 1fr 1fr 1fr')
          .height(180)
          .width('100%')
```

首页的部分就是这样，由小见大，其实所有页面的开发也就是这些东西，所以其他页面这里也不再赘述
