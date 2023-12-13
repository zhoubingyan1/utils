### this的指向
- 普通函数:普通函数中this是动态的，在调用函数时确定，一般谁调用指向谁
调用方式this指向
普通函数调用 window
构造函数调用 实例对象
对象方法调用 该方法所属对象
事件绑定方法 绑定事件的对象
定时器函数window
立即执行函数 window
- 箭头函数:
箭头函数没有自己的this、arguments箭头函数的this是静态的，在定义函数时确定，一般和箭头函数所在父作用域的this指向一致

```js  
console.log(this) // window

function fn() {
    console.log(this) // window
}
fn()

// 法则 =>谁用就指向上一个调用者

var obj = {
    a:10,
    b:{
        a:12,
        fn:function() {
            console.log(this) //指向b
        }
    }
}
obj.b.fn()


var id=66
function fn1() {
    // 箭头函数没有作用域 没有this
    setTimeout(()=>{
        console.log(this.id) // 66
    })
}
fn1({id:22})

```