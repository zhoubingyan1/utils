### 🔥带并发限制的promise异步调度器

```js
function taskPool() {
  this.tasks = [];
  this.pool = [];
  this.max = 2;
}

taskPool.prototype.addTask = function(task) {
  this.tasks.push(task);
  this.run();
}

taskPool.prototype.run = function() {
  if(this.tasks.length === 0) {
    return;
  }
  let min = Math.min(this.tasks.length, this.max - this.pool.length);
  for(let i = 0; i<min;i++) {
    const currTask = this.tasks.shift();
    this.pool.push(currTask);
    currTask().finally(() => {
      this.pool.splice(this.pool.indexOf(currTask), 1);
      this.run();
    })
  }
}


```