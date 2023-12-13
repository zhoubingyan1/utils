1. 说说你对 setup 的理解
  a. 组合式 API 的入口
2. ref 和 reactive 有什么区别？
  a. ref 内部使用了 reactive
    >export const toReactive = <T extends unknown>(value: T): T =>
    >isObject(value) ? reactive(value) : value
ref(obj) === reactive({ value: obj })
3. ref 和 shallowRef 的区别，以及 reactive 和 shallowReactive 的区别？
  a. shallow 表示浅层，这里均是指的响应值作用在第一层，即 .value，不过我们可以使用 triggerRef(xxx) 来在深层内容变更后，手动触发更新，需要注意的是 shallowReactive 没有对应方法
4. watchEffect 与 watch 的区别
  a. 懒执行副作用；
  b. 更加明确是应该由哪个状态触发侦听器重新执行；
  c. 可以访问所侦听状态的前一个值和当前值