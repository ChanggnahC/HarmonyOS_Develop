# 鸿蒙开发中console.log和hilog的区别
在日常开发中打印日志是调试程序非常常用的操作，在鸿蒙的官方文档中介绍了hilog这种方式，有些前端转过来的友友发现console.log也可以进行日志打印。有一段时候幽蓝君也非常喜欢使用console.log，因为它看起来好像更加简单方便。

那么今天幽蓝君就来和大家说一说console.log和hilog有什么区别。

首先要说的是他们都有info、debug、warn、error等几个打印日志的方法，代表不同的日志级别。

先来分别看一下他们打印相同内容时的区别：

```
hilog.debug(0x0001, "testTag", "hello world");
console.debug('hello world');
```
![image](https://github.com/user-attachments/assets/084ba348-866a-4cca-9ce6-7fd2188fcbd1)

从后半部分来看他们俩好像没有什么区别，但前半部分略有不同。hilog打印的内容是A00001/testTag，console打印的前半部分是A03d00/JSAPP。

这里要跟大家hilog的四个部分：日志级别、日志领域、日志标识和日志内容，前半部分这两个东西分别是日志领域和日志标识。A00001/testTag在上面的代码可以找到对应的内容，代表我们是可以对它进行自定义的。而在console中这一部分是默认的，我们可以认为console就是对hilog的封装，需要我们自定义的内容少了，所以它用起来更加简单。

凡事都有两面性，console在简单的同时也降低了灵活性，我们无法自定义日志的业务域和标识，所以有时候无法对代码进行定位。

以上就是这两种方式的优缺点，要是问幽蓝君更推荐哪一种，当然还是hilog，在大型项目中对日志进行统一的管理是很有必要的，而且这也是官方文档比较推荐的方式。
