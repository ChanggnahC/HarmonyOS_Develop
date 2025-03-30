# 鸿蒙NEXT开发实战教程—文字识别
今天跟大家分享一个ocr文字识别的小项目：
![image](https://github.com/user-attachments/assets/83514657-adeb-4666-a553-8f43839da076)


鸿蒙系统提供了文字识别的能力，支持简体中文、英文、日文、韩文、繁体中文五种语言。实现步骤为初始化文字识别服务、将图片转换为PixelMap、文字识别、释放OCR服务。

首先从相册或者拍照获取图片，这一部分在之前的文章里有介绍，有疑问的同学可以查看这篇文章

HarmonyOS NEXT开发实战教程：选择相册和拍照

获取图片之后就可以进行图片的处理和识别，相关代码如下：

```
// 初始化 OCR 服务
const initResult = await textRecognition.init(); 
if (initResult) {   
  let imageSource: image.ImageSource | undefined = undefined;   
  let chooseImage: PixelMap | undefined = undefined;   
  let fileSource = await fileIo.open(this.ocrRecourse, fileIo.OpenMode.READ_ONLY);   
  imageSource = image.createImageSource(fileSource.fd);   
  console.log('file.fd:',);   
  chooseImage = await imageSource.createPixelMap();   
  hilog.info(0x0000, 'OCRDemo', `chooseImage：${chooseImage.toString()}`);   
  if (!chooseImage) {     return;   }   
  // 调用文本识别接口   
  let visionInfo: textRecognition.VisionInfo = {     
    pixelMap: chooseImage   
  };   
  textRecognition.recognizeText(visionInfo, (error: BusinessError, data: textRecognition.TextRecognitionResult) => {     
    if (error.code !== 0) {       
    // hilog.error(0x0000, 'OCRDemo', `Failed to recognize text. Code: ${error.code}, message: ${error.message}`);       
    return;     
  }     
  // 识别成功，获取对应的结果     
  let recognitionString = data.value.toString()     
  console.log('ocr识别结果：',JSON.stringify(recognitionString));     
  this.resultStr = data.value.toString()     
  router.pushUrl({       url:'pages/OCResultPage',       params:{         result:this.resultStr       }     })     
  if(chooseImage && imageSource) {       
    chooseImage.release();       
    imageSource.release();     
    }   });
    // 使用完毕后，释放 OCR 服务  
    await textRecognition.release();} 
  else {}
```
界面部分比较简单，直接贴个代码：

```
Navigation(){  
Column(){    
Image(this.ocrRecourse)      
.width(this.screen_width - 80)      
.height(450)      
.backgroundColor('rgb(240,240,240)')     
 .onClick(()=>{        
this.recogniZAction()      
})    
Row(){       
 Text('拍照')          
.fontSize(15)          
.fontColor(Color.White)          
.width(70)          
.height(30)          
.backgroundColor('rgb(18,136,119)')          
.textAlign(TextAlign.Center)          
.onClick(()=>{            
this.invokeCamera((url)=>{             
 this.ocrRecourse = url           
 })          })      

Text('相册')        
.fontSize(15)        
.fontColor(Color.White)        
.width(70)        
.height(30)        
.backgroundColor('rgb(18,136,119)')        
.textAlign(TextAlign.Center)        
.margin({left:40})        
.onClick(()=>{          
this.invokeAlbum((url)=>{            
this.ocrRecourse = url          
})        })    }   
 .width('100%')    
.justifyContent(FlexAlign.Center)    
.margin({top:60})   
 Text('识别')      
.width(180)     
 .height(40)     
 .fontSize(15)      
.textAlign(TextAlign.Center)   
   .fontColor(Color.White)    
  .backgroundColor('rgb(18,136,119)')     
 .margin({top:20})     
 .onClick(()=>{        this.recogniZAction()      })  }  
.justifyContent(FlexAlign.Center) 
 .alignItems(HorizontalAlign.Center) 
 .width('100%')  .height('100%')  
.backgroundColor(Color.White)  
.expandSafeArea([SafeAreaType.SYSTEM],[SafeAreaEdge.BOTTOM])}.width('100%').height('100%').titleMode(NavigationTitleMode.Mini).hideBackButton(true).backgroundColor('rgb(18,136,119)').title(this.Title())

```
