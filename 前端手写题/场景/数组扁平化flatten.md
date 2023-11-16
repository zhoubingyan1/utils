- 递归
- reduce
- 扩展运算符
- toString,split
- es6 flat
- 正则和json，json.stringify

```js
function flatten(arr) {
  let result = [];
  for (let i = 0; i < arr.length; i++) {
    if (Array.isArray(arr[i])) {
      result = result.concat(flatten(arr[i]));
    } else {
      result.push(arr[i]);
    }
  }
  return result;
}

function flatten(arr) {
  return arr.reduce((p, c) => {
    return p.concat(Array.isArray(c) ? flatten(c) : c);
  }, [])
}

```