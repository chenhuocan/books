##iOS脚本自动打包流程

###简要流程
- 脚本拉取工程代码
- 脚本修改工程基本信息/图标/启动图
- 脚本修改podfile文件
- 脚本进行Module/Component注册
- 脚本进行第三方key注册
- pod操作
- 自动打包
- 证书导入
- 获取描述文件信息

###JSON解析
使用jq进行JSON解析
Mac安装jq
````
brew install jq
````
备注：熟悉jq指令

###脚本拉取工程代码
使用 svn 的话，会提示输入用户名、密码，有时会因此报错，建议一开始就执行svn checkout指令，让电脑记住用户名、密码。
````
svn checkout ${SVN_PATH} ${CHECKOUT_PATH} --username ${USER_NAME} --password ${PASSWORD}
````
备注：命令行涉及svn指令

###脚本修改工程基本信息/图标/启动图
1、修改App显示名称、版本号、build版本号
本质修改info.plist
````
#修改App显示名称 CFBundleDisplayName
AppBundleDisplayName='App显示名称'
/usr/libexec/PlistBuddy -c "Set :CFBundleDisplayName $AppBundleDisplayName" $PLIST_PATH

#修改App版本号 CFBundleShortVersionString
AppBundleShortVersionString='1.0.0'
/usr/libexec/PlistBuddy -c "Set :CFBundleShortVersionString $AppBundleShortVersionString" $PLIST_PATH

#修改工程打包版本号 CFBundleVersion
AppBundleVersion='1'
/usr/libexec/PlistBuddy -c "Set :CFBundleVersion $AppBundleVersion" $PLIST_PATH
````
备注：命令行涉及PlistBuddy指令

2、替换图标/启动图
本质文件拷贝替换
````
cp "$FILE_PATH" "$TO_FILE_PATH"
````
备注：命令行涉及cp等文件操作指令

###脚本修改podfile文件
目前操作：本质进行字符串替换
````
//-i 后面''空字符串表示在源文件进行修改 若不添加 命令行会报错
sed -i '' "s/${替换字符串}/${替换内容}/g" ${文件路径}
````

备注：命令行涉及 sed

###脚本进行Module/Component注册
1、添加头文件
目前操作：指点行插入
````
\空格 表示空格
\回车 表示换行

//-i 后面''空字符串表示在源文件进行修改 若不添加 命令行会报错
//在指定行//EDIT_HEADER_START后面插入
sed -i '' -e '\/\/EDIT_HEADER_START/a\
/**'${头文件注释}'*/\
#import "'${头文件名}'"' ${文件路径}
````
2、Module/Component注册
目前操作：本质进行字符串替换
````
//-i 后面''空字符串表示在源文件进行修改 若不添加 命令行会报错
sed -i '' "s/${替换字符串}/${替换内容}/g" ${文件路径}
````
备注：命令行涉及 sed

###脚本进行第三方key注册
1、添加头文件
目前操作：指点行插入
````
\空格 表示空格
\回车 表示换行

示例：
//-i 后面''空字符串表示在源文件进行修改 若不添加 命令行会报错
//在指定行//EDIT_GEGISTAPP_START后面插入
sed -i '' -e '/\/EDIT_GEGISTAPP_START/a\
\ \ \ \ \/\/注释\
\ \ \ \ [[AipOcrService shardService] authWithAK:@\"'${registKey}'\" andSK:@\"'${registSecret}'\"];\
' ${文件路径}
````
备注：命令行涉及 sed

###pod操作
````
方式1 /usr/local/bin/pod update --verbose --no-repo-update
或者
方式2 /usr/local/bin/pod install
//如果工程中的组件从没有被pod过，使用方式1会出错
````
备注：命令行涉及 pod

###自动打包
````
# 是否编译工作空间 (例:若是用Cocopods管理的.xcworkspace项目,赋值true;用Xcode默认创建的.xcodeproj,赋值false)
is_workspace="true"
# .xcworkspace的名字，如果is_workspace为true，则必须填。否则可不填
workspace_name="TSMadp"
# .xcodeproj的名字，如果is_workspace为false，则必须填。否则可不填
project_name="TSMadp"
# 指定项目的scheme名称（也就是工程的target名称），必填。 多targets会默认第一个所以必填
scheme_name="TSMadp"

# =============打包设置相关=============
# 指定要打包编译的方式 : Release,Debug。一般用Release。必填
build_configuration="Release"
# method，打包的方式。方式分别为 development, ad-hoc, app-store, enterprise 。必填
method="enterprise"

# 下面两个参数只是在手动指定Pofile文件的时候用到，如果使用Xcode自动管理Profile,直接留空就好
# (跟method对应的)mobileprovision文件名，需要先双击安装.mobileprovision文件.手动管理Profile时必填
mobileprovision_name="demoTansunWX"
# 项目的bundleID，手动管理Profile时必填
bundle_identifier="com.tansun.demo.wx"

# =============打包设置其他配置=============

# 查看相关键值 可在.xcodeproj 显示包内容  project.pbxproj 中查看

