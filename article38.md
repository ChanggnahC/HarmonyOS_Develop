# 在鸿蒙开发中实现完整的注册登录流程
上次分享过一次注册登录的页面开发，不过不牵扯数据，今天加上数据存储实现一个完整的注册登录流程。

数据存储方式采用比较常用的本地存储方式，用户首选项来实现。

关于界面比较简单，之前的文章已经分享过，所以这里不再赘述，直接贴一个登录界面的代码，注册页面跟它也差不多：
```
build() {  
 Stack({alignContent:Alignment.Top}){    
   Stack({alignContent:Alignment.TopEnd}){      
      Image($r('app.media.log_img_1'))        
        .width('100%')      
      Image($r('app.media.log_img_2'))        
       .width(200)        
       .height(200)    }    
      Column(){     
      Row(){        
        Image($r('app.media.logo'))          
        .width(33)          
        .height(33)        
        Text('幽蓝计划')          
        .fontSize(27)          
    .fontColor(Color.Black)          
    .fontWeight(FontWeight.Bolder)          
     .margin({left:10})      }     
     Text('欢迎登录使用!')        
     .fontSize(18)        
     .fontColor('rgba(100, 106, 115, 1)')        
     .margin({top:8})      
     TextInput({placeholder:'请输入手机号'})        
     .placeholderColor('#8F959E')        
     .fontSize(15)        
     .fontColor(Color.Black)        
     .width('100%')        
     .height(50)        
     .borderRadius(10)        
     .borderWidth(1)       
      .borderColor('#D0D3D5')       
      .backgroundColor(Color.White)       
      .margin({top:50})       
       .onChange((value)=>{         
      this.telePhone = value      
       })     
      Row(){       
      TextInput({placeholder:'请输入验证码'})         
      .placeholderColor('#8F959E')        
       .fontSize(15)         
     .fontColor(Color.Black)         
     .width('70%')         
      .height(50)       
      .backgroundColor(Color.White)    
        .onChange((value)=>{
            this.psw = value
          })
        Row(){
        }
        .width(1)
        .height(30)
        .backgroundColor('rgba(237, 237, 237, 1)')
        .borderRadius(0.5)
        Text('获取验证码')
          .width('30%')
          .height(50)
          .fontColor('#FAB84F')
          .fontSize(15)
          .textAlign(TextAlign.Center)
      }
      .width('100%')
      .height(50)
      .borderRadius(10)
      .borderWidth(1)
      .borderColor('#D0D3D5')
      .backgroundColor(Color.White)
      .margin({top:25})
      Column(){
        Text('立即登录')
          .width('100%')
          .height(50)
          .backgroundColor('#FAB84F')
          .fontColor(Color.White)
          .fontSize(16)
          .textAlign(TextAlign.Center)
          .borderRadius(10)
          .onClick(()=>{
            this.login()
          })
      }
      .margin({top:75})
      .width('100%')
      .alignItems(HorizontalAlign.Center)
      Row(){
        Text("没有账号？") 
         .fontColor('rgba(143, 149, 158, 1)')
          .fontSize(14)
        Text("立即注册")
          .fontColor('#FAB84F')
          .fontSize(14)
          .onClick(()=>{
            router.pushUrl({
              url:'pages/Register'
            })
          })
      } 
     .margin({top:25})
    }
    .width('100%')
    .height('100%')
    .alignItems(HorizontalAlign.Start)
    .padding({left:30,right:30,top:140})
  }  .width('100%')  .height('100%')}
```

下面进入数据校验和存储部分，先来初始化一个用户首选项实例：

initData(){  
  dataPreference.getPreferences(getContext(this),'myStore',(err,preference)=>{    
    this.localPreference = preference  
  }
)}


这里的实例化方法有两种方式，除了使用上面的异步操作回调方法之外，还可以使用await来等待异步方法执行完成，不过这样的写的话方法名要加上async：

```
async initData(){  
  this.localPreference = await dataPreference.getPreferences(getContext(this),'myStore')
}

```

同样的，以后所有的异步操作我们都可以采用这种方式。

下一步，在用户输入完成之后我们要先对手机号格式进行验证，这里可以使用正则表达式，如下表达式是验证一个1开头的11位数字：

```
validTelphone(tel:string){  
  //正则表达式 1开头的11位整数  
   const reg = /^1\d{10}$/  
   return reg.test(tel)
}
```
然后验证账号是否已经注册：

```
let hasUser = await this.localPreference!.has('user')
if(hasUser){  
  let user = await this.localPreference!.get('user','default')  
  let userItem:User = JSON.parse(user.toString())  
  if(userItem.username == this.telePhone){    
    this.showDialog('用户已经存在')    
    return  
   }
}

```

手机号验证完成之后，对密码进行验证，这个比较简单，两次输入一致，6位数就行了。

一切验证完毕，就可以把账号密码存到本地：

```
this.localPreference!.put('user',JSON.stringify(user),(err)=>{  
  if(!err){    
    this.localPreference!.flush((err)=>{      
      if(!err){        
        this.showDialog('注册成功',()=>{          
          router.back()        
         })      
      }    
   }) 
  }
})
```


然后回到登录部分，登录部分相对简单了，先校验账号密码不为空之后和本地数据进行比对就行了：

```
dataPreference.getPreferences(getContext(this),'myStore',(err,preference)=>{  preference.has('user',(err,val)=>{    if(val){      preference.get('user','default',(err,value)=>{        let user:User = JSON.parse(value.toString())        if(user.username == this.telePhone && user.psw== this.psw){          EntryAbility.windowStage.loadContent('pages/TabView')        }else {          this.showDialog('用户名密码不正确')          return        }      })    }else {      this.showDialog('用户不存在')      return    }  })})

```
