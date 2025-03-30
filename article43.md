# 鸿蒙Next开发实战教程--银行App
昨天Mate70的官方预热直接引起网络爆炸，现在预约人数已经两百多万了，大家都这么有米吗

今天跟大家分享一个银行app实战教程。
![image](https://github.com/user-attachments/assets/60fbcf3e-cefa-41b8-8ca3-712bc0d7a02a)

页面虽然看起来比较复杂，但是仔细分析一下并不难，下面跟大家分享一下本项目的一些难点。

首先是首页的导航栏，这个导航栏看起来没什么特别，不过它是一个可以变化的导航栏，在页面滑动的过程中，导航栏的背景色会从透明变为白色。

系统的导航栏无法满足需求，所以我们需要自定义一个导航栏。

这个导航栏的内容部分比较简单，中间标题加两侧按钮，布局为SpaceBetween，相关代码如下：

```
Row(){  
  Image($r('app.media.scan'))    
    .width(22)    
    .height(22)  
  Text('我的资产')    
    .width(120)    
    .textAlign(TextAlign.Center)    
    .fontColor('#1a1a1a')    
    .fontSize(16)    
    .fontWeight(FontWeight.Bold)  
  Image($r('app.media.ss'))    
    .width(22)    
    .height(22)    
    .onClick(()=>{      
      router.pushUrl({        
        url:'pages/Found'      
        })    
     }
  )}
  .width('100%')
  .justifyContent(FlexAlign.SpaceBetween)
  .padding({left:12,right:12,top:px2vp(this.topRectHeight) + 12,bottom:12})
  .backgroundColor('rgba(255,255,255,'+this.titleAlpha+')')
```

接下来我们要处理它的颜色变化。我们想要页面向上滑动到第二个单元的时候导航栏变为白色背景，在页面向下滑动到头的时候导航栏变为透明背景，这里可以使用list组件的onScrollIndex方法来实现，相关代码如下：

```
.onScrollIndex((start,end)=>{  
  if(start == 0 && end == 4){    
    if(this.havaStarted){      
      this.titleAlpha = '1'   
     } 
   }  
  if(start == 0 && end == 3){    
    if(this.havaStarted){      
        this.titleAlpha = '0'    
      }  
    }
})
```

首页里除导航栏以外的部分都比较简单，主要就是一个List组件，把每个部分拆分出来以后就是简单的横向或者纵向布局，以头部总资产部分为例，就是一个比较简单的横向布局，相关代码如下：

Stack({alignContent:Alignment.Center}){  Image($r('app.media.nav_img'))    .width('100%')  Row(){    Column({space:6}){      Row(){        Text('总资产')          .fontSize(13)          .fontColor(Color.Black)        Image($r('app.media.eyes'))          .width(14)          .height(10)          .margin({left:5})          .onClick(()=>{            this.showMoney = !this.showMoney          })      }      Text(this.showMoney? '2000':'****')        .fontSize(30)        .fontColor(Color.Black)        .fontWeight(FontWeight.Bold)      Text('今日收益 +13.3')        .fontSize(12)        .fontColor(Color.Gray)    }    .alignItems(HorizontalAlign.Start)    .margin({left:26})    Row(){      Image($r('app.media.shield'))        .width(16)        .height(16)      Text('开启保障')        .fontColor(Color.White)        .fontSize(13)        .margin({left:6})    }    .width(102)    .height(30)    .justifyContent(FlexAlign.Center)    .backgroundColor('rgba(0,0,0,0.1)')    .borderRadius({topLeft:15,bottomLeft:15})  }  .width('100%')  .justifyContent(FlexAlign.SpaceBetween)}
还有一个部分要注意，像热门推荐、稳健理财等部分，他们的标题可以使用List中Group的header来实现，这样实现的代码就比较简洁：

```
@Builder ListGroupHeader(title:string,subTitle:string){  
    Row({space:5}){    
      Text(title)      
        .fontColor(Color.Black)      
        .fontSize(18)      
        .fontWeight(FontWeight.Bold)    
      Text(subTitle)      
        .fontColor(13)      
        .fontColor('#999999')  
      }  
      .height(60)  
      .width('100%')  
      .padding({left:12,bottom:13})  
      .alignItems(VerticalAlign.Bottom)
  }
```


以上就是本项目的一些难点。
