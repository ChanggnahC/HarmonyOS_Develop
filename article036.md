## Practical Tutorial on Developing HarmonyOS Shopping Projects with Uniapp: Implementing Home Page Carousel Images

In the past few days' articles, we have talked about how to create cross-platform projects, how to carry out basic layouts, how to implement custom navigation bars, etc. Through this series of article tutorials, we ultimately aim to achieve a shopping app. Today, we are going to work on the carousel section of the shopping app's home page.

uniapp provides the corresponding component swiper for carousel images, and it supports HarmonyOS applications, which is very convenient. Let's simply write a carousel with three pages of content. First, define the carousel and the style of each element:
    
```
.swiper{
width: 100%;
height: 140px;
}
.swiper-item{
width: 100%;
height: 100%;
display: block;
}   
``` 
Now add carousel images on the page:
```
<swiper class="swiper">
  <swiper-item>
    <view class="swiper-item" style="background-color: red;">1</view>
  </swiper-item>
  <swiper-item> 
    <view class="swiper-item" style="background-color: yellow;">2</view>
  </swiper-item>
  <swiper-item>
    <view class="swiper-item" style="background-color: green;">3</view>
  </swiper-item>
</swiper>  
```  

Take a look at the effect:
    
![img2](https://dl-harmonyos.51cto.com/images/202505/0525f3b1467d329ae1c642b515eec2fa9c410e.png "img2")

    
Now we try to add pictures to each page content. Here, we take a swiper-item as an example:

```
<swiper-item>
  <view class="swiper-item" >
    <image src="/static/banner1.jpg" style="width: 100%;height: 100%;"></image>
  </view>
</swiper-item>
```    

![img2](https://dl-harmonyos.51cto.com/images/202505/c1e184756b88a75f9be9732be2dd773cf10de0.png "img2")

    
Such a basic carousel image has taken shape. Then we can further enrich its content. Here are some commonly used attributes listed for you:

```
indicator-dots：Whether to display the panel indicator points
indicator-color： Indicator point color
indicator-active-color： The color of the currently selected indicator point
autoplay：Whether to switch automatically
interval：Automatically switch the time interval
```

Below are the correct usage methods of the above attributes and the complete code of the carousel:    
    
```
<swiper class="swiper" indicator-dots="true" indicator-color="#f8f8f8" indicator-active-color="#007aff" duration="3000" autoplay="true">
  <swiper-item>
    <view class="swiper-item" >
      <image src="/static/banner1.jpg" style="width: 100%;height: 100%;"></image>
    </view>
  </swiper-item>
  <swiper-item>
    <view class="swiper-item" >
      <image src="/static/banner2.jpg" style="width: 100%;height: 100%;"></image>
    </view>
  </swiper-item>
  <swiper-item>
    <view class="swiper-item" >
      <image src="/static/banner3.jpg" style="width: 100%;height: 100%;"></image>
    </view>
  </swiper-item>
</swiper>  
```

The above is the tutorial sharing about carousel images. Thank you for your reading.