# 项目的bundleID
ENTER_PRODUCT_BUNDLE_IDENTIFIER="com.tansun.demo.wx"
# 签名标识 iPhone Distribution 、iPhone Developer
ENTER_CODE_SIGN_IDENTITY="iPhone Distribution"
# 签名方式 描述文件UUID
ENTER_PROVISIONING_PROFILE="72f0e017-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
# 签名类型 手动Manual、自动Automatic
ENTER_CODE_SIGN_STYLE="Manual"
#该模式下的描述文件的名称
ENTER_PROVISIONING_PROFILE_SPECIFIER="demoTansunWX"
#开发者证书的team
ENTER_DEVELOPMENT_TEAM="2TB7HBKRQL"
#是否打开bitcode
ENTER_ENABLE_BITCODE="NO"


# 判断编译的项目类型是workspace还是project
if $is_workspace ; then
# 编译前清理工程
xcodebuild clean -workspace ${workspace_name}.xcworkspace \
                 -scheme ${scheme_name} \
                 -configuration ${build_configuration}

xcodebuild archive -workspace ${workspace_name}.xcworkspace \
                   -scheme ${scheme_name} \
                   -configuration ${build_configuration} \
                   -archivePath ${export_archive_path}  \
                   clean \
                   build \
                   CODE_SIGN_IDENTITY="${ENTER_CODE_SIGN_IDENTITY}" \
                   PROVISIONING_PROFILE="${ENTER_PROVISIONING_PROFILE}" \
                   PROVISIONING_PROFILE_SPECIFIER="${ENTER_PROVISIONING_PROFILE_SPECIFIER}" \
                   PRODUCT_BUNDLE_IDENTIFIER="${ENTER_PRODUCT_BUNDLE_IDENTIFIER}" \
                   CODE_SIGN_STYLE="${ENTER_CODE_SIGN_STYLE}" \
                   DEVELOPMENT_TEAM="${ENTER_DEVELOPMENT_TEAM}" \
                   ENABLE_BITCODE="${ENTER_ENABLE_BITCODE}"

else
# 编译前清理工程
xcodebuild clean -project ${project_name}.xcodeproj \
                 -scheme ${scheme_name} \
                 -configuration ${build_configuration}

xcodebuild archive -project ${project_name}.xcodeproj \
                   -scheme ${scheme_name} \
                   -configuration ${build_configuration} \
                   -archivePath ${export_archive_path} \
                    clean \
                    build \
                    CODE_SIGN_IDENTITY="${ENTER_CODE_SIGN_IDENTITY}" \
                    PROVISIONING_PROFILE="${ENTER_PROVISIONING_PROFILE}" \
                    PROVISIONING_PROFILE_SPECIFIER="${ENTER_PROVISIONING_PROFILE_SPECIFIER}" \
                    PRODUCT_BUNDLE_IDENTIFIER="${ENTER_PRODUCT_BUNDLE_IDENTIFIER}" \
                    CODE_SIGN_STYLE="${ENTER_CODE_SIGN_STYLE}" \
                    DEVELOPMENT_TEAM="${ENTER_DEVELOPMENT_TEAM}" \
                    ENABLE_BITCODE="${ENTER_ENABLE_BITCODE}"

fi


# 根据参数生成export_options_plist文件 可以参考手动打包导出的ExportOptions.plist

/usr/libexec/PlistBuddy -c  "Add :method String ${method}"  $export_options_plist_path
/usr/libexec/PlistBuddy -c  "Add :provisioningProfiles:"    $export_options_plist_path
/usr/libexec/PlistBuddy -c  "Add :provisioningProfiles:${bundle_identifier} String ${mobileprovision_name}"  $export_options_plist_path

xcodebuild  -exportArchive \
            -archivePath ${export_archive_path} \
            -exportPath ${export_ipa_path} \
            -exportOptionsPlist ${export_options_plist_path} \
            -allowProvisioningUpdates
````

备注：命令行涉及 xcodebuild

###证书导入
````
#解锁KeyChain
security unlock-keychain -p ${MAC_PASSWORD} /Users/${USER_NAME}/Library/Keychains/login.keychain
#证书导入KeyChain
security import ${P12_FILE_PATH} -k /Users/${USER_NAME}/Library/Keychains/login.keychain -P ${P12_FILE_PASSWORD} -T /usr/bin/codesign
#security import ${P12_FILE_PATH} -k /Users/${USER_NAME}/Library/Keychains/login.keychain -P ${P12_FILE_PASSWORD} -A -T /usr/bin/codesign
````

###获取描述文件信息
````
mobileprovision_uuid=`/usr/libexec/PlistBuddy -c "Print UUID" /dev/stdin <<< $(/usr/bin/security cms -D -i $PROVISION_FILE_PATH)`
mobileprovision_appIDName=`/usr/libexec/PlistBuddy -c "Print AppIDName" /dev/stdin <<< $(/usr/bin/security cms -D -i $PROVISION_FILE_PATH)`
mobileprovision_name=`/usr/libexec/PlistBuddy -c "Print Name" /dev/stdin <<< $(/usr/bin/security cms -D -i $PROVISION_FILE_PATH)`
````

###脚本打包时去除钥匙串始终允许弹窗
````
securityset-key-partition-list -S apple-tool:,apple:,codesign: -s -k ${PASSWORD} ~/Library/Keychains/login.keychain-db
````
