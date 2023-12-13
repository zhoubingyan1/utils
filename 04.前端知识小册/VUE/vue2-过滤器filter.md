### VUE-过滤器filters
- vue3已经去除过滤器filters,methods、computed可替代

### 局部（当前组件引入filters）
- 可以串联
- 也可以接收参数 
```js
<template>
    <div>
        {{ name|capitalize|capitalizeLastWord }}
        串联 
        <br />
        {{ name|capitalize|capitalizeLastWord1('!') }}

    </div>
</template>
<script>


export default {
    data(){
        return {
            name: 'Hello World'
        }
    },
    filters:{
        capitalize(value){
            // 首字母大写
            if (!value) return ''
            value = value.toString()
            return value.charAt(0).toUpperCase() + value.slice(1)
        },
        capitalizeLastWord(value){
            //最后一个字母大写
            return value.slice(0,-1)+value.slice(-1).toUpperCase()
        },
        capitalizeLastWord1(value,suffix){
            //最后一个字母大写+接收参数
            return value.slice(0,-1)+value.slice(-1).toUpperCase()+suffix
        }
    }
}
</script>

```
### 全局引入过滤器
```js
// main.js
Vue.filter('capitalize',function(value){
// 首字母大写
            if (!value) return ''
            value = value.toString()
            return value.charAt(0).toUpperCase() + value.slice(1)
})
```