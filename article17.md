# 在鸿蒙Next中开发一个底部选项弹窗
今天大暑，酷暑难耐，蚊虫叮咬，幽蓝君依然在出差地饱受煎熬，打工人难呐。

今天和大家分享一个比较常见的底部选项弹窗，效果图如下：
![image](https://github.com/user-attachments/assets/9cd5f6cc-325a-4e58-9f9b-c14693b8ad6e)

这个弹窗基于系统的弹窗组件CustomDialogController开发，在它的builder参数中我们可以传入一些自定义的样式，所以我自定义一个选项列表：

```
@CustomDialog
struct AlertSheet {
  controller?: CustomDialogController
   @Prop titles:string[]
  itemClick:(str:string)=>void=()=>{}

  build() {
    Column() {
      ForEach(this.titles,(str:string,index)=>{
        Row(){
          Text(str)
            .fontColor('rgb(0,122,255)')
            .fontSize(17)
        }
        .alignItems(VerticalAlign.Center)
        .justifyContent(FlexAlign.Center)
        .width('100%')
        .height(50)
        .border({width:{bottom:0.5},color:{bottom:'rgb(216,216,216)'},style:{bottom:BorderStyle.Solid}})
        .onClick(()=>{
          this.controller?.close()
          this.itemClick(str)
        })
      })
      Row(){
        Text('取消')
          .fontColor('rgb(0,122,255)')
          .fontSize(18)
          .fontWeight(FontWeight.Bold)
      }
      .justifyContent(FlexAlign.Center)
      .alignItems(VerticalAlign.Center)
      .width('100%')
      .height(80)
      .onClick(()=>{
        this.controller?.close()
      })
    }
  }
}
```
引用的时候传入选项内容和回调事件就可以了：

```
dialogController: CustomDialogController = new CustomDialogController({
    builder: AlertSheet({titles:['选项1','选项2','选项3','选项4'],itemClick:(str)=>{
      console.log('点击了:' + str)
    }}),
    alignment: DialogAlignment.Bottom,
  })

```
