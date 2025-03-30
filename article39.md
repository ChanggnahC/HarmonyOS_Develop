# 鸿蒙NEXT开发教程：浅谈@ComponentV2装饰器
听说今天的广州车展上有一部分人已经看到华为汽车的最后一“界”，尊界超豪华大轿车，应该很快就要正式亮相，可以期待一波。

在api12之后，鸿蒙系统推出一个V2版本的状态管理装饰器，不过目前还在开发试用状态，幽蓝君仔细研究了一下，今天跟大家做一个简单的介绍。

幽蓝君对V2版本装饰器的总结是，它是为了修补目前版本状态管理的不足，新增了一些功能更加精细的状态变量装饰器，等到开发试用完善之后应该会和当前装饰器合并到一起，不再使用V2区分。

下面跟大家详细介绍一下@ComponentV2装饰器。

@ComponentV2和@Component一样，都用于装饰自定义组件，但是@ComponentV2内部只能使用全新的状态变量装饰器，比如@Local、@param、@Once、@Event、@Provider、@Consumer等。这几个装饰器有什么作用呢，请往下看：

@Local：组件内部状态

@Local的作用和现在的@State类似，但是鸿蒙研发团队认为@State装饰器可以从外部初始化是一个缺陷，所以@Local是一个组件内部状态，它不接受从外部进行的初始化，下面我写了一段demo，让我们来感受一下：
```
@ComponentV2 struct TextV2{
  @Local text:string = 'v2'
  build() {
    Text(this.text)
      .fontColor(Color.Brown)
      .fontSize(20)
  }
}

@Component struct TextV1{
  @State text:string = 'v1'
  build() {
    Text(this.text)
      .fontColor(Color.Grey)
      .fontSize(20)
  }
}


```



可以看到，当我向子组件传值时，@Local修饰符是不接受的。

@Param：组件外部输入

和@Local相反，@Param表示组件从外部传入的状态，它是为了父子组件之间的数据能够进行同步。但是它是不支持组件内部进行修改的：

![image](https://github.com/user-attachments/assets/a7ec368a-a726-4e7d-9531-78a7000a6e82)

@Once：初始化同步一次

这个装饰器看单词就比较好理解，只能修改一次，不过它的局限性比较大，必须和@Param配合使用,不能单独使用，它是为了实现仅从外部初始化一次、不接受后续同步变化的能力。

@Param @Once text:string = 'v2'


@Event组件输出

@Event也和@Param有关系，因为@Param修饰的变量无法从内部修改，但是有时它又需要修改，那怎么办呢，@Event装饰的事件负责告诉父组件，再由父组件进行修改。好像有点多此一举:

```
@ComponentV2 struct TextV2{
  @Param text:string = 'v2'
  @Event changeText: (x: string) => void = (x: string) => {};

  build() {
    Column(){
      Text(this.text)
        .fontColor(Color.Brown)
        .fontSize(20)
        .onClick(()=>{
          this.changeText('hahaha')
        })
    }
  }
```


@Provider装饰器和@Consumer装饰器：跨组件层级双向同步

我们在V1版本里都使用过@Provide和@Consume，是用来跨组件层级双向传递的装饰器，这两个长的很像，功能也类似，不过有一些细小的区别，直接跟大家截个图吧：

![image](https://github.com/user-attachments/assets/d19fb787-afeb-480e-8a93-503405f34385)

@Monitor：监听

我们之前使用@Watch来监听变量的变化，不过@Watch无法实现对对象、数组中某一单个属性或数组项变化的监听，且无法获取变化之前的值。而@Monitor可以:

```
@Local text:string = 'v2'
  //监听text
  @Monitor('text')
  onStrChange(monitor: IMonitor){
    monitor.dirty.forEach((path:string)=>{
      console.log('修改前：',monitor.value(path)?.before)
      console.log('修改后：',monitor.value(path)?.now
      )
    })
  }
```


@Computed:计算属性

@Computed用来装饰getter方法，并且会随着被计算的值的变化而变化，并且只会计算一次，另外，@Computed的计算性能更佳。文字比较难理解，举个例子：

```
@Local a:number = 10
@Local b:number = 20

@Computed
get c(){
  return this.a + this.b
}
```
