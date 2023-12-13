
- 这里简单实现一下，可以参考一下其他的Promise A+规范的实现，主要包含then,all,race

- then
```js
// 三个常量用于表示状态
const PENDING = 'pending';
const RESOLVED = 'resolved';
const REJECTED = 'rejected';

function MyPromise(fn) {
  const self = this;
  this.state = PENDING;
  // value 变量用于保存 resolve 或者 reject 中传入的值
  this.value = null;
  this.reason = null;
  // 用于保存 then 中的回调， 因为当执行完 Promise 时状态可能还是等待中， 这时候应该把
  this.resolvedCallbacks = [];
  this.rejectedCallbacks = [];

  function resolve(value) {
    if (value instanceof MyPromise) {
      value.then(resolve, reject)
    }
    // 保证代码执行顺序为本轮事件循环的末尾
    setTimeout(() => {
      // 首先两个函数都得判断当前状态是否为等待中
      if (self.state === PENDING) {
        self.state = RESOLVED;
        self.value = value;
        // 遍历回调数组并执行
        self.resolvedCallbacks.forEach(cb => cb(value));
      }
    }, 0)
  }

  function reject(reason) {
    setTimeout(() => {
      if (self.state === PENDING) {
        self.state = REJECTED;
        self.reason = reason;
        self.rejectedCallbacks.forEach(cb => cb(reason));
      }
    }, 0)
  }
  // 完成以上两个函数以后，我们就该实现如何执行 Promise 中传入的函数了
  try {
    fn(resolve, reject);
  } catch (e) {
    reject(e);
  }
}
// 最后我们来实现较为复杂的 then 函数
MyPromise.prototype.then = function (onFulfilled, onReject) {
  const self = this;
  return new MyPromise((resolve, reject) => {
    let fulfilled = () => {
      try {
        const result = onFulfilled(self.value);
        return result instanceof MyPromise ? result.then(result) : resolve(result);
      } catch (e) {
        reject(e);
      }
    };
    let rejected = () => {
      try {
        const result = onReject(self.reason);
        return result instanceof MyPromise ? result.then(resolve, reject) : reject(result);
      } catch (e) {
        reject(e);
      }
    }
    switch (self.state) {
      case PENDING:
      case RESOLVED:
      case RESOLVED:

    }
  })
}

MyPromise.all = (promises) => {
  return new MyPromise((resolve, reject) => {
    if (!Array.isArray(promises)) {
      throw new TypeError('arguments must be array');
    }
    let resolvedCounter = 0;
    let promiseNum = promises.length;
    let resolvedResult = [];

    for (let i = 0; i < promises.length; i++) {
      MyPromise.resolve(promises[i]).then(value => {
        resolvedCounter++;
        resolvedResult[i] = value;
        if (resolvedCounter === promiseNum) {
          return resolve(resolvedResult);
        }
      }, error => {
        return reject(error);
      })
    }
  })
}
MyPromise.race = function(args) {
  return new Promise((resolve, reject) => {
    for(let i = 0; len = args.length; i++) {
      args[i].then(resolve, reject);
    }
  })
}

```


