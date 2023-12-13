- 将js下划线专程驼峰形式 正则法
```js
function camelCase(str) {
  return str.replace(/_([a-z])/g, function(match, group1) {
    return group1.toUpperCase();
  });
}

console.log(camelCase("some_string"));  // "someString"


```
补充
```js
function camelCase(str) {
  return str.replace(/([-_])([a-z])/g, function(match, group1, group2) {
    return group2.toUpperCase();
  });
}

console.log(camelCase("some-string_with-underscores"));

```