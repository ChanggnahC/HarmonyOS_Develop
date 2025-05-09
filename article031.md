## uniapp跨平台开发HarmonyOS NEXT应用初体验
之前写过使用uniapp开发鸿蒙应用的教程，简单介绍了如何配置开发环境和运行项目。那时候的HbuilderX还是4.22版本，小一年过去了HbuilderX的正式版本已经来到4.64，历经了多个版本的更新后，跨平台开发鸿蒙应用的体验大幅提升。今天再次跟大家分享一下使用uniapp开发鸿蒙的使用教程。

这次的开发体验最大的感觉是非常的方便快捷，你甚至不需要打开DevEco studio，直接使用Hbuilder就可以连接真机进行调试，还有自动的实时编译更新，非常友好。

首先，你需要去AppGallery Connect创建一个新的应用，填写应用名称和包名之后创建应用，包名我们一会需要用到。贴一下AppGallery Connect地址：
`
https://developer.huawei.com/consumer/cn/service/josp/agc/index.html#/
`

![img1](https://dl-harmonyos.51cto.com/images/202505/a474d9309bc464d324c510adb65f7ec3eb446e.png "img1")

接下来下载安装最新版本的HbuilderX，目前是4.64版本，下载地址：
`
https://www.dcloud.io/hbuilderx.html
`
打开HbuilderX新建项目，Vue版本选择3.0

![img2](https://dl-harmonyos.51cto.com/images/202505/c55d87393082a8dd794458951bb376440a4222.png "img2")

创建项目之后，电脑连接真机，或者打开DevEco studio的模拟器也可以。然后再HbuilderX的运行菜单下找到运行到鸿蒙，第一次可能会有下载真机插件的步骤，还有配置调试证书的弹窗，真机插件是自动下载的，配置证书需要填写第一步创建应用时的包名，然后我推荐大家使用自动

申请调试证书，信息自动填写完成后点击保存。

再次运行，会弹出检测到的设备列表：

![img3](https://dl-harmonyos.51cto.com/images/202505/7334e3a90145a491fb8035559ad76d7da29e11.png "img3")

选择设备就可以将应用运行到设备上：

![img4](https://dl-harmonyos.51cto.com/images/202505/754e3042678b2292a008893fcdefa8d82bf6ba.png "img4")

这时候尝试修改一些内容，程序会自动编译更新，非常方便。

整个的开发过程甚至比原生开发更加友好，鸿蒙的跨平台开发体验又提升了一大步，相信以后会有越来越多的开发者加入跨平台开发的阵容。这也符合幽蓝君之前的预测，在三个移动操作系统面前，跨平台会开发是很多中小企业的第一选择，而unipp又是跨平台开发框架中的比较突出的佼佼者，开发体验卓越。

以上就是最新的uniapp开发鸿蒙应用的使用体验，以后也会持续深入的跟大家分享使用uniapp跨平台开发鸿蒙应用的教程，也打算把跨平台开发这部分做成一个合集，方便大家持续学习，欢迎大家关注。
