# 浅谈鸿蒙跨平台开发框架ArkUI-X
之前写过使用uniapp的跨平台开发鸿蒙项目，今天分享一下开发体验更友好的跨平台开发框架ArkUI-X。

ArkUI-X看起来像是鸿蒙官方的框架，在DevEco中就可以安装和使用，而且会ArkUI就可以开发安卓和、iOS和鸿蒙三个平台的app，下面简单介绍一下它的用法。

打开DevEco的Preference菜单，选择ArkUI-X，按照提示下载和安装SDK：
![image](https://github.com/user-attachments/assets/32e3db04-8459-410e-bd34-8a9881d38ef0)

然后重新打开DevEco，新建ArkUI-X项目，选择Empty Ability就可以：
![image](https://github.com/user-attachments/assets/c5d7e93e-05ee-4f50-9cb3-f735e52379b1)

等待初始化完成后，可以看到新项目中有个.arkui-x目录，里面分别有iOS和android的项目：
![image](https://github.com/user-attachments/assets/9d921662-b1fe-4d5f-afd5-7dc53de91313)

这两个项目是可以直接用Xcode或android studio打开运行的：
![image](https://github.com/user-attachments/assets/8e69b0cc-7c71-484e-82c7-2e7dd1b9cdf1)

新建的项目可能会遇到如下报错：
![image](https://github.com/user-attachments/assets/a92c06c5-95cb-4a6e-bce5-9dd7b0ccd4c6)


可以尝试按照官方文档给出的解决方案：

Windows环境变量设置方法： 
在此电脑 > 属性 > 高级系统设置 > 高级 > 环境变量中，新建系统变量。变量名为ANDROID_HOME，变量值为Android SDK安装目录。
环境变量配置完成后，关闭并重启DevEco Studio。


macOS环境变量设置方法：
1、打开终端工具，执行以下命令，打开.bash_profile文件。
vi ~/.bash_profile
2、单击字母“i”，进入Insert模式。
3、输入以下内容，配置Android SDK安装目录。
export ANDROID_HOME=/Users/xxx/Library/Android/sdk
4、编辑完成后，单击Esc键，退出编辑模式，然后输入“:wq”，单击Enter键保存。
5、执行以下命令，使配置的环境变量生效。
source ~/.bash_profile
6、环境变量配置完成后，关闭并重启DevEco Studio。
这个框架我也没有太深入使用，只是感觉比uniapp的方式更加方便，所以分享给大家。
