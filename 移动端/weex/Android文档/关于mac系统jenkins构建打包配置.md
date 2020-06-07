

## Android 关于mac系统jenkins构建打包配置说明
### 前提
在mac系统上配置好Android基础环境，安装好JDK以及Android Studio，详细说明可查看移动平台环境配置中的Android环境配置，链接地址：
http://106.39.97.163:58090/docs/ts_mobile/alicodescan-291563185278
### 一、全局工具配置
1.登陆Jenkins，进入Jenkins首页
![](http://106.39.97.163:58090/uploads/202003/ts_mobile/attach_1600f19f21a6cfe8.png)
2.点击左侧Manage Jenkins菜单，进入Jenkins 管理页面
![](http://106.39.97.163:58090/uploads/202003/ts_mobile/attach_1600f1b54eb1d79c.png)
3.点击Global Tool Configuration，进入全局工具配置页面，如下图所示，配置JDK配置，（根据需要进行git配置）
![](http://106.39.97.163:58090/uploads/202003/ts_mobile/attach_1600f20561bbed50.png)
4.进行Gradle配置，Gradle配置可配置为读取本地安装好的路径，也可以配置为通过远程自动加载
![](http://106.39.97.163:58090/uploads/202003/ts_mobile/attach_1600f223b42e1400.png)
### 二、新建一个Android构建打包Jenkins任务
1.点击新建Item
![](http://106.39.97.163:58090/uploads/202003/ts_mobile/attach_1600f2a0a78c06d0.png)
2.进入任务配置页面，填写任务名称以及选择任务形式，点击确定
![](http://106.39.97.163:58090/uploads/202003/ts_mobile/attach_1600f2b0de0d5e24.png)
3.进入构建打包配置页面，进行General选项配置。填写任务说明，根据需要添加配置任务执行参数
![](http://106.39.97.163:58090/uploads/202003/ts_mobile/attach_1600f330d06666dc.png)
4.源码管理配置，可以根据实际需要配置从git或者svn拉取项目源码，如下图以从svn拉取源码为例，配置源码地址以及账号密码即可。
![](http://106.39.97.163:58090/uploads/202003/ts_mobile/attach_1600f36278d77a8c.png)
5.构建触发器与构建环境可根据需要进行配置，这里不做太多说明
![](http://106.39.97.163:58090/uploads/202003/ts_mobile/attach_1600f38d7b0e1344.png)
6.通过构建选项，添加构建步骤，如下图所示
![](http://106.39.97.163:58090/uploads/202003/ts_mobile/attach_1600f5a9d1656710.png)
- 选择Gradle版本，添加Gradle构建命令进行构建，
- 执行shell脚本将构建成功后的安装包复制到指定位置
![](http://106.39.97.163:58090/uploads/202003/ts_mobile/attach_1600f5d1b0becb64.png)
shell脚本内容：

````
script_dir=${WORKSPACE}

echo "构建类型：${BUILD_TYPE}"

OUTPUT_APK_PATH="$script_dir/app/build/outputs/apk/${BUILD_TYPE}/xyjkerp.apk"

EXPORT_LAST_IPA_PATH="/usr/local/var/www/tsXyjkERP"
EXPORT_LAST_IPA_NAME="XyjkERP.apk"

# 检查文件是否存在
if [ -f "${OUTPUT_APK_PATH}" ] ; then
    echo "导出 ${EXPORT_LAST_IPA_NAME} 包成功 🎉🎉🎉🎉🎉🎉🎉🎉🎉 "
    # 进行文件移动
    mv $OUTPUT_APK_PATH $EXPORT_LAST_IPA_PATH/$EXPORT_LAST_IPA_NAME
else
    echo "导出 ${EXPORT_LAST_IPA_NAME} 包失败 😢😢😢😢😢😢😢😢😢"
    exit 1
fi

````
至此就可以实现使用Jenkins进行构建打包了
### 三、配置注意问题说明
1.进行任务参数配置时，需要根据需要选择合适的参数类型
![](http://106.39.97.163:58090/uploads/202003/ts_mobile/attach_1600f65715fd0524.png)
2.使用git或者svn拉取项目源码时，账号密码可以从全局变量列表中选择，
![](http://106.39.97.163:58090/uploads/202003/ts_mobile/attach_1600f68391678794.png)
也可以点击右边按钮直接添加
![](http://106.39.97.163:58090/uploads/202003/ts_mobile/attach_1600f69cd68ef8d4.png)
还可以在Jenkins首页凭据管理中进行添加
![](http://106.39.97.163:58090/uploads/202003/ts_mobile/attach_1600f6e3d1481058.png)
3.构建时，Gradle版本是全局工具中配置的Gradle版本列表，可根据需要在全局工具配置中添加多个Gradle版本
![](http://106.39.97.163:58090/uploads/202003/ts_mobile/attach_1600f6b16b3fcc74.png)
4.关于构建脚本，根据不同项目的构建任务编写具体的脚本，不同项目的脚本会有所差异，不能直接复制使用
