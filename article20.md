# 鸿蒙Next开发实战案例--图片预览器
今天分享一个简单的图片预览器，支持加载本地和网络图片，支持滑动翻页和默认页数。使用系统弹窗组件CustomDialogController实现。
![image](https://github.com/user-attachments/assets/82bf3bcd-e8a3-4bbf-9338-f4e4cc084bea)

首先搭建弹窗的内容部分，使用Swiper可以完美解决多张图片的滑动、翻页、默认页数等功能。

```
@CustomDialog
export default  struct PhotoBrowser {
  controller:CustomDialogController
  //图片列表
 @Prop images:Resource[]|string[]
  //默认第几张
  index:number = 0

  build() {
    Swiper(){
      ForEach(this.images,(str:string,index)=>{
        Image(str)
          .width('100%')
          .height('100%')
          .objectFit(ImageFit.Auto)
      })
    }
    .index(this.index)
    .indicatorStyle({
      color:Color.White
    })
    .loop(false)
    .backgroundColor(Color.Black)
    .width('100%')
    .height('100%')
    .onClick(()=>{
      this.controller.close()
    })
  }
}
```
内容部分写完后，我们要把它放进弹窗控制器里，要注意CustomDialogController默认样式不是全屏的，通过设置customStyle和offset属性可以让弹窗全屏：

```
dialogController: CustomDialogController = new CustomDialogController({
    builder: PhotoBrowser({images:this.imgList,index:1}),
    customStyle: true,
    offset: { dx: 0, dy: 0 },
    alignment: DialogAlignment.Top,
  })
```
使用时和普通弹窗一样：

 this.dialogController.open() 
