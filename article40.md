# 鸿蒙NEXT开发实用技巧：通用工具类
今天分享一个幽蓝君自己在开发中的小技巧，就是封装一个通用工具类，之前大家如果下载过幽蓝君的代码可能也会发现这个东西。比如我们在开发中有一些比较常用的颜色、尺寸或者方法，都可以用一个类封装起来，这样不仅使用方便，统一修改也更加方便。

首先,创建一个和pages同级别的文件夹，在文件夹下创建一个新文件BasicConstant.ets,注意是ets文件，然后在这个文件里创建一个类，并简单定义一个颜色：

```
export class BasicConstant {

  static readonly TEXT_COLOR = 'rgb(74,74,74,)'

}
```
这样你就可以在项目的任何一个地方使用这个颜色：

```
Text('这是一个示例')
    .fontColor(BasicConstant.TEXT_COLOR)
    .fontSize(18)
```
不但如此，在其他项目中，你也可以把这个文件直接拖过去使用。

所以接下来我们再把这个类丰富一下，比如常用的字体大小：
```
static readonly TEXT_SIZE = 14

```
常用的背景色：

```
static readonly BACK_GRAY_COLOR = 'rgb(239,239,247)'

```

屏幕的尺寸：

```
static readonly SCREEN_WIDTH = px2vp(display.getDefaultDisplaySync().width)

static readonly SCREEN_HEIGHT = px2vp(display.getDefaultDisplaySync().height)
```


除了常量之外，还可以定义方法，比如时间字符串前面补0的方法：

```
static addZero(num:number){

 return num > 9 ? num.toString():'0'+num.toString()

}

```

比较时间大小的方法：

```
compareDate(date: string) {
    let cDate1 = new Date(date)
    let time1 = cDate1.getTime()

    let chooseDate2: string =
      this.dateToday.getFullYear() + '-' + BasicConstant.addZero(this.dateToday.getMonth() + 1) + '-' +
      BasicConstant.addZero(this.dateToday.getDate())

    let cDate2 = new Date(chooseDate2)
    let time2 = cDate2.getTime()
    return time1 >= time2
  }
```

其他的常用方法大家可以根据自己的需要进行补充，是不是很方便。
