## Vue2-混入mixin（可复用性、全局混入）

### mixin 混入
- 混入 (mixin) 提供了一种非常灵活的方式，来分发 Vue 组件中的可复用功能。一个混入对象可以包含任意组件选项。当组件使用混入对象时，所有混入对象的选项将被“混合”进入该组件本身的选项

![WX20231120-145717@2x](https://github.com/zhoubingyan1/utils/assets/18182537/ea84bbeb-ff9e-4127-a38c-bae006b2ba39)
#### 局部混入

```js
// src/mixins/parentMixin.js
export const parentMixin = {
    data() {
      return {
        name:'parentMixin',
        isParentMixin:true,
        moreInfo:{
            position:"./mixins/parentMixin",
            isParentMixin:true,
        }
      };
    },
    beforeCreate(){
        console.log("this is a beforeCreate event in parentMixin");
    },
    created() {
        console.log("this is a created event in parentMixin");
    },
    beforeMount(){
        console.log("this is a beforeMount event in parentMixin");
    },
    methods: {
      helloWorld() {
        console.log("helloWorld in parentMixin");
      },
      printBaseMixin() {
        console.log("printBaseMixin in parentMixin");
      },
    },
  };
  
```

```js
// src/mixins/baseMixin.js
import {parentMixin} from './parentMixin'
export const baseMixin = {
    mixins:[parentMixin],
    data() {
      return {
        name:'baseMixin',
        isBaseMixin:true,
        moreInfo:{
            position:"./mixins/baseMixin",
            isBaseMixin:true,
        }
      };
    },
    beforeCreate(){
        console.log("this is a beforeCreate event in baseMixin");
    },
    created() {
        console.log("this is a created event in baseMixin");
    },
    beforeMount(){
        console.log("this is a beforeMount event in baseMixin");
    },
    methods: {
      helloWorld() {
        console.log("helloWorld in baseMixin");
      },
      printBaseMixin() {
        console.log("printBaseMixin in baseMixin");
      },
    },
  };
  
```

``` shell
// src/components/MixinSyntax.vue
<template>
    <div >

    </div>
  </template>
  
  <script>
  import { baseMixin } from "../mixins/baseMixin";
  
  export default {
    mixins: [baseMixin],
    name: "MixinSyntax",
    data() {
      return {
        name: "MixinSyntax",
        isMixinSynatx:true,
        moreInfo:{
            position:"./components/MixinSyntax",
            isMixinSynatx:true,
        }
      };
    },
    // props: {
    //   msg: String
    // },
    beforeCreate(){
        console.log("this is a beforeCreate event in mixinSyntax");
    },
    created() {
        console.log("this is a created event in mixinSyntax");
    },
    beforeMount(){
        console.log("this is a beforeMount event in mixinSyntax");
    },
    mounted(){
        console.log(this.$data)
        this.printBaseMixin()
    },
    methods: {
      helloWorld() {
        console.log("helloWorld in mixinSyntax");
      },
      printBaseMixin() {
        console.log("printBaseMixin in mixinSyntax");
      },
    },
  };
  </script>
  

  <style scoped>
  h3 {
    margin: 40px 0 0;
  }
  ul {
    list-style-type: none;
    padding: 0;
  }
  li {
    display: inline-block;
    margin: 0 10px;
  }
  a {
    color: #42b983;
  }
  </style>
  
```
![WX20231120-143952@2x](https://github.com/zhoubingyan1/utils/assets/18182537/f3321655-c6e9-4871-bf77-a67c050f38e8)




合并策略
1. data选型，数据对象内部进行递归合并，发生冲突，则后者数据优先
2. 生命周期钩子函数，合并为一个数组，前者优先执行
3. 值为对象的配置，如果键名发生冲突，则取后者的键值。methods、directives、components

mixin中定义的属性或方法的名称与组件中定义的名称没有冲突！
但是！！！！
生命周期函数：先执行mixin中生命周期函数中的代码，然后在执行组件内部的代码
data数据冲突：当mixin中的data数据与组件中的data数据冲突时，组件中的data数据会覆盖mixin中数据
methods中函数方法冲突：mixin和组件中都有printBaseMixin方法，但是最终在组件中调用时，实际调用的是组件中的printBaseMixin方法。

#### 全局混入

```js
// src/mixins/globalMixin.js

export const globalMixin = {
    data() {
      return {
        name:'globalMixin',
        isGlobalMixin:true,
        moreInfo:{
            position:"./mixins/globalMixin",
            isGlobalMixin:true,
        }
      };
    },
    beforeCreate(){
        console.log("this is a beforeCreate event in globalMixin");
    },
    created() {
        console.log("this is a created event in globalMixin");
    },
    beforeMount(){
        console.log("this is a beforeMount event in globalMixin");
    },
    methods: {
      helloWorld() {
        console.log("helloWorld in globalMixin");
      },
      printBaseMixin() {
        console.log("printBaseMixin in globalMixin");
      },
    },
  };
  
```

```js
// src/main.js
import Vue from 'vue'
import App from './App.vue'
import {globalMixin} from '@/mixins/globalMixin'
Vue.config.productionTip = false

Vue.mixin(globalMixin)

const app = new Vue({
  render: (h) => h(App),
})
app.$mount("#app");

```

### 全局mixin
Vue.mixin(xxxx)


mixin优缺点

- 优点：
- 提高代码复用性
- 无需传递状态
- 维护方便，只需要修改一个地方即可

缺点：

- 命名冲突
- 滥用的话后期很难维护
- 不好追溯源，排查问题稍显麻烦



### vue3不推荐使用mixin ，推荐组合式API


