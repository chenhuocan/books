## iOS Crash收集与分析

###一、Crash收集

官方文档

[Understanding and Analyzing Application Crash Reports][1]
[1]: https://developer.apple.com/library/archive/technotes/tn2151/_index.html "Understanding and Analyzing Application Crash Reports"

<div><img width=500px src="http://106.39.97.163:58090/uploads/201911/ts_mobile/attach_15d511bd01594948.png"/></div>


通过上图我们可以发现Crash的收集主要有两种方式

####1、使用Xcode从设备获取崩溃日志
把你的手机连接到Mac，并选择 Xcode->Windows->Device and Simulator，然后点击View Device Logs，你会看到手机上会有好多Log，其中Type为Crash的就是崩溃的Log，如下图：

<div><img width=500px src="http://106.39.97.163:58090/uploads/201911/ts_mobile/attach_15d5123aa276a49c.png"/></div>

右键导出Crash日志

####2、通过设备直接获取崩溃日志
1）打开设置->隐私->分析->分析数据，在其中找到你想要的应用程序的日志，日志将使用以下格式命名：<应用名称> _ <崩溃时间> _ <设备名>

2）选择所需的日志，复制文本或点击右上角的分享按钮分享出去，并且把分享得到的.ips.synced或者复制文本而来的.txt文件的后缀名改为.crash，因为Xcode不接受没有.crash扩展名的崩溃日志。

<div align=left><img width=300px src="http://106.39.97.163:58090/uploads/201911/ts_mobile/attach_15d5198651983bb4.png"/></div>

<div align=left><img width=300px src="http://106.39.97.163:58090/uploads/201911/ts_mobile/attach_15d5198ab8b25a4c.png"/></div>


###二、确定崩溃报告是否被符号化

![](http://106.39.97.163:58090/uploads/201911/ts_mobile/attach_15d519fdd4d21758.png)

###三、dSYM符号集
符号集是我们每次Archive一个包之后，都会随之生成的.dSYM文件，这个文件必须使用Xcode进行打包才有（Debug模式默认是关闭的）。每次发布一个版本，我们都需要备份这个文件，以方便以后的调试。
符号集中存储着文件名、函数名、行号与内存地址的映射表，通过符号集分析崩溃的.Crash文件可以准确知道具体的崩溃信息。
我们如果不使用.dSYM文件获取到的崩溃信息都是不完全的（官方文档说了会导致不完全符号化，也就是一部分符号化好了，一部分没有）。
每一个.dSYM文件都有一个UUID，和.app文件中的UUID对应，代表着是一个应用，而.dSYM文件中每一条崩溃信息也有一个单独的UUID，用来和程序的UUID进行校对。

####符号集的生成与获取：

- 符号集在Organizer中选中打包的Archive->Show in Finder中选中Archive，右键显示包内容下的dSYMs文件夹下（或者点击Organizer右边的Download dSYM，XCode会从App Store下载该文件并插入到此Archive中）。
![]
<div align=left><img width=500px src="http://106.39.97.163:58090/uploads/201911/ts_mobile/attach_15d51c3b91a9472c.jpeg"/></div>


- 如果在Debug模式下，找到项目的Build Settings。 把Debug Infomatiion Format设置成DWARF with dSYM file。并把Generate Debug Symbols置为YES。然后编译，在项目文件夹Products中找到.app文件右击Show in Finder找到dSYM文件。

<div align=left><img width=500px src="http://106.39.97.163:58090/uploads/201911/ts_mobile/attach_15d51aefea7df840.png"/></div>
<div align=left><img width=500px src="http://106.39.97.163:58090/uploads/201911/ts_mobile/attach_15d51bc9610f36fc.png"/></div>

###四、符号化文件
####1、通过XCode自动符号化Crash文件
如果本地存在.crash对应的.dSYM文件，则直接到上文中（1、使用Xcode从设备获取崩溃日志：）到View Device Logs这步，把文件拖入右边的logs列表，Xcode会自动去符号化文件，如果满眼都是16进制数字的化，点击Re-Symbolicate Log即可
![](http://106.39.97.163:58090/uploads/201911/ts_mobile/attach_15d51c816e141f58.png)
如果此时本地的Archive文件已经被你删除，.dSYM没有备份，则需要回到crash日志对应的版本重新打包（论版本控制的重要性！）

####2、通过Xcode工具symbolicatecrash符号化
#####1 通过find命令找到symbolicatecrash工具的路径
````
find /Applications/Xcode.app -name symbolicatecrash -type f
````
![](http://106.39.97.163:58090/uploads/201911/ts_mobile/attach_15d51cf473fe52bc.png)
前往该文件目录找到symbolicatecrash工具


#####2 将symbolicatecrash工具、crash文件、dSYM文件及.app文件拷贝自己新建的crash文件目录
示例：
![](http://106.39.97.163:58090/uploads/201911/ts_mobile/attach_15d51fd5ef771a30.png)
#####3 执行命令解析crash文件
cd到crash文件目录执行命令解析crash文件
````
./symbolicatecrash ./*.crash ./*.app.dSYM > symbol.crash
````
symbol.crash文件就是解析后的crash文件。

可能遇到的问题：
问题一：Error: "DEVELOPER_DIR" is not defined at ./symbolicatecrash line 69.
解决方法：
终端执行下面的命令设置环境变量。
````
export DEVELOPER_DIR=/Applications/XCode.app/Contents/Developer
````
解析前
![](http://106.39.97.163:58090/uploads/201911/ts_mobile/attach_15d51e897ab62760.png)
解析后
![](http://106.39.97.163:58090/uploads/201911/ts_mobile/attach_15d51e907456bd4c.png)

###五、补充文件校验
####在符号化Crash文件之前，你需要准备好.crash和.dSYM并校验是否匹配
为什么要校验：
- 因为符号集存储着文件名、函数名、行号的信息，每一次代码更改后编译符号集也会随之变更，所以要想符号化.crash文件，.crash与符号集必须一一对应。
- 也就是说由版本为1.0的代码生成了1.0的APP，同时生成了1.0的符号集，1.0的APP发生了Bug，生成了104115的crash文件，也只有1.0的符号集才能够符号化104115的crash文件，而同时也必须找到1.0的代码才能根据符号化的crash文件精确定位到bug产生的地方。

####如何判断.crash、.dSYM与.app（是否匹配你的代码）是否匹配？
- 通过UUID来匹配，UUID是Xcode在编译时自动为每个版本生成的唯一标识，即使功能相同的可执行文件是使用相同的编译器设置从相同的源代码重建的，它也将具有不同的构建UUID，总之UUID是唯一的。
获取.crash的UUID
````
grep "'Your AppName' arm64" xxx.crash
````
获取.dSYM的UUID
````
dwarfdump --uuid 'Your AppName'.app.dSYM
````
获取.app的的UUID
````
dwarfdump --uuid 'Your AppName'.app/'Your AppName'
````
![](http://106.39.97.163:58090/uploads/201911/ts_mobile/attach_15d51f9e1dcf531c.png)
比如上图能看到三者的UUID都是一致的，可以安心去符号化文件啦
