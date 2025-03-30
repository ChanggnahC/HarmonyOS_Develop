# HarmonyOS NEXT开发实战教程：选择相册和拍照
今天的内容是介绍在鸿蒙开发中从相册选择照片，和调用相机拍照，并使用这两个功能实现朋友圈编辑页面。
![image](https://github.com/user-attachments/assets/5c96150c-8515-4644-82ed-0c7751e6ed2c)

这部分内容没什么好废话的，都是固定用法，直接上代码。
首先添加权限：

```
ohos.permission.CAMERA

```
选择相册：

```
async  getAlbum() {
    const photoSelectOptions = new photoAccessHelper.PhotoSelectOptions();
    photoSelectOptions.MIMEType = photoAccessHelper.PhotoViewMIMETypes.IMAGE_TYPE; // 过滤选择媒体文件类型为IMAGE
    photoSelectOptions.maxSelectNumber = 5; // 选择媒体文件的最大数目
    let uris: Array<string> = [];
    const photoViewPicker = new photoAccessHelper.PhotoViewPicker();
    photoViewPicker.select(photoSelectOptions).then((photoSelectResult: photoAccessHelper.PhotoSelectResult) => {
      uris = photoSelectResult.photoUris;
      uris.forEach(element => {
        this.photoList.push(element)
      });
      console.info('photoViewPicker.select to file succeed and uris are:' + uris);
    }).catch((err: BusinessError) => {
      console.error(`Invoke photoViewPicker.select failed, code is ${err.code}, message is ${err.message}`);
    })
  }

```

调用相机：

```
async invokeCamera(callback: (uri: string) => void) {
    try {
      let pickerProfile: cameraPicker.PickerProfile = {
        cameraPosition: camera.CameraPosition.CAMERA_POSITION_BACK
      };
      let pickerResult: cameraPicker.PickerResult = await cameraPicker.pick(getContext(),
        [cameraPicker.PickerMediaType.PHOTO, cameraPicker.PickerMediaType.VIDEO], pickerProfile);
      console.log("the pick pickerResult is:" + JSON.stringify(pickerResult));
      if (callback && pickerResult) {
        callback(pickerResult.resultUri);
      }
    } catch (error) {
      let err = error as BusinessError;
      console.error(`the pick call failed. error code: ${err.code}`);
    }
  }
```


整个页面完整代码如下：

```
import { photoAccessHelper } from '@kit.MediaLibraryKit';
import { BusinessError } from '@kit.BasicServicesKit';
import { cameraPicker } from '@kit.CameraKit';
import { camera } from '@kit.CameraKit';
import { AlertSheet } from './AlertSheet';

@Entry
@Component
struct Index {

  @State photoList:string[] = []

  dialogController: CustomDialogController | null = new CustomDialogController({
    builder: AlertSheet({titles:['拍照','相册'],itemClick:(str)=>{
      switch (str){
        case '拍照':
          this.invokeCamera((url)=>{
            this.photoList.push(url)
          })
          break;
        case '相册':
          this.getAlbum()
          break;

      }
    }}),
    alignment: DialogAlignment.Bottom,
    offset: { dx: 0, dy: -25 }
  })

  async  getAlbum() {
    const photoSelectOptions = new photoAccessHelper.PhotoSelectOptions();
    photoSelectOptions.MIMEType = photoAccessHelper.PhotoViewMIMETypes.IMAGE_TYPE; // 过滤选择媒体文件类型为IMAGE
    photoSelectOptions.maxSelectNumber = 5; // 选择媒体文件的最大数目
    let uris: Array<string> = [];
    const photoViewPicker = new photoAccessHelper.PhotoViewPicker();
    photoViewPicker.select(photoSelectOptions).then((photoSelectResult: photoAccessHelper.PhotoSelectResult) => {
      uris = photoSelectResult.photoUris;
      uris.forEach(element => {
        this.photoList.push(element)
      });
      console.info('photoViewPicker.select to file succeed and uris are:' + uris);
    }).catch((err: BusinessError) => {
      console.error(`Invoke photoViewPicker.select failed, code is ${err.code}, message is ${err.message}`);
    })
  }

  async invokeCamera(callback: (uri: string) => void) {
    try {
      let pickerProfile: cameraPicker.PickerProfile = {
        cameraPosition: camera.CameraPosition.CAMERA_POSITION_BACK
      };
      let pickerResult: cameraPicker.PickerResult = await cameraPicker.pick(getContext(),
        [cameraPicker.PickerMediaType.PHOTO, cameraPicker.PickerMediaType.VIDEO], pickerProfile);
      console.log("the pick pickerResult is:" + JSON.stringify(pickerResult));
      if (callback && pickerResult) {
        callback(pickerResult.resultUri);
      }
    } catch (error) {
      let err = error as BusinessError;
      console.error(`the pick call failed. error code: ${err.code}`);
    }
  }

  build() {
    Column(){
      TextArea({placeholder:'这一刻的想法'})
        .placeholderFont({size:16})
        .placeholderColor('rgb(188,188,188)')
        .width('100%')
        .height(100)
        .backgroundColor(Color.White)

      Grid(){
        ForEach(this.photoList,(str:string,index)=>{
          GridItem(){
            Image(str)
              .width(90)
              .height(90)
          }
        })
        GridItem(){
          Image($r('app.media.add'))
            .width(90)
            .height(90)
            .onClick(()=>{
              this.dialogController?.open()
            })
        }
      }
      .columnsGap(10)
      .rowsGap(10)
      .columnsTemplate('1fr 1fr 1fr 1fr')
    }
    .padding({left:15,right:15,top:40})

  }
}
```
