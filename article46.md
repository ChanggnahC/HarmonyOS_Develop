# 鸿蒙NEXT开发实战教程—小红书app
幽蓝君最近发现小红书是个好东西，一定要多逛

今天就浅浅模仿一下小红书app，主要是底部tab栏和主页部分。
![image](https://github.com/user-attachments/assets/114e9368-cc27-4d61-9e2d-e9ff713c087f)


首先看一下tabbar，由于中间有一个红色按钮的存在，所以这里我使用自定义导航栏来实现，自定义的实现逻辑是在本来app的上层叠加一层自定义tabbar，使用监听index的变化来改变tababr的状态，具体实现代码如下：

```
Column(){  
  if(!this.tabItem.middleMode){    
      Text(this.tabItem.title)      
        .fontSize(16)      
        .fontWeight(FontWeight.Bold)      
        .fontColor(this.isSelected ? '#000000' : '#B7B7B7')  
   }else {    
      Text('+')      
        .fontSize(30)      
        .width(50)      
        .height(30)      
        .backgroundColor(Color.Red)      
        .fontColor(Color.White)      
        .textAlign(TextAlign.Center)      
        .lineHeight(30)  
    }
}.backgroundColor(Color.White).width("100%").height(56).justifyContent(FlexAlign.Center)

Flex(){  
  ForEach(this.tab,(item:YLTabClass,index:number)=>{    
    YLTabbarItem({tabItem:item,isSelected:this.currentIndex === index})      
    .onClick(()=>{        
      if(index != 2){          
        this.currentIndex = index        
      }        
      this.tabItemClick(index);      
     }) 
 })}
```
然后来到主页部分，最上面是一个导航栏，这个导航栏使用系统的Navigation就可以实现，需要注意的地方是导航栏上有带角标的按钮，这个按钮在很多地方都有出现，比如tabbar上等等，所以把它抽出来做一个单独的组件：

```
Stack({alignContent:Alignment.TopEnd}){  
  Text(this.title)    
    .fontColor(this.isSelect?Color.Black: Color.Gray)    
    .fontSize(17)    
    .fontWeight(FontWeight.Bold)  
  if(this.badge > 0){   
     Text(this.badge.toString())      
       .width(16)      
       .height(16)      
       .fontSize(12)      
       .textAlign(TextAlign.Center)      
       .backgroundColor(Color.Red)      
       .fontColor(Color.White)      
       .borderRadius(8)      
       .margin({right:-10,top:-4})  
    }}.padding(10)
```
接下来是分类频道部分，这一部分比较简单，使用一个scroll组件就能实现：

```
Scroll(){  
  Row({space:20}){    
    ForEach(this.scrollTitleList,(str:string,index)=>{      
      Text(str)        
        .fontSize(16)        
        .fontColor(Color.Gray)    
      })  
 }}.scrollable(ScrollDirection.Horizontal).scrollBar(BarState.Off).width('100%')
```
最后主要内容部分是一个瀑布流，瀑布流看起来比较难，其实它和普通网格组件唯一的不同点是每一个图片的尺寸不同，瀑布流的相关代码如下：

```
WaterFlow() {  LazyForEach(this.dataSource, (item: number) => {    FlowItem() {      Column() {        Image('/pages/img/img' + item % 5 + '.jpg')          .objectFit(ImageFit.Fill)          .width('100%')          .height(this.itemHeightArray[item % 100])        Text('和闺蜜在一起能长寿 ')          .fontColor(Color.Black)          .fontSize(15)          .margin({top:6})        Row(){          Row(){            Image($r('app.media.header'))              .width(20)              .height(20)              .backgroundColor(Color.Gray)              .borderRadius(10)            Text('这里是昵称')              .fontColor(Color.Gray)              .fontSize(13)              .margin({left:4})          }          .alignItems(VerticalAlign.Center)          Text('3233')            .fontColor(Color.Gray)            .fontSize(13)        }        .margin({top:6})        .width('100%')        .alignItems(VerticalAlign.Center)        .justifyContent(FlexAlign.SpaceBetween)      }      .alignItems(HorizontalAlign.Start)    }    .width('100%')  }, (item: string) => item)}.padding({left:5,right:5}).columnsTemplate("1fr 1fr").columnsGap(8).rowsGap(5).backgroundColor(Color.White).width('100%').height('100%')

```
