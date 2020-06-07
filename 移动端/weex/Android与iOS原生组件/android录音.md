文档修改人：刘旸
邮箱：liuyangxm01@tansun.com.cn
## 调用系统功能
##API
###1、调用系统录音API
````
const tsSystemAudio= weex.requireModule('tsSystemAudio');
````
recordAudio(callback)
#### 参数
|  参数 | 类型  | 含义  |备注 |
| ------------ | ------------ | ------------ | ------------ |
| callback  |  Function | 回调函数  | 录音回调函数 |

#### 返回值(Object)
|  字段 | 类型  | 含义  |备注 |
| ------------ | ------------ | ------------ | ------------ |
| resultCode  | Integer  | 结果码  |  -1：取消 0：失败 1：成功 |
| resultMsg  |  String | 结果提示消息  | 取消  失败 成功 |
| data | object | 结果数据 | 详见data说明 |

#####data(Object)说明
|  字段 | 类型  | 含义  |备注 |
| ------------ | ------------ | ------------ | ------------ |
| path  |  String |  文件路径 | 无 |

````
//成功
{
	"resultMsg": "成功",
	"data": {
		"path": "/storage/emulated/0/MIUI/sound_recorder/5月3日 下午7点08分.mp3"
	},
	"resultCode": 1
}
//失败
{
	"resultMsg": "未获取权限",
	"data": {},
	"resultCode": 0
}
//取消
{
	"resultMsg": "取消",
	"data": {},
	"resultCode": -1
}
````
#### 代码示例
````
const tsSystemAudio = weex.requireModule('tsSystemAudio');
tsSystemAudio.recordAudio(function (e) {
	 console.log(e);
});
````
