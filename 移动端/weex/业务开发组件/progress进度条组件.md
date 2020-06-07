# **ts_progress.vue 进度条组件**

## **1、组件名称**
ts_progress.vue 进度条组件,表明某个任务的当前进度
## **2、效果图**
<center>
![](http://106.39.97.163:58090/uploads/201911/mobileapp/attach_15d48435e246a358.png)
</center>
## **3、使用方法**
````
 <template>
  <div class="bgf8f8f8">
    <ts-header :title="title"
               :leftItem="leftItem"
               @onLeftClick="goback"
    ></ts-header>
    <div style="height:30px"></div>
    <div class="demo">
      <text class="demo-text">默认</text>
      <ts-progress></ts-progress>
    </div>
    <div class="demo">
      <text class="demo-text">设置value</text>
      <ts-progress :value=50 :bar-width=600></ts-progress>
    </div>
    <div class="demo">
      <text class="demo-text">自定义</text>
      <ts-progress :value=70
                    bar-color='#9B7B56'
                    :bar-height=16
                    :bar-radius=16
                    :bar-width=640></ts-progress>
    </div>
    <div class="btn" @click="uploadFile">
      <text class="btn-txt">上传文件</text>
    </div>
    <div class="up-demo" v-if="progressVisible">
      <text class="progress-text left">0%</text>
      <ts-progress :value="value" :bar-width=540></ts-progress>
      <text class="progress-text right">{{value}}%</text>
    </div>
  </div>
</template>

<script>
  import TsHeader from '@/components/ts_header';
  import TsProgress from '@/components/ts_progress';
  import mixins from '@/mixins/mixins';
  import common from '@/common/js/common';
  export default {
    components: { TsHeader,TsProgress },
    mixins: [mixins],
    data() {
      return {
        title: '进度条',
        leftItem: {
          image: '',
        },
        value: 0,
        uploading: false,
        progressVisible: false,
        timer: null
      }
    },
    created() {
      this.onInit();
    },
    methods: {
      onInit() {
        const imageBaseUrl = common.getImageUrl();
        this.leftItem.image = imageBaseUrl + 'common/left.png';
      },
      uploadFile() {
        const self=this;
        if (!self.uploading) {
          self.value = 0;
          self.uploading = true;
          self.progressVisible = true;
          self.timer = setInterval(() => {
            if (self.value < 100) {
              self.value++
            } else {
              self.uploading = false;
              setTimeout(() => {
                self.progressVisible = false;
              }, 500)
              clearInterval(self.timer);
            }
          }, 10);
        }
      }
    }
  }
</script>

<style>
  .bgf8f8f8 {
    background-color: #f8f8f8;
  }
  .wxc-demo {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: #ffffff;
  }
  .scroller {
    flex: 1;
  }
  .first-demo {
    margin-top: 80px;
  }
  .demo {
    margin-bottom: 50px;
    padding-left: 75px;
  }
  .demo-text {
    margin-bottom: 20px;
  }
  .up-demo {
    flex-direction: row;
    align-items: center;
    margin-left: 32px;
  }
  .progress-text {
    color: #333333;
    font-size: 30px;
  }
  .left {
    margin-right: 20px;
  }
  .right {
    margin-left: 20px;
  }
  .btn {
    width: 600px;
    height: 80px;
    margin-top: 60px;
    margin-bottom: 50px;
    margin-left: 75px;
    flex-direction: row;
    align-items: center;
    justify-content: center;
    border-radius: 6px;
    background-color: #635147;
    border-color: #635147;
  }
  .btn-txt {
    font-size: 32px;
    color: #ffffff;
  }
</style>
````

## **4、可配置参数说明**

#### 可配置参数
| 属性名 | 类型 | 是否必传 | 默认值 | 描述 |
| ------------ | ------------  | ------------  | ------------| ------------ |
| value | Number | true |0| 百分比(0-100) |
| bar-height | Number | false| 8 | 高度 |
| bar-width | Number | true |600| 长度 |
| bar-color | String |false| #FFC900 | 颜色 |
| bar-radius | Number |false| 0 | 圆角 |
| background-color | String |false| #f2f3f4 | 整体背景色 |
## **5、事件回调**
无
## **6、组件代码**
````
<template>
  <div class="wxc-progress"
       :style="runWayStyle"
       :accessible="true"
       :aria-label="`进度为百分之${value}`">
    <div class="progress" :style="progressStyle"></div>
  </div>
</template>

<style scoped>
  .wxc-progress {
    background-color: #f2f3f4;
  }

  .progress {
    position: absolute;
    background-color: #FFC900;
  }
</style>

<script>
  export default {
    props: {
      barColor: {
        type: String,
        default: '#FFC900'
      },
      barWidth: {
        type: Number,
        default: 600
      },
      barHeight: {
        type: Number,
        default: 8
      },
      barRadius: {
        type: Number,
        default: 0
      },
      value: {
        type: Number,
        default: 0
      },
      backgroundColor: {
        type: String,
        default: '#f2f3f4'
      }
    },
    computed: {
      runWayStyle() {
        const {barWidth, barHeight, barRadius, backgroundColor} = this;
        return {
          width: barWidth + 'px',
          height: barHeight + 'px',
          borderRadius: barRadius + 'px',
          backgroundColor
        }
      },
      progressStyle() {
        const {value, barWidth, barHeight, barColor, barRadius} = this;
        const newValue = value < 0 ? 0 : (value > 100 ? 100 : value);
        return {
          backgroundColor: barColor,
          borderRadius: barRadius + 'px',
          height: barHeight + 'px',
          width: newValue / 100 * barWidth + 'px'
        }
      }
    }
  }
</script>
````
