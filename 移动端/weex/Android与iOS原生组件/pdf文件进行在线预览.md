文档修改人：张颜木盛
邮箱：zhangyanmusheng@tansun.com.cn

### tsFilepreviewComponent
通过组件对pdf文件进行在线预览
### 属性
##### param(String)：文件参数json字符串

|  字段 | 类型  | 含义  |备注 |
| ------------ | ------------ | ------------ | ------------ |
| param  | String  | 参数配置 | paramObject的JSON字符串， |


#### param(Object)说明
|  字段 | 类型  | 含义  |备注 |
| ------------ | ------------ | ------------ | ------------ |
| pdfPath  | String  | 文件路径  | 支持本地及网络url |
| fullname | String | 水印文字  | 水印文字 |

````
param格式示例
{
"pdfPath": "https://102.alibaba.com/downloadFile.do?file=1520478361732/Android_v9.pdf",
"fullname": "天阳科技"
}
````

### 代码示例
````
<template>
	<div>
<tsFilepreviewComponent class="quickLook" :param="format(param)"> </tsFilepreviewComponent>
	</div>
</template>

<script>
export default {

data: function () {
    return {
    param: {},
    }
},

created: function () {
    var self = this;
    var fileUrl = "https://102.alibaba.com/downloadFile.do?file=1520478361732/Android_v9.pdf";
    self.param = {pdfPath: fileUrl, fullname: "天阳科技"};
},

methods: {

//对象转json
format(options) {
        return JSON.stringify(JSON.stringify(options, function (key, val) {
            if (typeof val === 'function') {
                return val + '';
            }
            return val;
        }))
    }
}
}
</script>

<style scoped>
.quickLook {
 top: 0px;
 left: 0px;
 width: 750px;
 bottom: 0px;
 flex: 1
}
</style>
````
