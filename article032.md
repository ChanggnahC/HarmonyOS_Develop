## A detailed interpretation of the project structure for developing HarmonyOS NEXT Applications by uniapp

Yesterday's article introduced how to configure the development environment and run and debug projects when using the uniapp cross-platform HarmonyOS application. Today, let's introduce the structure of the uniapp project directory.

Perhaps for friends engaged in mobile development, the project structure of uniapp may seem a bit unfamiliar. It is closer to a front-end project. The structure of a newly created uniapp project is as follows:

![img2](https://dl-harmonyos.51cto.com/images/202505/b69b09521132097f1b3315500809bbe72b2665.png "img2")

The two folders above, "pages", store the pages of the project, while "static" stores the files of static resources.


Open the "pages" folder and you can see the index file of the project initialization. This is the initialization page of the project. There is a simple "Hello World" project code in it, which can be seen when running the project.

![img2](https://dl-harmonyos.51cto.com/images/202505/7186cbb21e778dbc6c80097972a8bb037d15cf.png "img2")

Keep looking down. When you open the App.vue file, you can see that it stores the project's lifecycle functions, such as project startup, project exit, and so on.

![img2](https://dl-harmonyos.51cto.com/images/202505/01e649b44e0ea2f0de7421ce7e7c84d7e65642.png "img2")

index.html is equivalent to the root page of the project. Although it is also a page, in uniapp, it usually doesn't need to be modified by everyone because you can see that there is a container with the id of the app inside. All the code we write in the project will eventually be injected into this container.

![img2](https://dl-harmonyos.51cto.com/images/202505/c77b0ae217aed397f1d44680360b861f0ddb44.png "img2")

Main.js is the core entry file of the entire project, responsible for initializing Vue instances and global configuration.


The Manifest.json file stores various configuration files of the project, including the project name, version, description, and the operation configuration for each platform.


The Pages.json file stores all the page paths and global styles in the project. If you try to make some modifications to the styles in globalStyle, the global style of the project will be changed. And when a new page is created, it will also be automatically registered here.

![img2](https://dl-harmonyos.51cto.com/images/202505/66e0b0e296d0de2ce93653410f6a3df90a3c5f.png "img2")

The final uni.scss file. When opened, it can be seen that it has provided explanations. Here are the common style variables built into uni-app.


The above is the project structure after the initialization of the uniapp project. It's not over yet. After we run the project, a new unpackage directory will be added. Open it to see if it's familiar. The compiler compiles the uniapp project into a HarmonyOS project. This is the operating principle of cross-platform development.


![img2](https://dl-harmonyos.51cto.com/images/202505/84e317b2129568afd1430705e2880c526539b3.png "img2")

The above is a detailed interpretation of the uniapp project structure. After two days of project environment setup and introduction, in the following tutorial, we will move on to the formal cross-platform development and coding stage. We welcome your continuous attention.
