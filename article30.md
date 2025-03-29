# 鸿蒙NEXT实战教程—实现音乐歌词同步滚动
之前写过一个音乐播放器项目，今天再给它完善一下，加一个歌词同步滚动。

先看效果图：
![image](https://github.com/user-attachments/assets/ff66142d-2506-4d3a-ba92-838406fe9119)

要做歌词同步滚动，我们首先需要的文件资源就是音乐文件和与之匹配的歌词文件。现在歌词文件不太好找，没关系，我们可以自己做。先来看本项目的歌词示例，文件格式是.lrc

[00:20.5]   
[00:20.5]I walked through the door with you
[00:23.63]曾与你一起穿过那扇门
[00:23.63]The air was cold
[00:25.83]空气充满寒意
[00:25.83]But something 'bout it felt like home somehow and I
[00:31.41]不知为何却让我觉得温暖得像家一般
[00:31.41]Left my scarf there at your sister's house
[00:35.22]我把自己的围巾落在了你姐姐家


内容中除了要有歌词文本之外，还要有每一行歌词出现的时间点，大家按照这种格式创建就可以，就是一行一行写比较麻烦。

准备好之后把音乐和歌词放入rawfile文件夹中，播放音乐就不说了，我们今天只说歌词部分。

首先读取一下本地文件：

```
this.context .resourceManager.getRawFileContent('AllTooWell.lrc')  .then((value: Uint8Array) => {      
  let textDecoder = util.TextDecoder.create('utf-8', { ignoreBOM: true });      
    let stringData = textDecoder.decodeWithStream(value, { stream: false });      
    this.mLrcEntryList = parseLrcLyric(stringData);  
  })

```

然后后要把歌词和时间分离出来分别存储，方便进行滚动展示，这里使用正则表达式来提取时间部分：

```
const lrcLineRegex: RegExp = new RegExp('\\[\\d{2,}:\\d{2}((\\.|:)\\d{2,})\\]', 'g');
for (let i = 0; i < lyric.length; i++) {  
    let lineTime = lyric[i].match(lrcLineRegex);  
     let lineText = lyric[i].replace(lrcLineRegex, '');  
     if (lineTime && lineText) {    
       for (let j = 0; j < lineTime.length; j++) {      
         let min = Number(String(lineTime[j].match(lrcTimeRegex1)).slice(1));      
         let sec = Number.parseFloat(String(lineTime[j].match(lrcTimeRegex2)));      
         let timeInSeconds = (min * 60 + sec) * 1000;      
         lrc.push({ lineStartTime: timeInSeconds, lineDuration: 0, lineWords: lineText, words: [] });    
       }  
   }}

```

这样就得到了一个由时间和内容组成的数组。

接下来将歌词逐行展示，这里使用的是绘制画布的方式：

```
for (let i = 0; i < lyric.words.length; i++) {
  let wordStartTime = lyric.lineStartTime + lyric.words[i].wordStartTime;
  let wordEndTime = lyric.lineStartTime + lyric.words[i].wordStartTime + lyric.words[i].duration;
  if (wordStartTime <= this.lyricMilliSecondsTime && wordEndTime >= this.lyricMilliSecondsTime) { 
   let wordProgress =      (this.lyricMilliSecondsTime - wordStartTime) / lyric.words[i].duration / lyric.words.length;    let wordPassedProgress = i / lyric.words.length;    let progress = wordPassedProgress + wordProgress;    this.context.fillStyle = this.progressGrad(startX, gradY, endX, gradY, progress);
    this.context.font =      this.fontWeight + ' ' + (this.mCurrentTextSize + this.TEXT_ADD_SIZE) + 'vp ' + this.fontFamily;    this.context.fillText(lyric.words[i].text, wordX, this.lrcY + this.TEXT_ADD_SIZE / 2, this.lrcWidth);
  } else {

    this.context.font = this.fontWeight + ' ' + this.mCurrentTextSize + 'vp ' + this.fontFamily;
    this.context.fillText(lyric.words[i].text, wordX, this.lrcY, this.lrcWidth); 
 }
  wordX += this.context.measureText(lyric.words[i].text).width;}


```
下面就是歌词同步滚动的问题，在播放音乐的时候肯定会有一个计时器，歌词滚动同样使用这个计时器，计时器计数改变时，找到对应的歌词，并计算该行歌词的行数和偏移量，进行画布滚动，就可以实现歌词的同步滚动了，我们还可以对歌词的透明度进行绘制，实现渐隐渐出的效果。

由于众所周知的原因，本项目就和大家分享一个大概的思路，有了思路这个功能就不是很难了，感谢您的阅读。
