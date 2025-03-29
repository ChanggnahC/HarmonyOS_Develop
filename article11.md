# 鸿蒙Next开发教程之懒加载
今天分享的内容是懒加载。先跟初学者友友介绍一下什么是懒加载，以及为什么要使用懒加载。

懒加载是一种延迟加载技术，它允许在需要时才加载资源，如对象或数据，以提高系统性能和资源利用率。这样介绍比较抽象，举一个例子，当一个页面中加载大量图片时，下面分别是不使用懒加载和使用懒加载的效果：

不使用懒加载：
![image](https://github.com/user-attachments/assets/74fd8438-da8c-4d44-9346-ccee74e3655d)

使用懒加载：
![image](https://github.com/user-attachments/assets/83111126-676c-490b-9519-52c35341d4e1)

明显看出使用懒加载加载速度更快，用户体验更佳，因为懒加载是先加载屏幕中正在展示的数据。

在鸿蒙开发中，懒加载的使用方式一般是使用LazyForEach代替ForEach，但是数据源不能是通常的数组了，而是IDataSource类型，并且需要实现相关接口，下面是上文示例中的相关代码：

```
class WaterFlowDataSource implements IDataSource {



  private dataArray: Object[] = []

  private listeners: DataChangeListener[] = []

  constructor() {

    let data = getContext(this).resourceManager.getRawFileContent('images.json',(err,value)=> {

      let view: Uint8Array = new Uint8Array(value); // 使用Uint8Array读取arrayBuffer的数据

      let textDecoder: util.TextDecoder = util.TextDecoder.create(); // 调用util模块的TextDecoder类

      let res: string = textDecoder.decodeWithStream(view); // 对view解码

      let strArr:object[] = JSON.parse(res)



      for (let i = 0; i < strArr.length; i++) {

        this.dataArray.push(strArr[i])

      }

      this.notifyDataReload()

    })



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



class MyDataSource extends WaterFlowDataSource {

  private dataArray2: Object[] = [];



  public totalCount(): number {

    return this.dataArray2.length;

  }



  public getData(index: number): Object {

    return this.dataArray2[index];

  }



  public addData(index: number, data: Object): void {

    this.dataArray2.splice(index, 0, data);

    this.notifyDataAdd(index);

  }



  public pushData(data: Object): void {

    this.dataArray2.push(data);

    this.notifyDataAdd(this.dataArray2.length - 1);

  }

}
```
实现懒加载：

```
@State dataSource: WaterFlowDataSource = new WaterFlowDataSource()
Grid(){

          LazyForEach(this.dataSource,(item:object,index:number)=>{

            GridItem(){

              Image(this.imageURL + item['cover'])

                .width((this.screen_width - 10)/2)

                .height((this.screen_width - 10)/2)

            }



          })

        }

        .columnsTemplate('1fr 1fr')

        .columnsGap(10)

        .rowsGap(10)

        .width('100%')

        .height('100%')
```


关于鸿蒙开发懒加载的内容就是这样，祝大家学习顺利。
