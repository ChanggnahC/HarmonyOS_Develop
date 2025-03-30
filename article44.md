# HarmonyOS NEXT开发实战教程：聊天交友App
一早醒来Mate70上热搜了，余承东发文宣布Mate70要在本月发布，史上最强手机终于要来了。

今天分享一个交友app实战教程，是幽蓝君用整整一个周末开发的，时间有限，只做了些皮毛，不是很完善，不过拿来练手应该是足够了。

先上效果图:
![image](https://github.com/user-attachments/assets/ab4613ba-8a71-4c99-99e6-109c7a0fbb39)

下面为大家分享一下本项目的开发过程：

首页

首先当中首先要注意的是渐变色的背景，这个背景在项目的每一个页面中都有出现，这个背景我使用图片来实现，让图片充满整个页面，页面内容部分与背景图片层叠布局。相关代码如下：

```
Stack({alignContent:Alignment.Top}){
      Image($r('app.media.backImg'))
        .width('100%')
        .height('100%')
        .objectFit(ImageFit.Fill)
      Navigation(){
      //内容部分
      }
 }
```
然后进入首页的内容部分，首先是导航栏，很明显需要自定义，不过比较简单，一个横向布局搞定：

```
@Builder NavMenu(){
    Row(){
      Row({space:3}){
        Image($r('app.media.location'))
          .width(16)
          .height(16)
          .objectFit(ImageFit.Auto)
        Text('西安市')
          .fontSize(13)
          .fontColor('#333333')
      }
      .width(68)
      .height(26)
      .backgroundColor(Color.White)
      .borderRadius(13)


      Row({space:12}){
        ForEach(this.timeList,(str:string,index)=>{
          Column(){
            Text(str)
              .fontSize(this.timeIndex == index?20:18)
              .fontColor(this.timeIndex == index?BasicContant.TEXT_BLACK_COLOR:'rgb(129,129,129)')
              .height(21)
            if(this.timeIndex == index){
              Image($r('app.media.s_time'))
                .width(17)
                .height(10)
                .objectFit(ImageFit.Auto)
            }
          }
          .height(35)
          .justifyContent(FlexAlign.Start)
        })
      }


      Row(){
        Image($r('app.media.more'))
          .width(16)
          .height(16)
          .objectFit(ImageFit.Auto)
      }
      .width(68)
      .height(16)
      .justifyContent(FlexAlign.End)


    }
    .width('100%')
    .justifyContent(FlexAlign.SpaceBetween)
    .alignItems(VerticalAlign.Center)
    .height(44)
    .padding({left:12,right:12})
  }
```
接下来是人物列表，是一个瀑布流布局，瀑布流看起来很难，但是鸿蒙里直接提供了瀑布流组件WaterFlow，文档也比较详细，甚至直接复制过来就可以使用，需要注意的是我们首先要知道自己的数据列表里图片的尺寸。相关代码如下：

```
export class WaterFlowDataSource implements IDataSource {
  private dataArray: Object[] = []
  private listeners: DataChangeListener[] = []


   cardList:CardItem[] = [
    {cover:$r('app.media.girl_1'),name:'橙子',distance:'3.4km',width:358,height:495},
    {cover:$r('app.media.header1'),name:'橙子',distance:'3.4km',width:854,height:1280},
    {cover:$r('app.media.header2'),name:'橙子',distance:'3.4km',width:1280,height:853},
    {cover:$r('app.media.header3'),name:'橙子',distance:'3.4km',width:1050,height:1280},
    {cover:$r('app.media.header4'),name:'橙子',distance:'3.4km',width:853,height:1280},
     {cover:$r('app.media.header5'),name:'橙子',distance:'3.4km',width:692,height:1280},
     {cover:$r('app.media.header6'),name:'橙子',distance:'3.4km',width:853,height:1280},
  ]
  constructor() {
    this.dataArray = this.cardList
  }
  // 获取索引对应的数据
  public getData(index: number): Object {
    return this.dataArray[index]
  }
  // 通知控制器数据重新加载
  notifyDataReload(): void {
    this.listeners.forEach(listener => {
      listener.onDataReloaded()
    })
  }
  // 通知控制器数据增加
  notifyDataAdd(index: number): void {
    this.listeners.forEach(listener => {
      listener.onDataAdded(index)
    })
  }
  // 通知控制器数据变化
  notifyDataChange(index: number): void {
    this.listeners.forEach(listener => {
      listener.onDataChanged(index)
    })
  }
  // 通知控制器数据删除
  notifyDataDelete(index: number): void {
    this.listeners.forEach(listener => {
      listener.onDataDeleted(index)
    })
  }
  // 通知控制器数据位置变化
  notifyDataMove(from: number, to: number): void {
    this.listeners.forEach(listener => {
      listener.onDataMoved(from, to)
    })
  }
  // 获取数据总数
  public totalCount(): number {
    return this.dataArray.length
  }
  // 注册改变数据的控制器
  registerDataChangeListener(listener: DataChangeListener): void {
    if (this.listeners.indexOf(listener) < 0) {
      this.listeners.push(listener)
    }
  }
  // 注销改变数据的控制器
  unregisterDataChangeListener(listener: DataChangeListener): void {
    const pos = this.listeners.indexOf(listener)
    if (pos >= 0) {
      this.listeners.splice(pos, 1)
    }
  }
  // 增加数据
  public Add1stItem(): void {
    this.dataArray.splice(0, 0, this.dataArray.length)
    this.notifyDataAdd(0)
  }
  // 在数据尾部增加一个元素
  public AddLastItem(): void {
    this.dataArray.splice(this.dataArray.length, 0, this.dataArray.length)
    this.notifyDataAdd(this.dataArray.length-1)
  }
  // 在指定索引位置增加一个元素
  public AddItem(index: number): void {
    this.dataArray.splice(index, 0, this.dataArray.length)
    this.notifyDataAdd(index)
  }
  // 删除第一个元素
  public Delete1stItem(): void {
    this.dataArray.splice(0, 1)
    this.notifyDataDelete(0)
  }
  // 删除第二个元素
  public Delete2ndItem(): void {
    this.dataArray.splice(1, 1)
    this.notifyDataDelete(1)
  }
  // 删除最后一个元素
  public DeleteLastItem(): void {
    this.dataArray.splice(-1, 1)
    this.notifyDataDelete(this.dataArray.length)
  }
  // 重新加载数据
  public Reload(): void {
    this.dataArray.splice(1, 1)
    this.dataArray.splice(3, 2)
    this.notifyDataReload()
  }
}


WaterFlow() {
            LazyForEach(this.dataSource, (item: CardItem,index:number) => {
              FlowItem() {
                Card({cardItem:item})
                  .width(this.item_width)
                  .height(this.item_width * item.height/item.width)
              }


            }, (item: string) => item)
          }
          .columnsTemplate("1fr 1fr")
          .columnsGap(5)
          .rowsGap(5)
          .width('100%')
          .height('100%')
```
聊天页面

聊天页面其实也比较简单，写页面无非那几种布局方式，这里幽蓝君想讲一下消息气泡，自己的发的消息和别人发的消息我们可以使用FlexAlign.End和FlexAlign.Start两种对齐方式来实现气泡的靠左和靠右显示，然后这个气泡还有一个最大宽度，可以使用constraintSize来实现，以自己发的消息为例贴一段具体代码：

```
@Component struct MsgFromMe{
  build() {
    Row(){
      Text('这周六有空吗？一起去广场滑滑板可以吗？')
        .fontColor(Color.White)
        .fontSize(16)
        .lineHeight(22)
        .padding({left:24,right:24,top:16,bottom:16})
        .backgroundColor('rgba(154, 107, 249, 1)')
        .borderRadius({bottomLeft:24,topRight:24})
        .constraintSize({maxWidth:BasicContant.SCREEN_WIDTH*0.6})
    }
    .width('100%')
    .justifyContent(FlexAlign.End)
    .padding({top:12})
  }
}
```
以上就是本次交友App的简单讲解
