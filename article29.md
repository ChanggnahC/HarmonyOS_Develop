# 在鸿蒙NEXT中实现完全自定义导航栏
在日常app开发中，导航栏扮演着重要的角色。鸿蒙提供了系统导航栏Navigation,它支持很多属性的修改，但是应用需求更加灵活多变，比如有的导航栏有背景图片，有的导航栏要求渐变色，有的导航栏需要随时隐藏和显示等等。

遇到这些需求系统的Navigation就无法实现，这时候我们就需要自定义导航栏。今天分享一下幽蓝君自己封装的导航栏，并介绍一下它的实现过程。
![image](https://github.com/user-attachments/assets/f85e946a-720c-41e8-b91d-5243155dabb3)


作为一个导航栏，首先需要标题和返回按钮：

```
Row() {
  Row(){  
      Image($r('app.media.back'))      
        .width(20)      
        .height(20)      
        .objectFit(ImageFit.Contain)  
      Text(this.title)      
         .fontColor(Color.Black)      
         .fontSize(23)      
         .fontWeight(FontWeight.Bold)      
         .margin({left:5})      
         .maxLines(1)    
   }
}
.alignItems(VerticalAlign.Center)
.backgroundImageSize(ImageSize.Cover)
.width('100%').constraintSize({maxWidth:'100%'})
.linearGradient({  angle: 90,  colors: [[0x00BAAD, 0.0], [0xDCF5F3, 0.6], [0xFFCCA6, 1.0]]})
.padding({left:15,right:15,top:px2vp(this.topRectHeight)})
.width('100%')
.height(70)
```


上面的代码首先定义导航栏的高度是70，然后留出了顶部的状态栏区域，并设置了背景渐变色。这时候返回按钮和标题都分布在左侧。

除了返回按钮和标题，有时候我们在导航栏右侧也需要添加一些组件：

```
@BuilderParam menuBuilderParam?: () => void; 

//右侧内容 
Row(){  
  this.menuBuilderParam()
}.constraintSize({  maxWidth:'50%'})

```

导航栏的标题有时候我们需要它居中，那就在中间再添加一个标题组件，并使用一个状态判断标题是在左侧还是在中间：

```
//枚举标题位置
export enum YLTitleMode {  Center,  Left} 

Row(){  
    Image($r('app.media.back'))      
      .width(20)      
      .height(20)      
      .objectFit(ImageFit.Contain)  

  if(this.titleModel == YLTitleMode.Left){    
    Text(this.title)     
      .fontColor(Color.Black)      
      .fontSize(23)      
      .fontWeight(FontWeight.Bold)      
      .margin({left:5})      
      .maxLines(1)  
    }
  }
  if(this.titleModel == YLTitleMode.Center){
    Text(this.title)  
      .maxLines(1)  
      .fontSize(23)  
      .fontColor(Color.Black)  
      .fontWeight(FontWeight.Bold)  
      .textOverflow({ overflow: TextOverflow.Ellipsis })  
      .textAlign(TextAlign.Center)
    }

```

现在有一个问题，导航栏在左、右都有内容的情况下，怎么保持标题的居中？首先想到的是使用SpaceBetween对齐方式，但是这种对齐方式是相对于两侧内容的对齐，只有左右内容长度一致的情况下标题才能居中。

所以我想计算两侧组件的长度，并使他们保持一致：

```
//右边布局 
Row(){  
  this.menuBuilderParam()
}
.justifyContent(FlexAlign.End)
.constraintSize({maxWidth:'40%'})
.width(parseInt(this.sizeValue)>parseInt(this.sizeValue2)?this.sizeValue:this.sizeValue2)
.onSizeChange((oldValue: SizeOptions, newValue: SizeOptions)=>{  
  this.sizeValue = JSON.stringify(newValue.width)
 })
```


这样的话当左侧也有自定义的内容时，计算两侧内容的长度，使用较大的值作为他俩的长度，标题就可以绝对居中了。

可能这么介绍会让很多友友摸不着头脑，我就分享代码大家直接拿过去用就行，发一个使用示例：

```
YLNavigation({title:'自定义导航栏',menuBuilderParam:this.menu,customBuilder:this.mainPage,hideBackButton:true,titleModel:YLTitleMode.Left})

```
