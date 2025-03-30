# HarmonyOS NEXT开发实战案例--圆盘
这是之前写过的一个项目，后来删掉了，现在适配到api12重新发布，友友们按需查阅。

本文主要通过抽奖转盘小项目讲解在鸿蒙开发中如何使用画布组件Canvas绘制图形和文字，以及转圈动画的实现。效果图如下：
![image](https://github.com/user-attachments/assets/effebc63-397c-4c8e-b249-8d1ed8916c93)

首先绘制转盘的六个分区：

```
drawInnerArc() {
    let colors = [
      'rgb(61,127,255)','rgb(121,189,255)'
    ];
    let radius = this.screenWidth * 0.336;
    for (let i = 0; i < 6; i++) {
      if (this.canvasContext !== undefined) {
        this.canvasContext.beginPath();
        this.canvasContext.fillStyle = colors[i%2];
        this.canvasContext.arc(0, 0, radius,
          this.startAngle * Math.PI / 180,  (this.startAngle + this.avgAngle) * Math.PI / 180);
        this.canvasContext.fill();
      }
      this.canvasContext?.lineTo(0, 0);
      this.canvasContext?.fill();
      this.startAngle += this.avgAngle;
    }
  }
```
在界面中添加画布组件，将转盘区域绘制出来：

```
Canvas(this.canvasContext)
        .width('100%')
        .height('100%')
        .onReady(() => {
          this.drawModel.draw(this.canvasContext, this.screenWidth, this.screenHeight);
        })
```
![image](https://github.com/user-attachments/assets/217088cf-ce95-4455-9a2f-62e5109ddfa9)


然后在转盘的分区上分别绘制文字，注意，这里的文字使用Text组件并不容易实现，因为要进行角度的调整，使每个文字都朝向圆心的位置：

```
let textArrays = [
      "玉米","番茄","西瓜","苹果","樱桃","菠萝"
    ];

    let arcTextStartAngle = 34;
    let arcTextEndAngle = 26;
    for (let i = 0; i < 6; i++) {
      
      let startAngle = (this.startAngle + arcTextStartAngle) * Math.PI / 180
      let endAngle = (this.startAngle + arcTextEndAngle) * Math.PI / 180

      class CircleText {
        x: number = 0;
        y: number = 0;
        radius: number = 0;
      }
      let circleText: CircleText = {
        x: 0,
        y: 0,
        radius: this.screenWidth * 0.336
      };
      // The radius of the circle.
      let radius = circleText.radius - circleText.radius / 6;
      // The radians occupied by each letter.
      let angleDecrement = (startAngle - endAngle) / (textArrays[i].length - 1);
      let angle = startAngle;
      let index = 0;
      let character: string;

      while (index < textArrays[i].length) {
        character = textArrays[i].charAt(index);
        this.canvasContext?.save();
        this.canvasContext?.beginPath();
        this.canvasContext?.translate(circleText.x + Math.cos(angle) * radius,
          circleText.y - Math.sin(angle) * radius);
        this.canvasContext?.rotate(Math.PI / 2 - angle);
        this.canvasContext?.fillText(character, 0, 0);
        angle -= angleDecrement;
        index++;
        this.canvasContext?.restore();
      }
      
      this.startAngle += this.avgAngle;
    }
```

![image](https://github.com/user-attachments/assets/57b158fc-4941-4842-a5be-19791385c57c)

此时一个基本的转盘已经完成了，可以在转盘外增加一个大转盘作为修饰：

```
drawOutCircle() {
    // Draw outer disc.
    this.fillArc(new FillArcData(0, 0, this.screenWidth * 0.4, 0,
      Math.PI * 2),  'rgb(102,177,255)');

    let beginAngle = this.startAngle;
    // Draw small circle.
    for (let i = 0; i < 8; i++) {
      this.canvasContext?.save();
      this.canvasContext?.rotate(beginAngle * Math.PI / 180);
      
      if (this.canvasContext !== undefined) {
        this.canvasContext.beginPath();
        this.canvasContext.fillStyle = '#FFFFFF';
        this.canvasContext.arc(this.screenWidth * 0.378, 0, 4.1,
          0, 360);
        this.canvasContext.fill();
      }
      
      beginAngle = beginAngle + 360 / 8;
      this.canvasContext?.restore();
    }
  }
```

![image](https://github.com/user-attachments/assets/b7ed245e-9225-4708-93f4-b51f54580d9c)


使用叠加布局为转盘添加指针：

```
Stack({ alignContent: Alignment.Center }) {
      Canvas(this.canvasContext)
        .width('100%')
        .height('100%')
        .onReady(() => {
          this.drawModel.draw(this.canvasContext, this.screenWidth, this.screenHeight);
        })
        .rotate({
          x: 0,
          y: 0,
          z: 1,
          angle: this.rotateDegree,
          centerX: this.screenWidth / 2,
          centerY: this.screenHeight / 2
        })

      Image($r('app.media.btn_start'))
        .width('19.3%')
        .height('10.6%')
        .enabled(this.enableFlag)
       
    }
```
![image](https://github.com/user-attachments/assets/46d90201-6af0-43d5-8185-c3c502b687ca)

好了，界面大功告成。接下来是动画部分，我们可以给指针添加一个点击事件，为转盘添加旋转动画：

```
startAnimator() {
    let randomAngle = Math.round(Math.random() * 360);

    animateTo({
      duration: 4000,
      curve: Curve.Ease,
      delay: 0,
      iterations: 1,
      playMode: PlayMode.Normal,
      onFinish: () => {
        this.rotateDegree = 270 - randomAngle;
      }
    }, () => {
      this.rotateDegree = 360 * 5 +
      270 - randomAngle;
    })
  }

Image($r('app.media.btn_start'))
        .width('19.3%')
        .height('10.6%')
        .enabled(this.enableFlag)
        .onClick(() => {
          this.enableFlag = !this.enableFlag;
          this.startAnimator();
        })
```


以上就是整个转盘的开发过程
