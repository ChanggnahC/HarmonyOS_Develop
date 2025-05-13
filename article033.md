## Harmonyos Cross-platform Development Tutorial - Basic Layout of Uniapp


The content of the article two days ago provided some detailed introductions on developing HarmonyOS applications through uniapp, including the configuration of the development environment and the interpretation of the project structure directory. Today, we officially start writing code.


Getting started with a new development language often begins with "Hello World". A simple demo has already been written in the initialization project of Uniapp, so it won't be elaborated here. Let's start directly from the layout.


The layout of Uniapp is different from that of the native language ArkTs of HarmonyOS, but it is quite similar.


Youlan Jun previously summarized that there are no more than three layout methods: horizontal, vertical, and layering. All other layout methods are derived from these three, and Uniapp is no exception.


In ArkTs, there are several basic layout container components such as Row(), Column(), Stack(), and Flex(). More complex ones include List(), Grid(), Scroll(), and so on.


In Uniapp, the basic layout method is usually implemented directly using the view container. For example, if I want to implement a horizontal layout, I will use the view container and set the layout mode to row in the style of the view:


` 
<view style="display: flex;flex-direction: row;" >
  <view style="width: 100px;height: 100px;background-color: aqua;">组件1</view>
  <view style="width: 100px;height: 100px;background-color:bisque;">组件2</view>
</view>
`

![img2](https://dl-harmonyos.51cto.com/images/202505/c5a88a45276f022c72549053b4500b0dee2f99.png "img2")
    
When it comes to vertical layout, you only need to set the layout direction to column:

`
<view style="display: flex;flex-direction: column;" >
  <view style="width: 100px;height: 100px;background-color: aqua;">组件1</view>
  <view style="width: 100px;height: 100px;background-color:bisque;">组件2</view>
</view>    
`

![img2](https://dl-harmonyos.51cto.com/images/202505/e4be6b7508fc722c436674b509ee040ff44d2b.png "img2")

The more challenging part comes next. For the stacked layout, ArkTs directly provides the Stack() container, and there is a corresponding alignment method that can be set directly, which is relatively simple. However, uniapp does not provide this alignment method. The cascading layout cannot be directly set in flex-direction.


We can achieve it by using the postion attribute. The function of postion is to set the positioning method, including static, relative, fixed, and absolute centralized methods. Today, we are going to talk about absolute.


"absolute" is an absolute positioning method, which is an absolute positioning method that is detached from the document flow and relative to the parent element.


A more detailed explanation is that no matter how many components of the same level it has, it does not affect its positioning with the upper left corner of the parent element as the origin. Similarly, it does not affect others either. It is equivalent to floating on the upper layer and using offset to control the position. For example, the following code:

` 
<view  style="display: flex;flex-direction: column;position: relative;" >
<view style="width: 50px;height: 50px;background-color:bisque;">组件1</view>
<view style="width: 50px;height: 50px;background-color:blue;">组件2</view>
<view style="width: 50px;height: 50px;background-color:brown;">组件3</view>
<view style="width: 100px;height: 100px;background-color: aqua;position: absolute;opacity: 0.5;align-items: center;">组件4</view>
</view>    
`  

![img2](https://dl-harmonyos.51cto.com/images/202505/c2f5285706407090169764b6684dbda3a7bc9a.png "img2")


So if both containers that require a stacked layout are positioned using absolute and the alignment is set using top, left, bottom, and right, the same function as Stack() in HarmonyOS is achieved:

`
<view class="content" style="display: flex;flex-direction: column;position: relative;" >
<view style="width: 100px;height: 100px;background-color: aqua;position: absolute;top: 0;">组件1</view>
<view style="width: 50px;height: 50px;background-color:bisque;position: absolute;z-index: 10;top: 0;">组件2</view>
</view>    
`

![img2](https://dl-harmonyos.51cto.com/images/202505/444a7d437a1c5c0db31874a19b99a0d104f557.png "img2")

Here, z-index can be used to set who is in the upper layer. Additionally, the parent container with absolute positioning needs to set the position: relative attribute; otherwise, the child components cannot find the target.


The above is the basic layout method for developing HarmonyOS with Uniapp. Thank you for your reading.
