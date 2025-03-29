# 关于HarmonyOS NEXT中的模块化开发
今天不写页面和动画，斗胆给大家讲一讲软件工程。

软件工程讲究高内聚低耦合，意思就是把整个工程按照分工不同分成不同的模块，每一个模块紧密联系又互不影响。就像一座摩天大楼，它里面的电路网非常庞大和复杂，它们每一层楼每一个房间都能独立运行且互不影响，所有的房间电路都通过一个总开关控制。

软件工程亦是如此，让每一个模块职责明确，易于管理，可以提高代码的可维护性、可扩展性。

那么在鸿蒙中怎么实现模块化的开发，今天幽蓝君跟大家分享一下。

DevEco创建的项目一开始是没有进行模块化的，我们通常会在entry目录下的pages中进行项目的开发。
![image](https://github.com/user-attachments/assets/65ad5d60-c41f-4611-9648-eafcb656f99a)

如果这个工程比较庞大，我们就可以尝试使用模块化的开发，比如把项目的首页、个人中心等模块各自分开。
![image](https://github.com/user-attachments/assets/43ea79d0-38f4-455f-8ac9-b85ceb700e21)

现在尝试在工程目录下新建一个features文件夹，然后在features文件夹右键，新建Module，这里给了四个选项，第一个选项会创建一个可以独立编译的项目，这通常用于一个项目的不同版本，我们今天要选择的是最后一个Static Library，命名注意要小写,完成之后可以看到模块目录下有一些资源和配置文件。
![image](https://github.com/user-attachments/assets/db97c7dd-4766-416b-b205-88c1612e3511)

这样我们就把工程中不同的模块分离开来，功能明确，互不打扰，打包的时候这些模块也会被单独打包为本地依赖库，非常完美。

那么，外部怎么引入这些模块呢。

首先注意每一个模块根目录下的Index文件，需要被外界引用的页面要先在这里导出。
![image](https://github.com/user-attachments/assets/d0a9a1ac-4d1c-426d-9019-af691cfc571f)

导出之后，这些组件还是不能被外部直接引用的。要先在oh-package.json5中添加依赖，举个例子：

```
"dependencies": {  
  'home': 'file:../features/home'
}

```

这个写法非常有讲究，前面是名字，这个名字要和组件目录中oh-package.json5文件里的name一致，后面是以file开头的组件目录，然后就可以在页面中进行引用了。

```
import {MainPage} from 'home/Index'

```

以上就是鸿蒙模块化开发的基本配置，感谢您的阅读。
