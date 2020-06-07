文档修改人：张颜木盛
邮箱：zhangyanmusheng@tansun.com.cn

## tsRecorder
采集音频信息
````
<tsRecorder></tsRecorder>
````
##**事件**
|  参数 | 类型  | 含义  |备注 |
| ------------ | ------------ | ------------ | ------------ |
| didSelectRecord  |  Function | 选中录音事件  | 返回值为Object类型 |

##**事件返回值**

###1、didSelectRecord:事件返回值(Object)

###Object.tsResult说明

|  字段 | 类型  | 含义  |备注 |
| ------------ | ------------ | ------------ | ------------ |
| resultCode  | Integer | 结果码  | 0:选择失败，1：选择成功 |
| resultMsg  | String | 结果提示消息 | 选择失败，选择成功， |
| data  | Object | 返回值  | dataObject对象  |

####dataObject说明
|  字段 | 类型  | 含义  |备注 |
| ------------ | ------------ | ------------ | ------------ |
| filePath  |  String | 录音文件路径  | 录音文件路径 |

````
//示例
"tsResult": {
   "resultMsg": "选择成功",
   "data": {
      "filePath": "file://xxxxxxxx"
   },
   "resultCode": 1
   
}

"tsResult": {
   "resultMsg": "选择失败",
   "data": {
   },
   "resultCode": 0
}
````

### 代码示例
````
<template>
    <tsRecorder class="recorder"  @didSelectRecord="didSelectRecord"></tsRecorder>
</template>

<script>
	methods: {
		didSelectRecord:function (event) {
			console.log('输出数据' + JSON.stringify(event));
		},
	}
</script>

<style scoped>
    .recorder {
        top: 0px;
        left: 0px;
        width: 750px;
        bottom: 0px;
    }
</style>
````
