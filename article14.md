# 鸿蒙Next开发实战教程--完整的购物商城项目
最近看到很多同学在寻找商城类项目的教程，幽蓝君马不停蹄在假期期间开发了一款鸿蒙商城app，非常适合学生朋友们学习。

本项目采用前后端分离设计，后端使用Apache+MySql+PHP，前端为鸿蒙原生。实现了完整的注册、登录、商品分类、购物车、购买、我的订单等功能，管理员可以在后端数据库对用户和商品进行增删改查操作。

下面跟大家分享本项目的一些知识要点。
![image](https://github.com/user-attachments/assets/5dcf6864-4e68-4672-b59e-72ccc5ec0ad9)


首页部分是一个List组件，内部分为三部分，banner图使用Swiper组件，商品分类和商品列表都使用Grid组件，具体代码如下：

```
List({space:8}){

          ListItem(){

            Swiper() {

              ForEach(this.data, (item:Resource) => {

                Image(item)

                  .size({ width: '100%', height: 200 })

                  .borderRadius(12)

                  .padding(8)

              })

            }

          }

          

          ListItem(){

            Grid(){

              ForEach(this.classData,(item:object,index:number) => {

                GridItem(){

                  Column(){

                    Image(item['cover'])

                      .width('100%')

                      .height('80%')

                    Text(item['classname'])

                      .fontColor(Color.Black)

                      .fontSize(14)

                      .margin({top:3})

                  }

                }

                .width(70)

                .height(90)

              })

            }

            .columnsTemplate('1fr 1fr 1fr 1fr')

            .rowsTemplate('1fr 1fr')

            .width('100%')

            .height(200)

            .backgroundColor(Color.White)

          }

          .width('100%')

          .height(200)



          ListItem(){

            Grid(){

              ForEach(this.goods,(item:object,index:number) => {

                GridItem(){

                  Column(){

                    Image(item['cover'])

                      .width('100%')

                      .height((this.screen_width - 25)/2)

                    Text(item['name'])

                      .fontColor(Color.Black)

                      .fontSize(17)

                      .margin({top:4})

                      .fontWeight(FontWeight.Bold)

                      .maxLines(2)

                    Text(item['price'])

                      .fontColor(Color.Red)

                      .fontSize(15)

                      .margin({top:6})

                  }

                  .alignItems(HorizontalAlign.Start)

                }

                .width((this.screen_width - 24)/2)

                .height(this.listHeight)

              })

            }

            .backgroundColor(Color.White)

            .columnsTemplate('1fr 1fr')

            .rowsGap(5)

            .columnsGap(5)

            .width('100%')

          }

          .width('100%')

        }

        .width('100%')

        .height('100%')
```


分类界面分为两部分，左侧使用List组件，右侧使用Grid组件，点击List列表后刷新右侧网格列表。

![image](https://github.com/user-attachments/assets/4f45f3be-81b5-47ea-9e76-ea6b814416ce)

代码如下：

```
Row() {

        List(){

          ForEach(this.classData,(item:object,index)=>{

            ListItem(){

              Text(item['classname'])

                .fontColor(Color.Gray)

                .fontSize(17)

            }

            .backgroundColor(index == this.classIndex? 'rgb(255,255,255)' : 'rgb(240,240,240)')

            .width(100)

            .height(55)

            .onClick(()=>{

              this.getSubClass(item['id'])

              this.classIndex = index

            })

          })

        }

        .width(100)

        .height('100%')

        .backgroundColor(Color.White)



        Grid(){

          ForEach(this.subClassData,(item:object,index)=>{

            GridItem(){

              Column(){

                Image(item['cover'])

                  .width('100%')

                  .height((this.screen_width - 100)/3)

                Text(item['name'])

                  .fontSize(15)

                  .fontColor(Color.Black)

              }

              .onClick(()=>{

               

              })

            }

            .backgroundColor(Color.White)

            .width((this.screen_width - 100)/3)

            .height((this.screen_width - 100)/3 + 20)

          })

        }

        .columnsTemplate('1fr 1fr 1fr')

        .columnsGap(5)

        .rowsGap(5)

        .width(this.screen_width - 100)

        .height('100%')

      }
```


其他页面也都比较简单，大多数使用list组件实现，就不再一一赘述了。
