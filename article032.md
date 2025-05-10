## uniapp开发HarmonyOS NEXT应用之项目结构详细解读

昨天的文章介绍了使用uniapp跨平台鸿蒙应用时如何配置开发环境和运行调试项目,今天介绍一下uniapp项目目录的结构。
可能对于从事移动开发的友友来说，uniapp的项目结构看起来有一些陌生，它更接近于前端项目，新建的uniapp项目结构是这样的：

![img2](https://dl-harmonyos.51cto.com/images/202505/b69b09521132097f1b3315500809bbe72b2665.png "img2")

上面的两个文件夹pages中存放的是项目的页面，static则是存放静态资源的文件。

打开pages文件夹，可以看到项目初始化的index文件，这就是项目的初始化页面，里面有一个简单的Hello World项目代码，运行项目的时候可以看到。

![img2](https://dl-harmonyos.51cto.com/images/202505/7186cbb21e778dbc6c80097972a8bb037d15cf.png "img2")

继续往下面看，App.vue文件打开可以看到它里面存放了项目的生命周期函数，比如项目启动、项目退出等等。

![img2](https://dl-harmonyos.51cto.com/images/202505/01e649b44e0ea2f0de7421ce7e7c84d7e65642.png "img2")

index.html相当于项目的根页面，它虽然也属于页面，但是在uniapp中通常是不需要大家修改的，因为可以看到里面有一个id是app的容器，我们在项目中写的所有代码最终都会注入到这个容器中。

![img2](https://dl-harmonyos.51cto.com/images/202505/c77b0ae217aed397f1d44680360b861f0ddb44.png "img2")

Main.js是整个项目的核心入口文件，负责初始化Vue实例和全局配置。

Manifest.json中存放了项目的各种配置文件，项目的名称、版本、描述、各个平台的的运行配置都在这里。

Pages.json中存放了项目中所有的页面路径和全局样式，尝试在globalStyle中的样式做一些修改，项目的全局样式就会被改变。而当新建了一个页面时，也会在这里自动注册。

![img2](https://dl-harmonyos.51cto.com/images/202505/66e0b0e296d0de2ce93653410f6a3df90a3c5f.png "img2")

最后的uni.scss文件，打开可以看到它已经给出了解释，这里是uni-app内置的常用样式变量。

以上就是uniapp项目初始化后的项目结构，还没结束，当我们运行项目后，还会新增unpackage目录，打开可以看一下是不是很熟悉，编译器把uniapp项目编译成了鸿蒙项目，这就是跨平台开发的运行原理。

![img2](https://dl-harmonyos.51cto.com/images/202505/84e317b2129568afd1430705e2880c526539b3.png "img2")

以上就是对uniapp项目结构的详细解读。经历了两天的项目环境搭建和介绍，接下来的教程我们会进入到正式的跨平台开发编码环节，欢迎您持续关注。
