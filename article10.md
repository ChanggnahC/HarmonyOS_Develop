# 鸿蒙Next开发入门教程--断点调试
今天跟大家介绍DevEco中重要的功能-断点调试。断点调试最好使用真机，本地模拟器可以试一下， Previewer是不行的。

首先点击Run->Edit Configurations打开如下的菜单界面，并在Debuger type中选择Detect Automatically。
![image](https://github.com/user-attachments/assets/537863c5-2d90-47ae-bc36-ccf3d4de4310)


然后点击这右上角这两个绿色的小虫子，任选一个，运行程序。
![image](https://github.com/user-attachments/assets/c0a0e0dc-7fa5-45bc-a460-84c9bad2a42e)


在代码行数后单击，会出现红点，说明断点设置成功了，项目运行到此处会停下来。

![image](https://github.com/user-attachments/assets/f69d62ee-261d-472c-889d-2235c43f4129)

停下来之后，把光标放到变量上会显示该变量的值。
![image](https://github.com/user-attachments/assets/61230ab8-7885-4eb9-ab97-de1149e65eb5)


在左下角还有一排按钮，也是用来用了调试程序的。
![image](https://github.com/user-attachments/assets/d6db8949-9b90-4246-8830-68fd6909a820)


其中绿色的按钮1是当代码执行到断点停止后，单击它继续执行代码；

按钮2和按钮3都是单步执行，就是说点一下会执行一行代码，不同点是当遇到子函数时，按钮2会将子函数当成一行，不会进入子函数，而按钮3会进入子函数内部。

按钮4的功能是在单步调试执行到子函数内时，单击Step Out会执行完子函数剩余部分，并跳出返回到上一层函数。

断点调试中常用的功能就这么多吧，更多的功能大家可以在实战中多多尝试。
