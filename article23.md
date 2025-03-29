# 鸿蒙Next开发自定义tabbar，带中间凸起按钮
今天要分享的是开发一个自定义tabbar，先看效果图：
![image](https://github.com/user-attachments/assets/8f933afa-a600-4011-8f99-28fa5c1577a8)

自己做的图标不太美观，大家见谅哈哈哈。

这种带中间凸起的tabbar在项目中非常常见，但是我研究了一下系统的tabbar是不支持这样设置的，所以我们就自己开发一个。

首先给每个tabbar的item定义一个class：

```
export class YLTabClass{

  //选中图标

  selectImage:Resource

  //默认图标

  defaultImage:Resource

  //中间是否凸起

  middleMode:boolean

  //标题

  title:string

  //是否选中

  isSelected:boolean = false



  constructor(selectImage:Resource,defaultImage:Resource,title:string,middleMode = false) {

    this.selectImage = selectImage

    this.defaultImage = defaultImage

    this.title = title

    this.middleMode = middleMode

  }

}

```

然后创建一个tabbar Item：

```
import {YLTabClass} from './YLTabCLass'

@Component

export struct YLTabbarItem{

  tabItem:YLTabClass

  @Prop isSelected:boolean

  build(){

    Column(){

      //根据isSelected字段设置图标，如果isSelected为true设置选中图标，否则为默认图标

      Image(this.isSelected ? this.tabItem.selectImage : this.tabItem.defaultImage)

        //如果是中间凸起按钮图片尺寸为45，否则是42

        .size({width:this.tabItem.middleMode ? 45 : 22,height:this.tabItem.middleMode ? 45 : 22})



      //为中间按钮外的其他按钮设置标题

      if(!this.tabItem.middleMode){

        Text(this.tabItem.title)

          .fontSize(12)

          .margin({top:6})

          .fontColor(this.isSelected ? '#3C8DFF' : '#B7B7B7')

      }

    }

    .width("100%")

    .height(56)

    .justifyContent(FlexAlign.Center)

  }

}
```


接下来，为tabbar创建5个按钮，记得把中间按钮的位置做调整：

```
import {YLTabClass} from './YLTabCLass'

import {YLTabbarItem} from './YLTabbarItem'



@Component

export  struct YLTabbar {



  tabItemClick:(index:number)=>void=()=>{}



  @State currentIndex:number = 0

  tab:YLTabClass[] = [

    new YLTabClass($r('app.media.tb01'),$r('app.media.tb00'),'首页'),

    new YLTabClass($r('app.media.tb11'),$r('app.media.tb10'),'行程'),

    new YLTabClass($r('app.media.tab_add'),$r('app.media.tab_add'),'',true),

    new YLTabClass($r('app.media.tb21'),$r('app.media.tb20'),'发现'),

    new YLTabClass($r('app.media.tb31'),$r('app.media.tb30'),'我的'),

  ]



  build() {

    Flex(){

      ForEach(this.tab,(item:YLTabClass,index:number)=>{

        YLTabbarItem({tabItem:item,isSelected:this.currentIndex === index})

          .offset({x:item.x,y:item.middleMode ? -25:0})

          .onClick(()=>{

            if(index != 2){

              this.currentIndex = index

            }

            console.log(index+ "" + this.currentIndex);

            this.tabItemClick(index);



          })

      })

    }

  }

}
```

这样一个自定义的tabbar就完成啦，不过我们还有一些工作要做。就是如何用它控制页面的切换，幽蓝君的思路是在页面上再放一个Tabs，将tabbar高度设为0，大致代码如下，供大家参考：

```
Tabs({ barPosition: BarPosition.End, controller: this.tabsController }) {

        TabContent() {

          home()

        }

        TabContent() {

          goals()

        }

        TabContent() {

          reccuring()

        }

        TabContent() {

          more()

        }

      }

      .vertical(false)

      .backgroundColor('#F1F3F5')

      .barHeight(0)

 

 TLTabbar()
```


今天的分享就是这样
