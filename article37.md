# 鸿蒙Next开发实战教程—电影app
最近忙忙活活写了不少教程，但是总感觉千篇一律，没什么意思，大家如果有感兴趣的项目可以私信给幽蓝君写一写。

今天分享一个电影App。
![image](https://github.com/user-attachments/assets/2342b95e-2f4b-431b-8324-0edd9fb92dbe)

这个项目也比较简单，主要是一些简单页面的开发和本地视频的播放以及横竖屏切换。

页面搭建以首页为例，很明显这是一个List页面，不过每一个部分都可以左右滑动，顶部banner部分是支持翻页的，所以使用Swiper组件实现，其他部分均使用Scroll组件实现。相关代码如下：

```
build()
 {
  List(){
    ListItem(){
      Swiper(){
        Image($r('app.media.headimg'))
          .width('100%')
          .height(380)
          .objectFit(ImageFit.Fill)
          .onClick(()=>{
            router.pushUrl({
              url:'pages/Info'
            })
          })
      }
    }
    ListItemGroup({header:this.ListHeader('分类',false)}){
      ListItem(){
        Scroll(){
          Row(){
            ForEach(this.types,(str:string,index)=>{
              Text(str)
                .fontSize(15)
                .fontColor(Color.White) 
               .padding({top:8,bottom:8,left:22,right:22})
                .borderRadius(15)
                .backgroundColor(this.currentType == index ? '#6152FF':'rgb(6,4,31)')                .onClick(()=>{                  router.pushUrl({                    url:'pages/MovieList'                  })                })            })          }        }        .scrollable(ScrollDirection.Horizontal)        .scrollBar(BarState.Off)        .width('100%')      }    }    .padding({left:12,right:12})    ListItemGroup({header:this.ListHeader('最受欢迎',true)}){      ListItem(){        Scroll(){          Row({space:10}){            ForEach(this.popularMovies,(img:Resource,index)=>{              Image(img)                .width(124)                .height(180)                .objectFit(ImageFit.Contain)                .borderRadius(10)                .onClick(()=>{                  router.pushUrl({                    url:'pages/Info'                  })                })            })          }        }        .scrollable(ScrollDirection.Horizontal)        .scrollBar(BarState.Off)        .width('100%')      }    }    .padding({left:12,right:12})    ListItemGroup({header:this.ListHeader('最新电影',true)}){      ListItem(){        Scroll(){          Row({space:10}){            ForEach(this.popularMovies,(img:Resource,index)=>{              Image(img)                .width(124)                .height(180)                .objectFit(ImageFit.Contain)                .borderRadius(10)                .onClick(()=>{                  router.pushUrl({                    url:'pages/Info'                  })                })            })          }        }        .scrollable(ScrollDirection.Horizontal)        .scrollBar(BarState.Off)        .width('100%')      }    }    .padding({left:12,right:12})  }  .width('100%')  .height('100%')  .backgroundColor('rgb(6,4,31)')}@Builder ListHeader(title:string,isRight:boolean){  Row(){    Text(title)      .fontColor(Color.White)      .fontSize(15)    if(isRight){      Text('查看全部')        .fontColor(Color.White)        .fontSize(11)    }  }  .alignItems(VerticalAlign.Center)  .width('100%')  .height(40)  .justifyContent(FlexAlign.SpaceBetween)  .onClick(()=>{    router.pushUrl({      url:'pages/MovieList'    })  })}


```
页面开发就说这么多，下面进入视频处理部分，这里使用的本地视频，首先把视频文件拖进rawfile文件夹中，然后使用video组件播放视频：

```
Video({  src: $rawfile('movie.mp4'),  controller: this.controller})  
.width('100%')  
.height(this.isLandscapeStart?'100%':this.screen_width * 9 / 16)  
.autoPlay(true)  
.controls(false)  
.objectFit(ImageFit.Cover)  
.loop(false)
```


Video组件提供了工具栏和全屏方法，不过效果不好，非常丑陋，实际项目中我们通常需要自定义工具栏，全屏也通过设置组件的宽高尺寸来实现，我这里就简单添加一个全屏按钮，下面演示一下如何对视频进行横竖屏切换。

设置横屏，也就是全屏模式的步骤分别是先获取到当前窗口，然后将状态栏、导航栏隐藏，再将窗口横屏，相关代码如下：

```
changeOrientation() {  
  // 获取UIAbility实例的上下文信息  
  let context = getContext(this);  
  // 调用该接口手动改变设备横竖屏状态（设置全屏模式，先强制横屏，再加上传感器模式）  
  window.getLastWindow(context).then((lastWindow) => {    
    if (this.isLandscapeStart) {      
      // 设置窗口的布局是否为沉浸式布局      
      lastWindow.setWindowLayoutFullScreen(true).then(() => {        
        // 设置窗口全屏模式时导航栏、状态栏的可见模式        
        lastWindow.setWindowSystemBarEnable([]);        
        // 设置窗口的显示方向属性，AUTO_ROTATION_LANDSCAPE表示传感器自动横向旋转模式        
        lastWindow.setPreferredOrientation(window.Orientation.AUTO_ROTATION_LANDSCAPE, () => {          
          this.isLandscape = !this.isLandscape;        
        });      
      });    
    } 
   });
 }
```


退出全屏时将上面代码反向操作就行了。
