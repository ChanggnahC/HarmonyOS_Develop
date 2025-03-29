# 鸿蒙Next开发入门教程--@Prop和@Link装饰器

在后台的很多留言中，幽蓝君发现很多友友分不清楚@Prop和@Link装饰器，不明白他们到底有什么作用。

简单来说，@Prop装饰器的特点是父子单向同步，父组件中状态变量的改变会影响子组件，而子组件中状态的变量的改变不会影响父组件。@Link装饰的特点是父子双向同步，父子组件中的状态变量变化时会互相影响。下面我们通过一个例子感受一下：

首先在页面中创建一个Text组件用来显示变量，再创建一个按钮用来修改变量：
```
@State childNumber:number = 10
  
  build() {
    Row() {
      Column() {
        Text("父组件")
          .fontSize(20)
          .fontWeight(FontWeight.Bold)
        
        Text("变量：" + this.childNumber)
          .fontSize(20)
          .fontWeight(FontWeight.Bold)

        Button('修改变量')
          .onClick(()=>{
            this.childNumber += 1
          })
      }
      .width('100%')
    }
    .height('100%')
  }
```

![image](https://github.com/user-attachments/assets/49a0ac81-e765-49d0-8d69-df6e083555ef)

然后创建一个子组件，它的变量使用@prop修饰，其他部分和父组件相同：

```
@Prop childNumber:number
  build(){
    Column(){
      Text("prop组件")
        .fontSize(20)
        .fontWeight(FontWeight.Bold)

      Text("变量：" + this.childNumber)
        .fontSize(20)
        .fontWeight(FontWeight.Bold)

      Button('修改变量')
        .onClick(()=>{
          this.childNumber += 1
        })
    }
    .width(100)
    .height(100)
    .backgroundColor(Color.Pink)
  }
```

![image](https://github.com/user-attachments/assets/4b6745da-c291-4dae-bd03-910fb9b85496)

现在，点击父组件的按钮会发现两个变量同时变化，点击子组件中的按钮时父组件不会发生变化：

![image](https://github.com/user-attachments/assets/e6206e5f-444a-4a4d-8d70-7971b018de87)


再创建一个子组件，使用@Link装饰变量：

```
@Link childNumber:number
  build(){
    Column(){

      Text("link组件")
        .fontSize(20)
        .fontWeight(FontWeight.Bold)

      Text("变量：" + this.childNumber)
        .fontSize(20)
        .fontWeight(FontWeight.Bold)

      Button('修改变量')
        .onClick(()=>{
          this.childNumber += 1
        })
    }
    .width(100)
    .height(100)
    .backgroundColor(Color.Pink)
  }
```
观察变化：
![image](https://github.com/user-attachments/assets/f785a453-11db-4938-85df-1b73fbff270c)


可以看到当@Link修饰的变量变化父组件中的变量也会发生变化。

需要注意的是@Link和@Prop装饰器都只能用在子组件中，不能用在@Entry装饰的组件中。

今天的内容就到这里，感谢您的阅读。
