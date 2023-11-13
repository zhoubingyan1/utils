
```js
 // 构造函数 => 返回新创建的对象
 function Person(){
    // 属性
    this.name = '张三';
    this.age = 20;
    this.sayHello = function(){
        console.log('hello');
    }
 }

let person1 = new Person();

// new Person() new的过程是什么

// 1.创建一个空对象
var obj = new Object();
// 2. 设置它的原型链
obj.__proto__ = Person.prototype;
// 3.改变this的指向，并执行构造函数
let result=Person.call(obj);
// 4. 判断返回值类型
if(typeof result == 'object'){
    person1 = result;
}else{
    person1 = obj;
}
```

