## Uniapp Development Tutorial for HarmonyOS Applications - Custom Navigation Bar
​
I have been sharing articles on cross-platform development of HarmonyOS applications through Uniapp for several consecutive days. I believe everyone has gained a preliminary understanding of cross-platform development. Today, I would like to share the custom navigation bar in cross-platform development.


In the initialization project of Hbuilder, a navigation bar is built-in. This is a global navigation bar, and its style Settings and modifications are carried out in the global configuration file pages.json.
​

![img2](https://dl-harmonyos.51cto.com/images/202505/84ac40230b1ec6a7a756373b226fa7df52955f.png "img2")

Now open the pages.json file. In globalStyle, there are some properties related to the navigation bar. Let's try to modify them:

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

Then it was seen that the navigation bar had changed:

![img2](https://dl-harmonyos.51cto.com/images/202505/377f71724ece495e58c093880a2da59fb3e46d.png "img2")


Such modifications are very convenient and easy, but it seems that only basic attributes like color and text can be modified. Many times, we need to add some components to the navigation bar, such as buttons or search boxes.


For this situation, uniapp also provides a corresponding setting solution. It is still in the pages.json file. When we need to add components to the navigation bar of a certain page, we set the style under the corresponding path. There is a titleNView property in the style, which is used to add custom components to the navigation bar, like this:


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

However, Youlan Jun has personally tested that this setting method can be displayed normally when the browser is running, but it is ineffective in HarmonyOS:

![img2](https://dl-harmonyos.51cto.com/images/202505/9759c186778204966b32594543a66f6733dbbe.png "img2")


So in the development of HarmonyOS, we need to define the navigation bar ourselves.


Return to the pages.json file again. This time, set the navigationStyle to custom. The function is to cancel the native navigation bar:

`
"path": "pages/index/index",
"style": {
  "navigationStyle": "custom"
}
`

Then open the page where you need to customize the navigation bar. Here, I directly operate in index.vue on the home page. The implementation logic is relatively simple. Just add a component of the size of the navigation bar at the top of the page, and then add a search box in it. The relevant code is as follows:

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

Take a look at the running effect:

![img2](https://dl-harmonyos.51cto.com/images/202505/538855c2327fdb8ca0c792605ae034f0eba3b5.png "img2")

Today, let's take adding a search box as an example. If you need to customize other styles, the implementation method is similar.


The above is the custom navigation bar in the cross-platform development of HarmonyOS applications through uniapp. Thank you all for reading.
