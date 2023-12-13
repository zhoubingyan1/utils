### 实现new关键字

1.创建一个空对象
2.设置原型:将空白对象的原型设置为函数的prototype对象
3.让函数的this指向这个对象，执行构造函数的代码（为这个空白对象添加属性）
4.判断函数的返回值
4.1 如果是引用类型，直接返回，比如构造函数主动返回了一个对象：function T(){return {x: 1}}
4.2 如果不是引用类型，返回空白对象；比如构造函数返回一个数字：function T(){return 1}
```js
//  调用方法：objectFactory(构造函数，构造函数的参数)
function objectFactory() {
  let newObject = null;
  let constructor = Array.prototype.shift.call(arguments);
  let result = null;
  if (typeof constructor !== 'function') {
    console.error('type error');
    return
  }
  newObject = Object.create(constructor.prototype);
  result = constructor.apply(newObject, arguments);
  let flag = result && (typeof result === 'function' || typeof result === 'object');
  return flag ? result : newObject;
}

```
