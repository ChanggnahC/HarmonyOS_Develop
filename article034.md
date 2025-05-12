## Uniapp开发鸿蒙应用教程之自定义导航栏
​
连续分享了几天的Uniapp跨平台开发鸿蒙应用教程的文章，相信大家对跨平台开发已经有了初步的了解，今天分享一下跨平台开发中的自定义导航栏。

在Hbuilder的初始化项目中是自带了导航栏的，这是一个全局的导航栏，它的样式设置和修改是在全局的配置文件pages.json中进行。
​

![img2](https://dl-harmonyos.51cto.com/images/202505/84ac40230b1ec6a7a756373b226fa7df52955f.png "img2")

现在打开pages.json文件，在globalStyle中有一些关于导航栏的属性，我们尝试修改一下：

`
{
"pages": [ //pages数组中第一项表示应用启动页，参考：https://uniapp.dcloud.io/collocation/pages
		{
"path": "pages/index/index",
"style": {
    "navigationBarTitleText": "uni-app"
			}
		}

	],
"globalStyle": {
"navigationBarTextStyle": "black",
"navigationBarTitleText": "自定义导航栏",
"navigationBarBackgroundColor": "#F8F800",
"backgroundColor": "#F8F8F8"
	},
"uniIdRouter": {}
}
`

然后看到导航栏已经发生了改变：

![img2](https://dl-harmonyos.51cto.com/images/202505/377f71724ece495e58c093880a2da59fb3e46d.png "img2")


这样的修改很方便很便捷，但是好像只有颜色和文字这些基础的属性可以修改，很多时候我们需要在导航栏上添加一些组件，比如按钮或者搜索框。

对于这种情况，uniapp也提供了相应的设置方案，还是在pages.json文件中，当我们需要为某一个页面的导航栏添加组件，就在对应的path路径下设置style，style中有个titleNView属性就是为导航栏添加自定义组件，像这样：

`
"path": "pages/index/index",
"style": {
   "navigationBarBackgroundColor": "#00c170",
   "app-harmony": {
       "softinputMode": "adjustPan"
   },
   "app-plus": {
"titleNView": {
	   "buttons": [{
"text": "搜索",
"fontSize": "16",
	        "float": "right",
	        "color": "#fff"
	    }],
"searchInput": {
"align": "center",
"placeholder": "请输入信息",
"backgroundColor": "#fff",
"placeholderColor": "#000"
}
    }
  }
}
`

但是幽蓝君亲测这种设置方式在浏览器运行时可以正常显示，在鸿蒙中是无效的：

![img2](https://dl-harmonyos.51cto.com/images/202505/9759c186778204966b32594543a66f6733dbbe.png "img2")



所以在鸿蒙开发中我们需要自己定义导航栏。

再次回到pages.json文件，这次将navigationStyle设置成custom，作用是取消原生的导航栏：

`
"path": "pages/index/index",
"style": {
  "navigationStyle": "custom"
}
`

然后打开需要自定义导航栏的页面，我这里就直接在首页index.vue中操作，实现逻辑比较简单，就是在页面顶部添加一个导航栏大小的组件，然后在其中添加搜索框，相关代码如下：

`
<view class="custom-nav-bar">
  <input
style="width: calc(100% - 40px);background-color: white;height: 35px;padding: 0px 10px;border-radius: 18px;"
placeholder="请输入搜索内容" placeholder-style="#000" confirm-type="search" @input="navSearchAction" />
</view>
`

`
.custom-nav-bar {
display: flex;
justify-content: space-between;
align-items: center;
height: 54px;
/* 根据需要调整高度 */
background-color: #f8f8f8;
/* 导航栏背景色 */
/* 其他样式属性 */
padding: 30px 12px 0px 12px;
align-items: center;
justify-content: center;
}
`

看一下运行效果：

![img2](https://dl-harmonyos.51cto.com/images/202505/538855c2327fdb8ca0c792605ae034f0eba3b5.png "img2")

今天就以添加一个搜索框为例，大家如果需要自定义其他的样式也是类似的实现方式。

以上就是uniapp跨平台开发鸿蒙应用中的自定义导航栏，感谢大家的阅读。
