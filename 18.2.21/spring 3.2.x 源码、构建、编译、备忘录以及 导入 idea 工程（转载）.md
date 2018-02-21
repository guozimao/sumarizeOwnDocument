# spring 3.2.x 源码、构建、编译、备忘录以及 导入 idea 工程（转载）

#### 1、环境

1. windows 7 64位
2. IDEA 2017.3 64位
3. JDK8 64 位 8u162 [JDK8 下载](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
4. [GRADLE 2.14.1](https://services.gradle.org/distributions/gradle-2.14.1-bin.zip)

#### 2、准备工作

1.设置JAVA_HOME,设置GRADLE_HOME，java和gradle的bin要放在windows的path里。 
2.GitHub [下载spring源码](https://github.com/spring-projects/spring-framework)，这里是3.2.x,记得切换在网页上切换分支，或者直接clone下来切换到3.2.x 。

#### 3、步骤

1. 在源码的根目录里，运行 gradle idea，开始编译idea所需的项目文件，提示build success 即可。另外可以运行 gradle tasks来查看所有的命令。另外可以执行 gradle cleanidea来清除执行idea的编译。千万不要按照官方的文档执行 gradle build…..到时候你就哭把。。。
2. 在IDEA里直接open 所在的源码目录就行了，提示你import gradle 项目，直接cancel。不用理会。
3. ![这里写图片描述](http://img.blog.csdn.net/20171213112521248?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdGFvbGkxOTg2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast) 
   如图所示有时候idea会扫描所有的spring 模块 文件夹并自动添加编译好的模块，但有时候需要自己添加，具体看如图，**选择对应模块根目录的以模块名开头的.iml文件，右键选择import xxxx module.**
4. 如果一切顺利的话，如图就能看到，对应模块前面有个**蓝色的小点**，表示模块顺利导入。![这里写图片描述](http://img.blog.csdn.net/20171213112700643?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdGFvbGkxOTg2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
5. 按照上面所示，依次导入在spring源码工程里的各个模块，导入工作完成。
6. **补充: 可以在spring源码目录通过点击spring.ipr文件来直接导入整个工程，不需要一个个按照模块去添加了。（跳过3-5步）**
7. 添加丢失的2个jar文件，如图所示， 
   ![这里写图片描述](http://img.blog.csdn.net/20171213150920504?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdGFvbGkxOTg2/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast) 
   spring core里会需要依赖2个jar包，spring-asm-repack-5.0.4.jar 和 spring-cglib-repack-3.1.jar。这个是我自己制作的，基于spring-core-3.2.18.RELEASE.jar，可以参考[我另外的一篇博客](http://blog.csdn.net/taoli1986/article/details/78791168)下载对应的spring 3工程。lib位置在源码路径的 \spring-core\build\libs里。本质就是修改spring-core-3.2.18.RELEASE.jar里的结构，删除掉不用的class，并修改jar的名称。我是直接用winrar来修改保存的，直接解压出来，并重新打包为JAR有问题。
8. 这里附上我提供的2个lib包地址。[点我下载](http://download.csdn.net/download/taoli1986/10156866)