# 鸿蒙Next开发实战教程：对图片添加滤镜、调色、旋转等操作
出差足足俩月，环境恶劣，体重骤减，各种心酸，打工人苦啊。不过好在回来了，心情大好，今天写个小项目练练手。

上次写了个图片预览器，今天写一写对图片的一些处理，比如添加滤镜、调色、旋转等等。

滤镜
![image](https://github.com/user-attachments/assets/06cffdb3-1646-4c8c-b8a5-0353971645f0)

鸿蒙中的图片组件有自带的滤镜属性colorFilter，不过要设置这个属性并不容易。

```
Image($r('app.media.img'))
 .colorFilter(this.DrawingColorFilterFirst) //滤镜颜色矩阵
ColorFilter是一个4*5矩阵的颜色过滤器，像这样：

[ a,  b,  c,  d,  e,
  f,  g,  h,  i,  j,
  k,  l,  m,  n,  o,
  p,  q,  r,  s,  t ]
```
它的每一行分别代表R、G、B、A，前四列代表RGBA的偏向成分，第五列代表着颜色偏移量。它的计算公式是这样的：

```
R’ = a*R + b*G + c*B + d*A + e; 
G’ = f*R + g*G + h*B + i*A + j; 
B’ = k*R + l*G + m*B + n*A + o; 
A’ = p*R + q*G + r*B + s*A + t;
```
大家可以根据这个公式去调一些需要的颜色，我这里提供几个比较常用的滤镜矩阵：

```
//原图
  private Filter_0: number[] = [
    1,0,0,0,0,
    0,1,0,0,0,
    0,0,1,0,0,
    0,0,0,1,0
  ]
  //黑白
  private Filter_1: number[] = [
  0.213,0.715,0.072,0,0,
  0.213,0.715,0.072,0,0,
  0.213,0.715,0.072,0,0,
  0,0,0,1,0,
 ]
  //怀旧
  private Filter_2: number[] = [
  1/2,1/2,1/2,0,0,
  1/3,1/3,1/3,0,0,
  1/4,1/4,1/4,0,0,
  0,0,0,1,0
  ]
  //宝丽来
  private Filter_3: number[] = [
    1.438, -0.062, -0.062, 0, 0,
    -0.122, 1.378, -0.122, 0, 0,
    -0.016, -0.016, 1.483, 0, 0,
    -0.03, 0.05, -0.02, 1, 0
  ]

```

饱和度、对比度、高光

这几个属性设置起来比较简单，一个数字就能搞定，其实也没列举完，其他的还有灰度值、色相等等。

```
saturate 饱和度，原图为1
contrast 对比度，原图为1
brightness 高光，原图为1

Image($r('app.media.img'))
  .saturate(this.saturateValue) //饱和度
  .contrast(this.contrastValue) //对比度
  .brightness(this.brightnessValue)//高光
```
![image](https://github.com/user-attachments/assets/1cac74b8-7627-4339-abbe-50ac04a6da20)
![image](https://github.com/user-attachments/assets/cc0ee464-81ba-4518-b2e7-59f8241cbc65)
![image](https://github.com/user-attachments/assets/de0a9732-84bf-4bf4-91cd-b419d92e80aa)


旋转

旋转使用rotate中的angle属性，正数顺时针旋转，反之逆时针旋转。

```
Image($r('app.media.img'))
  .rotate({angle:this.angleValue})//角度
```

![image](https://github.com/user-attachments/assets/980f6a8d-9b29-4136-bee7-cec43206fc8b)


今天的内容就是这些
