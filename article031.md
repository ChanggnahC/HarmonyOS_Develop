## The first experience of developing HarmonyOS NEXT applications across platforms with uniapp
I previously wrote a tutorial on developing HarmonyOS applications using uniapp, briefly introducing how to configure the development environment and run the project. Back then, HbuilderX was still version 4.22. A year has passed, and the official version of HbuilderX has reached 4.64. After going through multiple version updates, the experience of developing HarmonyOS applications across platforms has been greatly improved. Today, I'd like to share with you again the tutorial on developing HarmonyOS using uniapp.


The biggest feeling of this development experience is that it is very convenient and fast. You don't even need to open DevEco studio. You can directly use Hbuilder to connect to the real machine for debugging. There is also automatic real-time compilation and update, which is very friendly.


First of all, you need to go to AppGallery Connect to create a new application. After filling in the application name and package name, create the application. We will need the package name later. Post the address of AppGallery Connect:
`
https://developer.huawei.com/consumer/cn/service/josp/agc/index.html#/
`

![img1](https://dl-harmonyos.51cto.com/images/202505/a474d9309bc464d324c510adb65f7ec3eb446e.png "img1")

Next, download and install the latest version of HbuilderX. Currently, it is version 4.64. Download link:

`
https://www.dcloud.io/hbuilderx.html
`
Open HbuilderX to create a new project and select 3.0 for the Vue version

![img2](https://dl-harmonyos.51cto.com/images/202505/c55d87393082a8dd794458951bb376440a4222.png "img2")

After creating the project, it is also possible to connect the computer to the real machine or open the simulator of DevEco studio. Then, under the run menu of HbuilderX, find "Run to HarmonyOS". The first time, there may be a step to download the real device plugin and a pop-up window to configure and debug the certificate. The real device plugin is downloaded automatically. To configure the certificate, you need to fill in the package name used when creating the application in the first step. Then, I recommend that everyone use the automatic version


Apply for a debugging certificate. After the information is automatically filled in, click "Save".


Run it again and a list of detected devices will pop up:

![img3](https://dl-harmonyos.51cto.com/images/202505/7334e3a90145a491fb8035559ad76d7da29e11.png "img3")

Select the device and the application can be run on the device:

![img4](https://dl-harmonyos.51cto.com/images/202505/754e3042678b2292a008893fcdefa8d82bf6ba.png "img4")

At this point, if you try to modify some content, the program will automatically compile and update, which is very convenient.


The entire development process is even more user-friendly than native development. The cross-platform development experience of HarmonyOS has taken a big step forward. It is believed that more and more developers will join the cross-platform development team in the future. This is also in line with the previous prediction of Youlan Jun. In the face of the three mobile operating systems, cross-platform development is the first choice for many small and medium-sized enterprises, and unipp is a particularly outstanding cross-platform development framework, offering an excellent development experience.


The above is the latest experience of developing HarmonyOS applications using uniapp. In the future, we will continue to share in-depth tutorials on cross-platform development of HarmonyOS applications using uniapp with you all. We also plan to compile this part into a collection to facilitate your continuous learning. Welcome to follow us.
