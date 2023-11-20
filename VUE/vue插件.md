### vue插件
能作什么
- 自定义过滤器 Vue.filter()
- 自定义组件 Vue.component()
- 自定义混入 Vue.mixin()
- 自定义实例方法
- 等



```js
// src/plugins/myplugin.js
export const myplugin={
    install(Vue,options){
        console.log(options) //{name:'xxxx'}
    }
}
//调用
// 在main.js 引入

import {myplugin} from '@/plugins/myplugin'
Vue.use(myplugin,{
    name:'xxxx'
})


```