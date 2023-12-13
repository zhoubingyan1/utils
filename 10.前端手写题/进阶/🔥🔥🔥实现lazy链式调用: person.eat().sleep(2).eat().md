### 🔥🔥🔥实现lazy链式调用: person.eat().sleep(2).eat()
- 解法其实就是将所有的任务异步化，然后存到一个任务队列里

```js
// 方法一
function Person() {
  this.queue = [];
  this.lock = false;
}

Person.prototype.eat = function () {
  this.queue.push(() => new Promise(resolve => { console.log('eat'); resolve(); }));
  // this.run();
  return this;
}

Person.prototype.sleep = function(time, flag) {
  this.queue.push(() => new Promise(resolve => {
    setTimeout(() => {
      console.log('sleep', flag);
      resolve();
    }, time * 1000)
  }));
  // this.run();
  return this;
}

Person.prototype.run = async function() {
  if(this.queue.length > 0 && !this.lock) {
    this.lock = true;
    const task = this.queue.shift();
    await task();
    this.lock = false;
    this.run();
  }
}

const person = new Person();
person.eat().sleep(1, '1').eat().sleep(3, '2').eat().run();

```


```js
class Lazy {
    // 函数调用记录，私有属性
    #cbs = [];
    constructor(num) {
        // 当前操作后的结果
        this.res = num;
    }

    // output时，执行，私有属性
    #add(num) {
        this.res += num;
        console.log(this.res);
    }

    // output时，执行，私有属性
    #multipy(num) {
        this.res *= num;
        console.log(this.res)
    }

    add(num) {

        // 往记录器里面添加一个add函数的操作记录
        // 为了实现lazy的效果，所以没有直接记录操作后的结果，而是记录了一个函数
        this.#cbs.push({
            type: 'function',
            params: num,
            fn: this.#add
        })
        return this;
    }
    multipy(num) {

        // 和add函数同理
        this.#cbs.push({
            type: 'function',
            params: num,
            fn: this.#multipy
        })
        return this;
    }
    top (fn) {

        // 记录需要执行的回调
        this.#cbs.push({
            type: 'callback',
            fn: fn
        })
        return this;
    }
    delay (time) {

        // 增加delay的记录
        this.#cbs.push({
            type: 'delay',

            // 因为需要在output调用是再做到延迟time的效果，利用了Promise来实现
            fn: () => {
                return new Promise(resolve => {
                    console.log(`等待${time}ms`);
                    setTimeout(() => {
                        resolve();
                    }, time);
                })
            }
        })
        return this;
    }

    // 关键性函数，区分#cbs中每项的类型，然后执行不同的操作
    // 因为需要用到延迟的效果，使用了async/await，所以output的返回值会是promise对象，无法链式调用
    // 如果需实现output的链式调用，把for里面函数的调用全部放到promise.then的方式
    async output() {
        let cbs = this.#cbs;
        for(let i = 0, l = cbs.length; i < l; i++) {
            const cb = cbs[i];
            let type = cb.type;
            if (type === 'function') {
                cb.fn.call(this, cb.params);
            }
            else if(type === 'callback') {
                cb.fn.call(this, this.res);
            }
            else if(type === 'delay') {
                await cb.fn();
            }
        }

        // 执行完成后清空 #cbs，下次再调用output的，只需再输出本轮的结果
        this.#cbs = [];
    }
}
function lazy(num) {
    return new Lazy(num);
}

const lazyFun = lazy(2).add(2).top(console.log).delay(1000).multipy(3)
console.log('start');
console.log('等待1000ms');
setTimeout(() => {
    lazyFun.output();
}, 1000);
```