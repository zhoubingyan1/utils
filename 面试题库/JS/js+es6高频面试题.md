1.js执行上下文
2.作用域和作用于链
3.对闭包的理解
4.原型和原型链
5.this指向
6.高阶函数
7.设计模式
8.js中的继承
9.call_apply
10.深浅拷贝
11.防抖和节流
12.浏览器如何渲染页面
13.对同步异步的理解解
14.事件循环机制 eventloop
15.js内存泄露
16.Set和Map
17.Promise
18.函数柯里化
19.js实现无限极目录树
20.js编译原理
21.js是如何实现异步的
22.js能否实现多线程
23.事件绑定_事件冒泡_事件代理
24.两个对象如何比较--有赞
25.变量提升
26.js如何进行垃圾回收
27.扩展运算符的作用及使用场景
28.JavaScript 脚本延迟加载的方式有哪些
29.如何理解ES6中Generator的,以及使用场景
30.Commonjs、AMD和CMD
31.New 操作符
32.如何实现sleep的效果
33.some和every
34.请用2种方式实现数组去重
35.找出多维数组的最大值
36.找出字符串中出现次数最多的字符
37.实现一个数组和对象的扁平化
38.js实现回文
39.冒泡和快排
40.瀑布流 (机试题)



### 深浅拷贝
深拷贝和浅拷贝是只针对Object和Array这样的引用数据类型的。
浅拷贝只复制指向某个对象的指针，而不复制对象本身，新旧对象还是共享同一块内存。但深拷贝会另外创造一个一模一样的对象，新对象跟原对象不共享内存，修改新对象不会改到原对象。
- 浅拷贝
1.Object.assign()
2.Array.prototype.concat()
3.Array.prototype.slice()
```js
    // 数组与对象的赋值都叫做浅拷贝
    let arr = [1,2,3,4,5];
    let newArr = arr;
    newArr.push(6);
    console.log(arr,newArr) //arr:[1,2,3,4,5,6] newArr:[1,2,3,4,5,6]

```

- 深拷贝
1.JSON.parse(JSON.stringify())
2.手写递归：递归方法实现深度克隆原理：遍历对象、数组直到里边都是基本数据类型，然后再去复制，就是深度拷贝
3.函数库lodash
该函数库也有提供_.cloneDeep用来做 Deep Copy

```js
// 标准的深拷贝 => 引用数据类型(str,array,object)

function deepClone(source){
    if (typeof obj !== 'object' || obj === null) {
        return obj;
    }
    // [] => Array(基类) {} =>Object
    const targetObj = source.constructor === Array ? [] : {};
    for(let key in source){
        if(source.hasOwnProperty(key)){
            // 引用数据类型
            if(source[key]&&typeof source[key]=== 'object'){
                targetObj[key]= source[key].constructor ===Array?[]:{}
                //递归
                targetObj[key]=deepClone(source[key])
            }else{
                // 基本数据类型，直接赋值
                targetObj[key]=source[key]
            }
        }
    }
    return targetObj;
}

```
```js
// 解构赋值是深拷贝还是浅拷贝
// 针对一维数组和对象是深拷贝 多维就是浅拷贝
let arr =[1,2,3,4,5]
let newArr = [...arr]
newArr.push(6)
console.log(arr,newArr)//arr:[1,2,3,4,5] newArr:[1,2,3,4,5,6]

let arr2 = [[1,2,3,],[4,5]]
let newArr2 = [...arr2]
newArr2[0].push(6)
console.log(arr2,newArr2) // arr2:[[1,2,3,6],[4,5]] newArr2:[[1,2,3,6],[4,5]]
```

