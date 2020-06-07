# **ts_loading.vue loading提示组件**

## **1、组件名称**
ts_loading.vue loading提示组件,可以配置加载图片及文本。
## **2、效果图**
<center>
![](http://106.39.97.163:58090/uploads/201806/mobileapp/attach_1537ac811b33a370.png)
</center>

## **3、使用方法**
````
  <template>
  <div class="bgf8f8f8">
    <ts-header :title="title"
               :leftItem="leftItem"
                @onLeftClick="goback"
    ></ts-header>
    <div class="loading-container">
      <text class="loading-btn" @click="_showLoading">显示加载</text>
      <ts-loading :show="show" :navStyle="navStyle"></ts-loading>
    </div>
  </div>
</template>

<script>
  import TsHeader from '@/components/ts_header';
  import TsLoading from '@/components/ts_loading';
  import mixins from '@/mixins/mixins';
  import common from '@/common/js/common';
  export default {
    components: { TsHeader, TsLoading },
    mixins: [mixins],
    data() {
      return {
        title: '加载',
        leftItem: {
          image: ''
        },
        show: false,
        navStyle: {
          top: '0px'
        }
      }
    },
    created() {
      this.onInit();
    },
    methods: {
      onInit() {
        const imageBaseUrl = common.getImageUrl();
        this.leftItem.image = imageBaseUrl + 'common/left.png';

        const barHeight = common.getBarHeight();
        this.navStyle.top = (barHeight + 90) + 'px';
      },
      _showLoading() {
        this.show = true;
      }
    }
  }
</script>

<style>
  .bgf8f8f8 {
    background-color: #f8f8f8;
  }
  .loading-container {
    left: 0px;
    right: 0px;
    padding-top: 40px;
    padding-bottom: 40px;
    align-items: center;
    justify-content: center;
  }
  .loading-btn {
    width: 200px;
    height: 50px;
    line-height: 50px;
    text-align: center;
    font-size: 26px;
    color: #fff;
    background-color: #3D80FA;
  }
</style>
````

## **4、可配置参数说明**

#### 可配置参数
| 属性名 | 类型 | 是否必传 | 默认值 | 描述 |
| ------------ | ------------  | ------------  | ------------| ------------ |
| loadUrl | String | false |-| 加载图标图片 |
| loadingText | Number | false| - | 加载文字 |
| show | Boolean | true |false| 是否显示 |
| navStyle | Object |false| {} | 加载样式 |
## **5、事件回调**
无
## **6、组件代码**
````
<template>
    <ts-overlay :show="show" :canAutoClose="false" :navStyle="loadingStyle">
      <div class="ts-loading">
        <ts-loading-icon :loadStyle="loadStyle"></ts-loading-icon>
        <text v-if="loadingText" class="ts-loading-text">{{loadingText}}</text>
      </div>
    </ts-overlay>
</template>
<script>
  import TsOverlay from './ts_overlay';
  import TsLoadingIcon from './element/ts_loading_icon';
  const animation = weex.requireModule('animation')
  const modal = weex.requireModule('modal')
  export default {
    components: { TsOverlay, TsLoadingIcon },
    props: {
      loadUrl: {
        type: String,
        default: ''
      },
      loadingText: {
        type: String,
        default: ''
      },
      show:{
        type: Boolean,
        default: false
      },
      navStyle: {
        type: Object,
        default: () => { return {}}
      }
    },
    data() {
      return {
        loadStyle: {
          width: '64px',
          height: '64px',
        }
      }
    },
    computed: {
      loadingStyle() {
        const { navStyle } = this;
        let style = {
          top: '0px',
          backgroundColor: 'rgba(255,255,255, .5)'
        }
        return Object.assign(style, navStyle);
      }
    }
  };
</script>
<style scoped>
.ts-loading {
  align-items: center;
  justify-content: center;
  border-top-left-radius:20px;
  border-top-right-radius:20px;
  border-bottom-left-radius:20px;
  border-bottom-right-radius:20px;
  width: 128px;
  height: 128px;
  background-color: #2D2E32;
}
.ts-loading-img {
  height: 68px;
  width: 68px;
}
.ts-loading-text {
  color: #666666;
  font-size: 26px;
  line-height:40px;
  height: 40px;
  margin-top: 12px;
  text-overflow: ellipsis;
  width: 150px;
  text-align: center;
}
</style>

````
