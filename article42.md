# 在鸿蒙NEXT中开发一个2048小游戏
本项目是基于api12开发的2048游戏，游戏的逻辑是当用户向某个方向滑动时，将该方向相邻且相等的数字相加，同时在空白区域的随机位置生成一个随机数字。游戏中的数字越大，分数越高。

![image](https://github.com/user-attachments/assets/15cea5bd-5bc1-46f0-8e35-f11165318662)


首先，游戏的界面布局分别采用两个网格组件Grid来实现，难点在于上方的菜单栏是不均等的三种尺寸的组件，可以使用Grid的rowsTemplate和columnsTemplate属性来实现：
```
Grid(){
            GridItem(){
              Column(){
                Text('2048')
                  .fontColor('rgb(249,249,249)')
                  .fontSize(35)
                  .fontWeight(FontWeight.Bold)
                Text('4*4')
                  .fontColor('rgb(249,249,249)')
                  .fontSize(22)
                  .fontWeight(FontWeight.Bold)
              }
            }
            .backgroundColor('rgb(239,202,26)')
            .borderRadius(10)
            .rowStart(1)
            .rowEnd(2)

            GridItem(){
              Column(){
                Text('分数')
                  .fontColor('rgb(232,232,232)')
                  .fontSize(15)
                  .fontWeight(FontWeight.Bold)
                Text(this.currentScore.toString())
                  .fontColor('rgb(249,249,249)')
                  .fontSize(25)
                  .fontWeight(FontWeight.Bold)
                  .margin({top:3})

              }
            }
            .backgroundColor('rgb(188,172,162)')
            .borderRadius(10)
            GridItem(){
              Column(){
                Text('最高分')
                  .fontColor('rgb(232,232,232)')
                  .fontSize(15)
                  .fontWeight(FontWeight.Bold)
                Text(this.highestScore.toString())
                  .fontColor('rgb(249,249,249)')
                  .fontSize(25)
                  .fontWeight(FontWeight.Bold)
                  .margin({top:3})
              }
            }
            .backgroundColor('rgb(188,172,162)')
            .borderRadius(10)


            GridItem(){
              Text('重新开始')
                .fontColor('rgb(249,249,249)')
                .fontSize(20)
                .fontWeight(FontWeight.Bold)
            }
            .backgroundColor('rgb(254,156,62)')
            .borderRadius(10)
            .onClick(()=>{
              this.startGame()
            })


            GridItem(){
              Text('返回菜单')
                .fontColor('rgb(249,249,249)')
                .fontSize(20)
                .fontWeight(FontWeight.Bold)
            }
            .backgroundColor('rgb(254,156,62)')
            .borderRadius(10)


          }
          .columnsTemplate('1fr 1fr 1fr')
          .rowsTemplate('2fr 1fr 1fr')
          .rowsGap(8)
          .columnsGap(8)
          .width('100%')
          .height(170)
```


在游戏逻辑的实现中，关键在于检测游戏中的空白格和相邻数字的相加操作，以及将它们放置到数组中对应的位置。

比如此时游戏中有4个数字：

```
//0代表空白
let arr:number[] = [0,0,2,2,0,0,4,0,0,8,0,0,0,0,0,0]
以向左滑动为例,将所有数字向左滑动，并将相同的数字相加：

let newArr:number[] = Array(4).fill(0)
    let arrWithoutZero = arr.filter((x) => x !== 0) //去掉所有的0
    if (arrWithoutZero.length == 0) {
      return newArr
    }
    if (toHead) {
      for (let i = 0; i < arrWithoutZero.length; i++) {
        newArr[i] = arrWithoutZero[i]
      }
      for (let i = 0; i < newArr.length - 1; i++) {
        if (newArr[3 - i] === newArr[2 - i] && newArr[3 - i] !== 0) {
          newArr[3 - i] = 0
          newArr[2 - i] *= 2

          this.score = this.score + newArr[2 - i]
          console.log("得分:",this.score)

        }
      }
    }
```


随后，在随机的空白位置添加一个数字：

```
let cells:number[] = []
    for (let i = 0; i < 4; i++) {
      for (let j = 0; j < 4; j++) {
        if (!this.itemList[i][j]) {
          cells.push(i * 4 + j)
        }
      }
    }
let row = Math.floor(cellIndex / 4)
let col = cellIndex % 4
this.itemList[row][col] = this.getRandomValue()
```


这就是游戏的基本逻辑，比较简单，适合初学者学习。

