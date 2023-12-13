### new 的原理是什么?
- 新生成了一个对象
- 链接到原型 
- 绑定 this 
- 返回新对象

> 根据以上几个过程， 我们也可以试着来自己实现一个 new

- 创建一个空对象
- 获取构造函数
- 设置空对象的原型
- 绑定 this 并执行构造函数 确保返回值为对象

```js
function create() {
    let obj = {}
    let Con = [].shift.call(arguments) obj.__proto__ = Con.prototype
    let result = Con.apply(obj, arguments)
    return result instanceof Object ? result : obj
}
```