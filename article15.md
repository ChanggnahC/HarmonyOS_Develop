# 在鸿蒙Next中开发一个月历组件
最近一直在出差，工作繁忙，很久没有时间更新文章了，连华为开发者大会也错过了。今天周末，忙里偷闲给大家分享一个鸿蒙月历组件。
![image](https://github.com/user-attachments/assets/a2df5f4a-d3e5-4ea3-82db-b6f4aebac3fd)

这样的组件大家在工作中应该经常会遇到，而鸿蒙又没有提供一个这样的系统组件，今天我们就来看看怎么实现它。

标题部分比较简单，由两个箭头图片和一个文本组件构成，不再赘述。

下面的‘星期’部分有很多方式去实现，这里我选择使用弹性布局Flex：

```
@State WEEKS:string[] = ['周日','周一','周二','周三','周四','周五','周六']

Flex({direction:FlexDirection.Row,wrap:FlexWrap.NoWrap}){
          ForEach(this.WEEKS,(str:string,index)=>{
            Text(str)
              .fontSize(14)
              .fontColor(Color.Gray)
              .textAlign(TextAlign.Center)
              .width('50%')
              .height(35)
          })
        }
```
最难的部分在于日历部分，难点在于我们首先要知道当前月份一共有多少天，还要知道从星期几开始排列，也就是这个月的1号是星期几。

在鸿蒙中只需要一行代码就可以获取当前月份的总天数：

```
const totalDays = new Date(year, month, 0).getDate();
再获取1号是星期几：

currentFirstWeekDay = new Date(year, month - 1, 1).getDay();
```


接下来，为了方便展示数据，我们除了要把总天数存入数组以外，还要在1号前补0。比如某个月的第一天是星期三，我们就要在前面补两个0，或者你如果把周日放在第一天就要补3个0。

```
for (let item = 0; item < currentFirstWeekDay; item++) {
      currentAllDay.push(0)
}
for (let item = 1; item <= totalDays; item++) {
      currentAllDay.push(item);
}
```


在创建数组的过程，你还可以插入一些其他需要展示的信息。

有了上面的数组之后就可以很容易的展示它们，你可以选择Grid网格组件，也可以使用Flex弹性布局组件：

```
Flex({direction:FlexDirection.Row,wrap:FlexWrap.Wrap}){
          ForEach(this.MONTHDAY,(calendar:Calendar,index)=>{
            Column({space:2}){
              Text(calendar.day.toString())
                .fontSize(16)
                .fontColor(Color.Black)
                .textAlign(TextAlign.Center)
                .fontWeight(FontWeight.Bold)
                Text(calendar.content)
                  .fontColor(Color.Gray)
                  .fontSize(10)
            }
            .justifyContent(FlexAlign.Center)
            .padding({top:4})
            .width('14.2%')
            .height(60)
            .alignItems(HorizontalAlign.Center)
            .visibility(calendar.day == 0? Visibility.Hidden:Visibility.Visible)
            .backgroundColor(Color.White)
          })
        }

```

下面为大家贴出完整代码：

```
@Entry
@Component
struct Index {
  @State WEEKS:string[] = ['周日','周一','周二','周三','周四','周五','周六']
  @State MONTHDAY:Calendar[] = []
  @State dateToday:Date = new Date()
  @State monthNow:number = this.dateToday.getMonth() + 1
  @State yearNow:number = this.dateToday.getFullYear()
  
  aboutToAppear(){
    this.getMonthContent()
  }

  getMonthContent(){
    const monthDays: number[] = this.getMonthDate(this.monthNow, this.yearNow);
    this.MONTHDAY = []
    for (let index = 0; index < monthDays.length; index++) {
      const num = monthDays[index];
      let calendarItem:Calendar = {
        year:this.yearNow,
        month:this.monthNow,
        day:num,
        content:'内容 ' + (Math.floor(Math.random() * 10)).toString()
      }
      this.MONTHDAY.push(calendarItem)
    }
  }

  getMonthDate(month: number, year: number): number[] {
    const SATURDAY: number = 6;
    let currentFirstWeekDay: number = 0;
    let currentLastWeekDay: number = 0;
    const currentAllDay: number[] = [];
    const totalDays = new Date(year, month, 0).getDate();
    //最后-1表示星期从周一开始，周日开始则不需要
    currentFirstWeekDay = new Date(year, month - 1, 1).getDay();
    currentLastWeekDay = new Date(year, month - 1, totalDays).getDay();
    for (let item = 0; item < currentFirstWeekDay; item++) {
      currentAllDay[item] = 0;
    }
    for (let item = 1; item <= totalDays; item++) {
      currentAllDay.push(item);
    }
    for (let item = 0; item < SATURDAY - currentLastWeekDay; item++) {
      currentAllDay.push(0);
    }
    return currentAllDay;
  }

  addZreo(num:number){
    return num > 9?num.toString() : '0' + num.toString()
  }

  lastMonth(){
    if(this.monthNow > 0){
      this.monthNow -= 1
    }else {
      this.monthNow = 12
      this.yearNow -= 1
    }
    this.getMonthContent()
  }
  nextMonth(){
    if(this.monthNow < 12){
      this.monthNow += 1
    }else {
      this.monthNow = 1
      this.yearNow += 1
    }
    this.getMonthContent()
  }

  build() {
    Column(){
      Row({space:20}){
        Image($r('app.media.last'))
          .width(25)
          .height(25)
          .onClick(()=>{
            this.lastMonth()
          })
        Text(this.yearNow.toString() + '-' + this.addZreo(this.monthNow))
          .fontSize(15)
          .fontColor(Color.Black)
          .fontWeight(FontWeight.Bold)
        Image($r('app.media.next'))
          .width(25)
          .height(25)
          .onClick(()=>{
            this.nextMonth()
          })
      }
      .width('100%')
      .height(40)
      .justifyContent(FlexAlign.Center)

      Column(){
        Flex({direction:FlexDirection.Row,wrap:FlexWrap.NoWrap}){
          ForEach(this.WEEKS,(str:string,index)=>{
            Text(str)
              .fontSize(14)
              .fontColor(Color.Gray)
              .textAlign(TextAlign.Center)
              .width('50%')
              .height(35)
          })
        }
        Flex({direction:FlexDirection.Row,wrap:FlexWrap.Wrap}){
          ForEach(this.MONTHDAY,(calendar:Calendar,index)=>{
            Column({space:2}){
              Text(calendar.day.toString())
                .fontSize(16)
                .fontColor(Color.Black)
                .textAlign(TextAlign.Center)
                .fontWeight(FontWeight.Bold)
                Text(calendar.content)
                  .fontColor(Color.Gray)
                  .fontSize(10)
            }
            .justifyContent(FlexAlign.Center)
            .padding({top:4})
            .width('14.2%')
            .height(60)
            .alignItems(HorizontalAlign.Center)
            .visibility(calendar.day == 0? Visibility.Hidden:Visibility.Visible)
            .backgroundColor(Color.White)
          })
        }
      }
    }
    .backgroundColor(Color.White)
    .width('100%')
    .padding({left:10,right:10})
  }
}
export interface Calendar {
  month: number; // 月
  day: number; // 日
  year:number; // 年
  content: string; // 内容
}
```
