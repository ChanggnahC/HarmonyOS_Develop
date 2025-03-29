# 鸿蒙Next开发实战教程-使用WebSocket实现即时聊天
鸿蒙系统提供了WebSocket库，使用它可以很方面的实现即时聊天功能，今天就使用WebSocket来实现一个完整的聊天功能。

首先创建一个WebSocket实例：

```
let ws = webSocket.createWebSocket()

```

然后创建WebSocket连接，我找到一个简单的ws地址，它直接返回我们发送的消息：

```
let url = 'ws://124.222.224.186:8800'
this.ws.connect(url,(err,value)=>{  
  if(!err){    
    console.log('链接成功');    
    this.isConnected = true 
   }else {    
     console.log('连接失败')  
   }
})
```


接下来订阅WebSocket的相关事件，首先订阅WebSocket的打开事件，发送消息等事件要在该事件后才可以使用：

```
this.ws.on('open', (err: BusinessError, value: Object) => {  
  console.log("on open, status:" + (value as OutValue).status + ", message:" + (value as OutValue).message);  
  // 当收到on('open')事件时，可以通过send()方法与服务器进行通信
});

```

然后订阅消息事件：

```
this.ws.on('message',(error: BusinessError, value: string | ArrayBuffer) => {  
   console.log("on message, message:" + value);  
   let content:string = value.toString()  
   const reg = /<.*?>/g  
   content = content.replace(reg,'')  
   let item:MessageClass = {    id:'1',    content:content  }  
   this.msgList.push(item)  
   this.listScroller.scrollToIndex(this.msgList.length,true)
});

```

由于返回的消息为富文本，我这里加了一个正则过滤html标签，然后把它们存到数组里便于展示。

当我们发送消息时，可以调用send方法：

```
this.ws.send(content, (err: BusinessError, value: boolean) => {  
  if (!err) {    
    console.log("send success");      
  } else {    
    console.log("send fail, err:" + JSON.stringify(err));  
  }
});
```
另外，当需要关闭连接时可以调用close方法：

```
this.ws.close()

```

以上就是一个即时聊天功能的实现过程.
