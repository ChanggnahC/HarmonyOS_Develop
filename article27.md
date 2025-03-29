# HarmonyOS NEXT开发教程：加速web页面访问
在日常app开发中，访问web页面是很常见的功能，在鸿蒙系统中有多种方案来加速web页面的访问，提升用户体验。

首先，可以在Web组件的onAppear方法中对要加载的页面进行预链接，比如：

```
Web({ src: 'https://www.example.com/', controller: this.webviewController })
        .onAppear(() => {
          webview.WebviewController.prepareForPageLoad('https://www.example.com/', true, 2);
        })
```


以上代码中的prepareForPageLoad()方法也可以用在项目的Ability文件中初始化Web内核并对首页进行预连接：

```
onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {  
  hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');  
  webview.WebviewController.initializeWebEngine();  
  webview.WebviewController.prepareForPageLoad("https://www.example.com/", true, 2);  
  AppStorage.setOrCreate("abilityWant", want);
}

```

如果你的web链接内部也存在一些跳转，可以在Web组件的onPageEnd方法中对即将跳转的页面进行预加载，比如：

```
Web({ src: 'https://www.example.com/', controller: this.webviewController })
        .onPageEnd(() => {
          this.webviewController.prefetchPage('https://www.iana.org/help/example-domains');
        })
```


我们还可以使用prefetchResource()方法预获取web页面中的post请求，也就是加载用户可能会访问的资源，例如图片、脚本、样式表等，然后再onPageEnd()方法中使用clearPrefetchedResource()清除不再需要的缓存：

```
Web({ src: "https://www.example.com/", controller: this.webviewController })
        .onAppear(() => {
          webview.WebviewController.prefetchResource(
            {
              url: "https://www.example1.com/post?e=f&g=h",
              method: "POST",
              formData: "a=x&b=y",
            },
            [{
              headerKey: "c",
              headerValue: "z",
            },],
            "KeyX", 500);
        })
        .onPageEnd(() => {
          webview.WebviewController.clearPrefetchedResource(["KeyX",]);
        })
```


以上就是对web页面的一些性能优化。

幽蓝君正在参加第二期的鸿蒙学习资源贡献者活动，诚邀各位友友也一起参加。获奖者将会获得华为官方的定制证书和奖品，还有机会受邀参加华为的新品发布会和开发者大会，咱们说不定有机会松山湖见。欢迎有意向的友友入群报名。
