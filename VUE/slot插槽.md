### slot原理
slot本质就是函数！！

vue组件实例化顺序为：父组件状态初始化(data、computed、watch...) --> 模板编译 --> 生成render方法 --> 实例化渲染watcher --> 调用render方法，生成VNode --> patch VNode，转换为真实DOM --> 实例化子组件 --> ......重复相同的流程 --> 子组件生成的真实DOM挂载到父组件生成的真实DOM上，挂载到页面中 --> 移除旧节点

从上述流程中，可以推测出：
1.父组件模板解析在子组件之前，所以父组件首先会获取到插槽模板内容
2.子组件模板解析在后，所以在子组件调用render方法生成VNode时，可以借助部分手段，拿到插槽的VNode节点
3.作用域插槽可以获取子组件内变量，因此作用域插槽的VNode生成，是动态的，即需要实时传入子组件的作用域scope

整个插槽的处理阶段大致分为三步：
- 编译
- 生成渲染模板
- 生成VNode

<slot name="xxx" :xxx></slot>


### 默认
- 只有一个的就用默认的
```
<template v-slot:default></template>
```

### 具名

- 实名插槽可以有多个,在使用时必须使用name属性来标识
```

<template v-slot:header></template>
// 或者
<template #header></template>
```


### 作用域插槽

```
<template #:header='{user}'>
{{user.name}}
</template>
```

### 动态插槽

```
<template #[slotName]>

</template>
<script>
 export default {
    data(){
        return {
            slotName:'default'
        }
    }
 }
 </script>

```
