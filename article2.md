# 鸿蒙开发实战案例--用户隐私弹窗
受后台友友要求，今天开发一个用户隐私保护弹窗

首先创建一个自定义组件privacyDialog：
```
@CustomDialog 

struct  privacyDialog{

  build(){

  }
}
```

本文依然使用自定义弹窗CustomDialogController：

//弹窗控制器
controller: CustomDialogController
然后在自定义组件中添加相应内容，布局比较简单，不再细说，直接上代码：

```
Column(){

        Flex({direction:FlexDirection.Row,justifyContent:FlexAlign.SpaceBetween}){

          Text('用户隐私保护提示')

            .fontColor(Color.White)

            .fontSize(18)

            .fontWeight(FontWeight.Bold)

            .height(52)



          Row(){

            Image($r('app.media.img'))

              .width(68)

              .height(52)

              .margin({right:5})



            Image($r('app.media.close_img'))

              .width(20)

              .height(20)

              .onClick(()=>{

                this.controller.close()

              })



          }

          .height(52)

          .alignItems(VerticalAlign.Top)



        }

        .padding({top:18,left:12,right:12})

        .width(303)

        .height(70)

        .backgroundColor(Color.Green)

        .borderRadius({topLeft:4,topRight:5})



        Text('感谢您使用本小程序，您使用前应当阅读并同意《用户隐私保护指引》当您点击同意并继续使用该服务时，即表示您已理解并同意该条款内容。')

          .fontColor('#4a4a4a')

          .fontSize(17)

          .lineHeight(25)

          .margin({top:30})



        Text('同意')

          .width(240)

          .height(40)

          .backgroundColor('#50B452')

          .textAlign(TextAlign.Center)

          .fontColor(Color.White)

          .borderRadius(5)

          .margin({top:30})

          .onClick(()=>{

            this.controller.close()

          })



        Text('拒绝')

          .width(240)

          .height(40)

          .backgroundColor('#F7F7F7')

          .textAlign(TextAlign.Center)

          .fontColor('#4a4a4a')

          .borderRadius(5)

          .margin({top:20})

          .onClick(()=>{

            this.controller.close()

          })

      }

      .padding({left:12,right:12})

      .width(303)

      .height(353)

      .backgroundColor($r('app.color.color_white'))

      .borderRadius(5)
```
在需要使用弹窗的页面中创建弹窗：

```
loadingDialogController: CustomDialogController = new CustomDialogController({

    builder: privacyDialog(),

    autoCancel: false,

    customStyle:true

  })
```
注意此处customStyle设置true是为了自定义弹窗尺寸，customStyle设置false时点击空白区域弹窗不会关闭。最后创建一个按钮来打开弹窗：

```
Button('show')

          .fontColor(Color.White)

          .width(220)

          .height(60)

          .onClick(()=>{

            this.loadingDialogController.open()

          })
```
以上就是今天的案例分享，感谢您的阅读。
