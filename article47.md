# HarmonyOS NEXT开发实战教程—搜索页
今天忙里偷闲，分享一个搜索页实现过程，先上效果图：
![image](https://github.com/user-attachments/assets/1bf9e1fa-1dc2-4540-b7b4-de2e730ab336)

界面部分比较简单，大体分为导航栏、历史搜索、猜你想搜和热搜榜几个部分，历史搜索采用用户首选项进行存储数据。

导航栏部分相关代码如下：

```
Flex({direction:FlexDirection.Row,wrap:FlexWrap.NoWrap,alignItems:ItemAlign.Center}){  
    Image($r('app.media.back'))    
    .width(30)    
    .height(30)    
    .objectFit(ImageFit.Contain)  
    Flex({direction:FlexDirection.Row,wrap:FlexWrap.NoWrap,alignItems:ItemAlign.Center}){      
    TextInput({placeholder:'长款羽绒服',text:this.searchText})        
        .width('100%')        
        .placeholderColor(Color.Gray)        
        .fontSize(14)        
        .backgroundColor('rgb(226,226,226)')                            
        .enterKeyType(EnterKeyType.Search)        
        .onChange((str)=>{          
            this.searchText = str        
        })        
        .onSubmit(()=>{          
            this.saveHistory(this.searchText)          
            this.searchText = ''        
        })    
    Image($r('app.media.scan'))      
    .width(22)      
    .height(22)      
    .margin({left:5})      
    .constraintSize({minWidth:22})      
    .objectFit(ImageFit.Contain)    
    
    Text('搜索')      
    .width(60)      
    .height(28)      
    .backgroundColor(Color.Orange)      
    .borderRadius(4)      
    .fontColor(Color.White)      
    .textAlign(TextAlign.Center)      
    .fontSize(12)      
    .margin({left:5})  
}  
.padding({left:5,right:5})  
.width('100%')  
.height(34)  
.backgroundColor('rgb(226,226,226)')  
.borderRadius(8)          
.margin({left:5})}
.padding({left:10,right:10})
.width('100%')
.backgroundColor(Color.White).height(45)
```
历史搜索部分采用Flex布局实现：

```
Flex({direction:FlexDirection.Row,wrap:FlexWrap.Wrap}){  
    ForEach(this.hisList,(str:string,index)=>{    
        Text(str)      
            .fontColor('#4a4a4a')      
            .fontSize(12)      
            .padding({left:6,right:6,top:4,bottom:4})      
            .borderWidth(1)      
            .borderColor('rgb(216,216,216)')      
            .borderStyle(BorderStyle.Solid)      
            .margin({left:5,top:5})      
            .textAlign(TextAlign.Center)      
            .borderRadius(4)  
})}
```
猜你想搜部分，同样使用Flex布局：

```
Flex({direction:FlexDirection.Row,wrap:FlexWrap.Wrap}){  
    ForEach(this.guessList,(str:string,index)=>{    
        Text(str)      
         .fontColor('#4a4a4a')      
        .fontSize(15)      
        .margin({top:6})      
        .width('50%')  
})}
```
热搜榜部分使用一个Scroll和一个Swiper实现联动效果：

```
Column(){  
    Scroll(this.scrollController){    
        Row({space:30}){      
            ForEach(this.hotTitleList,(str:string,index)=>{        
                Text(str)          
                .fontSize(15)          
                .fontColor(this.hotIndex == index?Color.Orange:Color.Gray)                          
                .fontWeight(this.hotIndex == index?FontWeight.Bold:FontWeight.Normal)          
                .onClick(()=>{            
                    this.hotIndex = index                    
                    this.swiperController.changeIndex(this.hotIndex,true)                  
                })      
    })    
    }    
    .height(45)    
    .alignItems(VerticalAlign.Center)  }  
    .scrollable(ScrollDirection.Horizontal)  
    .scrollBar(BarState.Off)  .width('100%')  

    Swiper(this.swiperController){    
        ForEach(this.hotTitleList,(str:string,index)=>{      
            Column(){        
                ForEach(this.hotContentList,(str:string,index)=>{          
                    Row(){            
                        Row(){              
                            Text((index+1).toString())                
                                .fontSize(15)                
                                .fontColor('#4a4a4a')                            
                                 .fontWeight(FontWeight.Bold)                       
                                 .width(40)                
                                 .textAlign(TextAlign.Center)                  
                              Text(str)                
                                   .fontSize(15)                        
                                  .fontColor('#4a4a4a')            }            
                              Text('热度100万')              
                                .fontSize(12)              
                                .fontColor(Color.Gray)          
        }          
        .width('100%')          
        .height(40)          
        .justifyContent(FlexAlign.SpaceBetween)                          
        .alignItems(VerticalAlign.Center)          
        .borderRadius(10)          
        .margin({top:8})          
        .linearGradient({            angle: 90,            colors: [[index<3?0xfff8f3:0xf9f9f9, 0.0], [index<4?0xfffbfa:0xfcfcfc, 0.4], [0xffffff, 1.0]]          })        })      
}      
.width('100%')      
.alignItems(HorizontalAlign.Start)   
 })  } 
.vertical(false)  
.indicator(false)  
.loop(false)  
.onAnimationEnd((index: number, extraInfo: SwiperAnimationEvent) => {    this.hotIndex = index    
console.info("index: " + index)    
console.info("current offset: " + extraInfo.currentOffset)    
const xOffset: number = this.scrollController.currentOffset().xOffset;    this.scrollController.scrollTo({ xOffset: index*70, yOffset: 0 })  
})}
```
