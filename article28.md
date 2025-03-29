# HarmonyOS NEXT开发实战教程--招聘app
这一周忙到起飞，只能在周末发个文章。今天的内容比较简单，是一个招聘app，适合新手友友参考，大佬们可以直接忽略。

看一下效果图：
![image](https://github.com/user-attachments/assets/f5ca3e53-cddf-4cff-80d9-7dcbb27d1378)


这是一个比较常见的应用，大家做这类应用建议大家先分析一下应用和页面的结构，避免写完发现错了又改。

这个应用首先有4个tabbaritem，是很常见的样式，使用系统的Tabs就可以实现：

```
Tabs({barPosition:BarPosition.End}){  
  TabContent(){    
    Home()  
  }  
  .tabIndex(0)  
  .tabBar(this.Tabbar($r('app.media.tab01'),'职位'))  
  TabContent(){    
    Has()  
  }  
  .tabIndex(0)  
  .tabBar(this.Tabbar($r('app.media.tab01'),'职位'))  
  TabContent(){    
    Msg()  
  }  
  .tabIndex(0)  
  .tabBar(this.Tabbar($r('app.media.tab01'),'职位'))  
  TabContent(){    
    Mine()  
  }  
  .tabIndex(0)  
  .tabBar(this.Tabbar($r('app.media.tab01'),'职位'))
}.scrollable(false).expandSafeArea([SafeAreaType.SYSTEM], [SafeAreaEdge.TOP, SafeAreaEdge.BOTTOM])
```


关于导航栏部分，可以看到它是一个渐变的颜色，而且延伸到了屏幕顶端，这种样式系统的导航栏不太容易实现，需要我们自定义导航栏，这个导航栏在上一篇文章中有过介绍，大家可以直接下载使用。

到了列表部分，这个列表数据很多，看起来很复杂，但是只要稍加分析就可以很容易的实现，因为都是很基础的布局方式：
![image](https://github.com/user-attachments/assets/6eb5109a-0ecc-4e7f-9049-7369c6667726)


列表的相关代码如下：

```
List({space:10}){
  ForEach(this.jobList,(item:JobClass,index)=>{
    ListItem(){
      Column(){
        Row(){
          Text(item.title)
            .fontSize(20)
            .fontColor('#151515')
          Text(item.money)
            .fontSize(19)
            .fontColor('#00B7C4')
        }
        .width('100%')
        .alignItems(VerticalAlign.Center)
        .justifyContent(FlexAlign.SpaceBetween)
        Text('李蛋个人设计团队  未融资  0-20人')
          .fontColor('#5E5E5E')
          .fontSize(12)
          .margin({top:12})
        Row({space:5}){
          Text('1-3年')
            .backgroundColor('#F6F6F6')
            .padding({left:6,right:6,top:3,bottom:3})
            .fontSize(10)
            .borderRadius(4)
            .fontColor('#626262')
          Text('学历不限')
            .backgroundColor('#F6F6F6')
            .padding({left:6,right:6,top:3,bottom:3})
            .fontSize(10)
            .borderRadius(4)
            .fontColor('#626262')
          Text('设计软件') 
           .backgroundColor('#F6F6F6')
            .padding({left:6,right:6,top:3,bottom:3})
            .fontSize(10)
            .borderRadius(4)
            .fontColor('#626262') 
         Text('经验不限')
            .backgroundColor('#F6F6F6')
            .padding({left:6,right:6,top:3,bottom:3})
            .fontSize(10)
            .borderRadius(4)
            .fontColor('#626262')
        } 
       .margin({top:8})
        Row(){
          Row(){
            Image($r('app.media.headerImg'))
              .width(22)
              .height(22)
            Column({space:4}){
              Text('李蛋·品牌经理')
                .fontSize(11)
                .fontColor('#151515')
              Text('今日回复5次') 
               .fontSize(11) 
               .fontColor('#848484')
            } 
           .alignItems(HorizontalAlign.Start)
            .margin({left:7})
          }
          Text('丛台区  政府大厅') 
           .fontSize(10)
            .fontColor('#848484')
        } 
       .margin({top:14})
        .width('100%')
        .justifyContent(FlexAlign.SpaceBetween) 
     } 
     .width('100%') 
     .height(149)
      .backgroundColor(Color.White)
      .borderRadius(4) 
     .padding({left:12,right:12,top:14,bottom:14})
      .alignItems(HorizontalAlign.Start)
    }
  })}
.padding({top:10,bottom:10})
```


这代码不会自动换行了，难受。

其实这应用还有两个页面，不过内容大同小异，不再赘述了：






祝大家周末愉快。
