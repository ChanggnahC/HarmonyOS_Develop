# 在鸿蒙NEXT开发中实现一个语音识别组件
鸿蒙系统发布以后都不知道叫它5.0版本还是NEXT版本了，哈哈，反正是最新版本就对了。
对于语音转换文字，鸿蒙系统提供了离线语音识别模型speechRecognizer，语种目前支持中文，识别效果非常不错。
![image](https://github.com/user-attachments/assets/9601b152-9de6-41d8-8f8f-bda7bf628178)

今天要分享的是使用speechRecognizer实现一个语音识别组件。
要实现语音识别，首先要配置麦克风使用权限：

ohos.permission.MICROPHONE


然后我们要检查应用是否获取到了麦克风权限，这一步操作很多时候都会用到，所以为大家封装一个通用方法：

```
import { abilityAccessCtrl, bundleManager, common, Permissions } from '@kit.AbilityKit';

export class PermissionManager {
  static checkPermission(permissions: Permissions[]): boolean {
    let atManager: abilityAccessCtrl.AtManager = abilityAccessCtrl.createAtManager();
    let tokenID: number = 0;
    const bundleInfo =
      bundleManager.getBundleInfoForSelfSync(bundleManager.BundleFlag.GET_BUNDLE_INFO_WITH_APPLICATION);

    tokenID = bundleInfo.appInfo.accessTokenId;
    if (permissions.length === 0) {
      return false;
    } else {
      return permissions.every(permission =>
      abilityAccessCtrl.GrantStatus.PERMISSION_GRANTED ===
      atManager.checkAccessTokenSync(tokenID, permission)
      );
    }
  }

  static async requestPermission(permissions: Permissions[]): Promise<boolean> {
    let atManager: abilityAccessCtrl.AtManager = abilityAccessCtrl.createAtManager();
    let context: Context = getContext() as common.UIAbilityContext;
    const result = await atManager.requestPermissionsFromUser(context, permissions);
    return !!result.authResults.length && result.authResults.every(authResults => authResults === 0);
  }
}
```


语音识别的使用分为创建引擎、设置回调、开始监听几个步骤，同样为大家封装一个实体类：

```
import { speechRecognizer } from '@kit.CoreSpeechKit';

class SpeechRecognizerManager {
  /**
   * 语种信息
   * 语音模式：长
   */
  private static extraParam: Record<string, Object> = { "locate": "CN", "recognizerMode": "long" };
  private static initParamsInfo: speechRecognizer.CreateEngineParams = {
    /**
     * 地区信息
     * */
    language: 'zh-CN',
    /**
     * 离线模式：1
     */
    online: 1,
    extraParams: this.extraParam
  };
  /**
   * 引擎
   */
  private static asrEngine: speechRecognizer.SpeechRecognitionEngine | null = null
  /**
   * 录音结果
   */
  static speechResult: speechRecognizer.SpeechRecognitionResult | null = null
  /**
   * 会话ID
   */
  private static sessionId: string = "as" + Date.now()

  /**
   * 创建引擎
   */
  private static async createEngine() {
    // 设置创建引擎参数
    SpeechRecognizerManager.asrEngine = await speechRecognizer.createEngine(SpeechRecognizerManager.initParamsInfo)
  }

  /**
   * 设置回调
   */
  private static setListener(callback: (srr: speechRecognizer.SpeechRecognitionResult) => void = () => {
  }) {
    // 创建回调对象
    let setListener: speechRecognizer.RecognitionListener = {
      // 开始识别成功回调
      onStart(sessionId: string, eventMessage: string) {
        console.log('onstart')
      },
      // 事件回调
      onEvent(sessionId: string, eventCode: number, eventMessage: string) {
      },
      // 识别结果回调，包括中间结果和最终结果
      onResult(sessionId: string, result: speechRecognizer.SpeechRecognitionResult) {
        SpeechRecognizerManager.speechResult = result
        callback && callback(result)
      },
      // 识别完成回调
      onComplete(sessionId: string, eventMessage: string) {
        console.log('complete')
      },
      // 错误回调，错误码通过本方法返回
      // 如：返回错误码1002200006，识别引擎正忙，引擎正在识别中
      // 更多错误码请参考错误码参考
      onError(sessionId: string, errorCode: number, errorMessage: string) {
        console.log('error')
      },
    }
    // 设置回调
    SpeechRecognizerManager.asrEngine?.setListener(setListener);
  }

  /**
   * 开始监听
   * */
  static startListening() {
    // 设置开始识别的相关参数
    let recognizerParams: speechRecognizer.StartParams = {
      // 会话id
      sessionId: SpeechRecognizerManager.sessionId,
      // 音频配置信息。
      audioInfo: {
        // 音频类型。当前仅支持“pcm”
        audioType: 'pcm',
        // 音频的采样率。当前仅支持16000采样率
        sampleRate: 16000,
        // 音频返回的通道数信息。当前仅支持通道1。
        soundChannel: 1,
        // 音频返回的采样位数。当前仅支持16位
        sampleBit: 16
      },
      //   录音识别
      extraParams: {
        // 0:实时录音识别  会自动打开麦克风 录制实时语音
        "recognitionMode": 0,
        //   最大支持音频时长
        maxAudioDuration: 60000
      }
    }
    // 调用开始识别方法
    SpeechRecognizerManager.asrEngine?.startListening(recognizerParams);
  };

  /**
   * 取消识别
   */
  static cancel() {
    SpeechRecognizerManager.asrEngine?.cancel(SpeechRecognizerManager.sessionId)
  }

  /**
   * 释放ai语音转文字引擎
   */
  static shutDown() {
    SpeechRecognizerManager.asrEngine?.shutdown()
  }

  /**
   * 停止并且释放资源
   */
  static async release() {
    SpeechRecognizerManager.cancel()
    SpeechRecognizerManager.shutDown()

  }

  /**
   * 初始化ai语音转文字引擎
   */
  static async init(callback: (srr: speechRecognizer.SpeechRecognitionResult) => void = () => {
  }) {
    try {
      await SpeechRecognizerManager.createEngine()
    } catch (err) {
      console.log('err',JSON.stringify(err))
    }

    SpeechRecognizerManager.setListener(callback)
    SpeechRecognizerManager.startListening()
  }
}

export default SpeechRecognizerManager
```


最后在需要使用的地方调用上面封装的方法就可以了：

```
const permissions: Permissions[] = ["ohos.permission.MICROPHONE"]
    // 检查是否拥有权限
    const isPermission = await PermissionManager.checkPermission(permissions)
    if (!isPermission) {
      //   如果没权限，就主动申请
      PermissionManager.requestPermission(permissions)
    }else {
      SpeechRecognizerManager.init(res => {
        console.log("实时语音识别", JSON.stringify(res))
        this.text = res.result
      })
    }
```
