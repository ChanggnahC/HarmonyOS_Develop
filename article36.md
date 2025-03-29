# 鸿蒙Next元服务开发详解
之前写过关于元服务的文章，大家对元服务应该也有一定的了解，它是一种更加高效便捷的应用形式，免安装，有独立的入口，说的简单一点就像是把微信小程序放到系统层面，相比微信小程序更加快捷，因为连微信也不用打开了。

今天就分享一下怎么开发一个鸿蒙元服务。
![image](https://github.com/user-attachments/assets/9b6eb93b-404f-4ee1-9874-6e0f7ddea871)

创建项目

元服务的创建和普通应用有所区别，我们要创建的是Atomic Service，模版选择Empty Ability就可以。
![image](https://github.com/user-attachments/assets/fb1a77f6-73e6-4164-b426-a3036c5b8f8e)


接下来要为元服务注册App ID，元服务和微信小程序类似，必须要有App ID才能进行开发。
![image](https://github.com/user-attachments/assets/36c52c59-03d8-416f-965c-68f91c82988f)


好在这一步比较方便，点注册填一下名称等信息就可以了，再回到工程页面，一个元服务工程就创建好了。看一下工程目录好像和普通应用并没有什么不同。

![image](https://github.com/user-attachments/assets/c3d5bb06-862f-46da-b6d3-f21c6813333e)

配置图标

在entry文件夹右键新建Image Asset，选择一个1024*1024的图片就可以设置为元服务的图标了。
![image](https://github.com/user-attachments/assets/790403dc-8442-4810-9c08-13ecc851bcc5)


开发元服务页面

现在进入正式的开发环节，依然打开pages文件夹下的Index文件，这里同样是元服务的内容页面，看一下好像比普通应用多了一些代码，不必理会。
![image](https://github.com/user-attachments/assets/fc52893f-3b21-447e-9d70-20dba9e4dd24)

在元服务中页面的开发、页面之间的跳转和普通应用一样，可以添加一个按钮感受一下：

```
Column(){  
  Text(this.message)    
    .id('HelloWorld')    
     .fontSize(50)    
     .fontWeight(FontWeight.Bold)    
     .alignRules({      
       center: { anchor: '__container__', align: VerticalAlign.Center },      
       middle: { anchor: '__container__', align: HorizontalAlign.Center }    
     })  
   Button('按钮')    
     .fontColor(Color.White)    
     .fontSize(15)    
     .width(100)    
     .margin({top:40})    
     .onClick(()=>{    
       })
      }
      .justifyContent(FlexAlign.Center)
      .alignItems(HorizontalAlign.Center)
      .height('100%')
      .width('100%')
```

![image](https://github.com/user-attachments/assets/e44d40d9-194c-4ee0-aa00-bb51878aa51b)


都差不多，不再赘述，我们重点说一下和普通应用开发不一样的地方，比如网络请求。

在元服务中，网络请求除了需要配置网络权限外，还要在应用中心配置域名白名单，这一点又和微信小程序类似。

![image](https://github.com/user-attachments/assets/c8931479-974c-4bff-a0c3-b7138bb101ad)


打开应用中心的开发管理就可以配置域名，不过这种方式是有延迟和缓存的，在开发阶段可以在手机的开发者选项中开发元服务豁免开关，取消对域名的访问限制，然后就可以进行网络请求了。我找了一个天气的接口：

```
let url = 'https://.com/api/weather?city=%E6%9E%A3%E5%BA%84%E6%BB%95%E5%B7%9E'
let result: ResultData = await HttpManager.getInstance().request({  
  method: RequestMethod.GET,  
  url: url
})
let resultData: WeatherItem[] = result.data.data
```


开发卡片页面

卡片是元服务的一种快捷操作和入口，现在我们尝试常见一个卡片，在entey文件夹右键新建Dynamic Widget，创建完成之后可以发现工程目录中除了新建的卡片，还新增了entryformability文件夹，这是卡片的生命周期函数。

![image](https://github.com/user-attachments/assets/a980d5c2-170d-4002-8698-32e64e9e300f)


现在我们尝试在卡片中开发一些简单的页面，比如一个天气页面：

```
Row() {
  Column() {
    Column(){
      Text(this.city)
        .fontSize(14)
        .fontColor(Color.White)
      Text(this.wendu )
        .fontWeight(FontWeight.Bold)
        .fontSize(20)
        .fontColor(Color.White)
        .margin({top:5})
    }
    Text(this.tianqi + '  空气' + this.pm)
      .fontColor(Color.White)
      .fontSize(13)
  }  .justifyContent(FlexAlign.SpaceBetween)
  .width(this.FULL_WIDTH_PERCENT)
  .alignItems(HorizontalAlign.Start)
  .height(this.FULL_HEIGHT_PERCENT)
  Image($r('app.media.clear'))
    .width(25)
    .height(25)
    .objectFit(ImageFit.Contain)
      .onClick(() => {
        postCardAction(this, {
          action: 'message', 
         params: { msgTest: 'messageEvent' }
        });
      })}.alignItems(VerticalAlign.Top).padding({left:15,right:40,top:15,bottom:15}).linearGradient({  direction: GradientDirection.Bottom, // 渐变方向  repeating: false, // 渐变颜色是否重复  colors: [[0x9FCCE0, 0.0], [0xC9E4DD, 1.0]]}).width(this.FULL_WIDTH_PERCENT).height(this.FULL_HEIGHT_PERCENT).onClick(() => {  postCardAction(this, {    action: this.ACTION_TYPE,    abilityName: this.ABILITY_NAME,
    params: {
      message: this.MESSAGE
    }  });})
```
![image](https://github.com/user-attachments/assets/349fe72a-9da2-4367-ac6c-4c83e28bd19a)


两个手指捏合屏幕，在列表里选择程序就可以把卡片添加到桌面。

数据传递

大家是否还记得刚才我在元服务中进行了一个网络请求，刚好也是一个天气数据接口，现在我们要做的是如何将元服务中的数据传递到卡片中进行展示。

需要告诉大家的是，元服务和卡片是两套进程，并不能直接进行通信。所以我们这里使用用户首选项，先在元服务中将数据保存，再从卡片中取出数据.

首先在主应用中保存数据：

```
let pf = await preferences.getPreferences(context, "CardData")
await pf.put('city', result.data.city)
await pf.put('tianqi', weatherItem.weather)
await pf.put('pm', weatherItem.air_quality)
await pf.put('wendu', weatherItem.temperature)pf.flush()
```


下面尝试在卡片里取出数据，这里涉及到卡片的刷新，卡片的刷讯分为主动刷新和定时刷新，今天今天先主要分享一个主动刷新。也就是通过用户的主动操作进行刷新，比如点击一个按钮,我给天气图片添加一个点击事件：

```
.onClick(() => {  
  postCardAction(this, {    
    action: 'message',    
    params: { msgTest: 'messageEvent' }  
  });
})
```
这时候点击图片会调用entryformability文件中的onFormEvent方法，我们在这里取出数据并更新页面：

```
onFormEvent(formId: string, message: string) {  
  // Called when a specified message event defined by the form provider is triggered.  
   this.updateUI(formId)
}
async updateUI(formId:string){  
  let pf = await preferences.getPreferences(this.context,'CardData')  
  let cityStr = await pf.get('city', '0')  
  let tianqi = await pf.get('tianqi', '0')  
  let pm = await pf.get('pm', '0')  
  let wendu = await pf.get('wendu', '0')  
  class FormDataClass {    
    city = cityStr   
     tianqi = tianqi    
     pm =  pm    
     wendu = wendu  
   }  
  let formData = new FormDataClass();  
  let formInfo: formBindingData.FormBindingData = formBindingData.createFormBindingData(formData);  
  formProvider.updateForm(formId, formInfo).then(() => {  }).catch((error: BusinessError) => {  })
}

```

以上就是开发一个元服务的简单介绍
