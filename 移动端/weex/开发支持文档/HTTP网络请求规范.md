#HTTP网络请求规范

## 1.1	网络分层协议
服务平台接口消息协议栈如下图所示：
<center>
![](http://106.39.97.163:58090/uploads/201806/mobileapp/attach_1537abe7951aa458.png)
接口消息协议栈示意图
</center>

###1.1.1	网络请求方式

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;接口消息采用HTTP/HTTPS + JSON方式，移动APP首先同服务平台建立会话，平台在消息应答返回token。移动App在后续的请求中携带token继续进行其他接口消息交互。

###1.1.2	网络通信报文规范
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;数据网络请求报文组装方式：head + body；其中head部分为平台要求的报文头，body部分为移动APP与服务端交互的业务报文，body部分的报文格式为子系统报文，均采用JSON格式。

####1.1.2.1	报文规范

HTTP BODY请求报文：
````
{
    "head": {
        "ClientType": 代表终端设备类型：iOS、Android（必输）, 
        "ClientIMEI ": 终端设备唯一标识码（必输）
        "Format": 报文格式（非必输，默认为json）,
        "Charset": 编码格式（非必输，默认utf8）,
        "SignType": 加密算法（非必输，默认国密），
        "Sign":签名（必输，调用加密方法时自动生成，如果不加密可忽略），
        "AppVersion":版本号(必输，APP版本号，以本规范版本号定义为准，如：1.0.0)，
        "Token":权限验证（必输，调用获取token方法获取，生成规则见Token生成规则），
            "Action ": 接口名称码(必输，调用接口名称),
        "TradeSerialNo": 交易流水号(必输，终端唯一设备号+时间（14位系统时间）),
    }
    "body": {
               接口业务内容
    }
}
````

HTTP BODY响应报文：
````
{
    "head": {
         "ClientType": 代表终端设备类型：iOS、Android（必输）, 
         "ClientIMEI ": 终端设备唯一标识码（必输）
         "Format": 报文格式（非必输，默认为json）,
         "Charset": 编码格式（非必输，默认utf8）,
         "SignType": 加密算法（非必输，默认国密），
         "Sign":签名（必输，加密规则见签名方式，如果不加密可忽略），
         "AppVersion":版本号(必输，APP版本号，以本规范版本号定义为准，如：1.0.0)，
         "Token":权限验证（必输，调用获取token方法获取，生成规则见Token生成规则），
             "Action ": 交易码(必输，调用接口名称),
         "TradeSerialNo": 交易流水号(必输，终端唯一设备号+时间（14位系统时间）),
         "rspCode":平台响应编码(必输)
         "rspMsg":平台响应描述(必输)
    }
    "body": {
        接口业务内容
    }
}
````

####1.1.2.2	签名方式
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;由于报文需要通过互联网进行传输，为了保证数据的安全性，按照业内大部分企业的方式，平台要求APP按照平台的要求对报文body进行摘要签名，先使用MD5进行摘要，然后用RSA进行签名，签名后生成sign放在报文head的sign节点里。

####1.1.2.3	接口业务内容加密规则
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;由于报文通过互联网传输，为来保证数据安全性，按照业内大部分企业采用的方式，平台要求对数据包中的业务内容进行加密，具体加密规则比如SMS4、AES等。

####1.1.2.4	Token生成规则
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;由服务端生成，调用获取token方法获取，用于替换用户名和密码，传递客户端和平台通信令牌；客户端和平台未建立会话时，该字段不存在；客户端和平台建立会话后，后续消息交互客户端必须携带该字段参数上报。

####1.1.2.5	业务接口名称定义示例
接口名称Action定义如下：
<table style="width:450px">
    <tr>
        <td style="width:50px;background-color:darkgrey;" align=center>编号</td>
        <td style="width:200px;background-color:darkgrey;">Action</td>
        <td style="width:200px;background-color:darkgrey;">接口描述</td>
    </tr>
    <tr>
        <td colspan=3>客户端登陆注册</td>
    </tr>
    <tr>
        <td style="width:100px;" align=center>1</td>
        <td style="width:200px;">register</td>
        <td style="width:200px;">客户端注册接口</td>
    </tr>
    <tr>
        <td style="width:100px;" align=center>2</td>
        <td style="width:200px;">authenticate</td>
        <td style="width:200px;">客户端登陆鉴权接口</td>
    </tr>
    <tr>
        <td style="width:100px;" align=center>3</td>
        <td style="width:200px;">checkUpdate</td>
        <td style="width:200px;">客户端升级查询接口</td>
    </tr>
    <tr>
        <td colspan=3>个人中心</td>
    </tr>
    <tr>
        <td style="width:100px;" align=center>1</td>
        <td style="width:200px;">getUserInfo</td>
        <td style="width:200px;">获取用户个性化信息接口</td>
    </tr>
</table>

###附录A：打包请求消息样例
加密前：
````
{
    "head":{
        "ClientType" : "Android",
            "ClientIMEI" : "A3C300E4-1DC3-4AF0-9B78-FACA3A7E4CF3",
            "AppVersion" : "V1.0.0",
            "Action" : "register",
            "TxnSrlNo" : "A3C300E4-1DC3-4AF0-9B78-FACA3A7E4CF315216905680307"
    },
    "body": {
        "phoneNumber" : "chenhuocan",
        "password" : "dagrddgfds"
    }
}
````
加密和签名后：
````
{
    "head" : {
        "ClientType" : "Android",
        "ClientIMEI" : "A3C300E4-1DC3-4AF0-9B78-FACA3A7E4CF3",
        "AppVersion" : "V1.0.0",
        "Action" : "register",
        "Sign" : "ycK+dVBfH/KPK7IVhH0hiHs45YrctSDbZO4h8bTnAuzY1FD4FyQRjlz0I73ON5cTzHoKEhSNFE9GukrQTyCQR4qlfHUS8iwuGxmARr0YVQYoLChtd8ZKC4B8O6apD/Pawdooo/wNobX//GavU+hq0HE46cm2LXeiCXt3Q2Hxyfw=",
        "TradeSerialNo" : "A3C300E4-1DC3-4AF0-9B78-FACA3A7E4CF315216899027908"
    },
    "body" : {
        "msgContent" : "H6oZ1A6K1jVm2KGyNcZkp6mqNBbkOcWnZiMZbeWYxnzm2PeQByGODK46EBbBJN9ZJ2NZd2QXlaUE5D6WjnIYvWw6AyleUuPe8o3kqZM/XfOWwNJdGBYXP73vHkHQn5kH"
    }
}
````
###附录B：打包返回消息样例
解密前：
````
{
    "head":{
        "ClientType" : "Android",
        "ClientIMEI" : "A3C300E4-1DC3-4AF0-9B78-FACA3A7E4CF3",
        "AppVersion" : "V1.0.0",
        "Action" : "register",
        "Sign" : "xDoWray4MwCbj/tr/KvMvT/NZk+6gia0ibeHMqJHNdLn8CLemzF4Ws6K1IwwuQI2FNq0mN1SPJJDbM2Gib6bSF5GV26tJRCPh4DkaA8YoXbQQs9kAipQLrAYL/lpKGW2dXw5woyWpcVJp3Quk+D71WQ4A1hEJJlFIWxYT9xEifo=",
        "rspCode" : 000000,
        "rspMsg" : "\U6210\U529f"
    },
    "body":{
        "msgContent" : "81JOU0sI+U1bE/hc2SKHVHxIDDARlYSbp5MNi+RRzCXBV20g2rbKp3LwurbI9xzSquq5IjIcpsKpKEZ4TMEH1NRSzKcGNS7U5Y/dKS3/3P8="
    }
}
````
解密后：
````
{
    "head":{
        "ClientType" : "Android",
        "ClientIMEI" : "A3C300E4-1DC3-4AF0-9B78-FACA3A7E4CF3",
        "AppVersion" : "V1.0.0",
        "Action" : "register",
        "Sign" : "xDoWray4MwCbj/tr/KvMvT/NZk+6gia0ibeHMqJHNdLn8CLemzF4Ws6K1IwwuQI2FNq0mN1SPJJDbM2Gib6bSF5GV26tJRCPh4DkaA8YoXbQQs9kAipQLrAYL/lpKGW2dXw5woyWpcVJp3Quk+D71WQ4A1hEJJlFIWxYT9xEifo=",
        "rspCode" : 000000,
        "rspMsg" : "\U6210\U529f"
    },
    "body":{
        "birthDate" : "19930206",
        "endDate" : "2920-04-25",
        "idCardSex" : "\U7537",
        "issDate" : "2010-04-25",
        "issueOffice : "\U53a6\U95e8\U5e02\U5b89\U708e\U5b89\U5206\U5c40",
        "name" : "\U9648\U706b\U707f",
        "userToken" : "7F58880C7C419C8A91EF055D57E3093256",
        "vcherNo" : "350212199302061536, "
        "vcherType" : "2001"
    };
}
````
