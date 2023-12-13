### 手写Proxy
- 能动态修改属性 
```js
    const initData={
        value:1
    }

    const proxy= New Proxy(initData,{
        get:function(target,key,receiver){
            console.log('访问元素',key)
            return Reflect.get(target,key,receiver)
        },
        set:function(target,key,value,receiver){
            console.log('修改元素',key)
            return Reflect.get(target,key,value,receiver)
        }
    })
    proxy.value //'访问元素',value 1

    proxy.value=2  // '修改元素',value 2

```