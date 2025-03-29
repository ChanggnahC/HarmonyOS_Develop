# HarmonyOS Next开发教程之地图定位
今天分享一下在鸿蒙开发中的地图定位问题，也就是在地图中如何定位自己所在的位置。

关于如何加载显示地图在之前的文章已经详细介绍过，有问题的友友可以点击查看：

HarmonyOS NEXT实战教程-实现Keep运动轨迹

将地图定位到自己所在的位置，有几种方法：

一种是在初始化地图前先获取到当前的坐标，然后将坐标初始化到地图上

```
geoLocationManager.getCurrentLocation(request).then((result) => {
  console.log('current location: ' + JSON.stringify(result));
this.mapOptions = {
  position: {
    target: {
       latitude: result.latitude,
       longitude: result.longitude
     
    },
    zoom: 14.2
  },
  minZoom:3,
  maxZoom:18,
  scaleControlsEnabled: true
};
})
  .catch((error:BusinessError) => {
    console.error('promise, getCurrentLocation: error=' + JSON.stringify(error));
  });
```


另一种方法是使用moveCamera方法移动相机来实现：

```
// 新建CameraUpdate对象
let cameraUpdate: map.CameraUpdate = map.newCameraPosition(cameraPosition);
// 移动相机
this.mapController.moveCamera(cameraUpdate);
```


然后可以使用setMyLocation方法标记当前的位置坐标：

```
this.mapController?.setMyLocation(location)
this.mapController?.setMyLocationEnabled(true)
```
![image](https://github.com/user-attachments/assets/1ea29df7-7700-4eb7-840f-176afc20690c)

这里有一个困扰了我好一阵的问题，这里的位置标记点没有问题，定位也是用的同一个坐标，为什么我的位置不在屏幕中央？？？

其实在初始化地图和移动镜头时不能直接传入获取到的位置坐标，而是要进行坐标系的转换，由WGS84坐标系转换到GCJ02坐标系：
```
let gcj02Posion: mapCommon.LatLng =  await map.convertCoordinate(mapCommon.CoordinateType.WGS84, mapCommon.CoordinateType.GCJ02,    point);

```
这样的话当前的位置就会出现在屏幕中间啦。
