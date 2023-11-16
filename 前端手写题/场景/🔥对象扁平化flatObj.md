- 多次遇到，建议背诵
```js
/* 题目*/
var entryObj = {
    a: {
        b: {
            c: {
                    dd: 'abcdd'
            }
        },
        d: {
            xx: 'adxx'
        },
        e: 'ae'
    }
}

// 要求转换成如下对象
var outputObj = {
	'a.b.c.dd': 'abcdd',
	'a.d.xx': 'adxx',
	'a.e': 'ae'
}

function flat(obj, path = '', res = {}, isArray) {
  for (let [k, v] of Object.entries(obj)) {
    if (Array.isArray(v)) {
      let _k = isArray ? `${path}[${k}]` : `${path}${k}`;
      flat(v, _k, res, true);
    } else if (typeof v === 'object') {
      let _k = isArray ? `${path}[${k}].` : `${path}${k}.`;
      flat(v, _k, res, false);
    } else {
      let _k = isArray ? `${path}[${k}]` : `${path}${k}`;
      res[_k] = v;
    }
  }
  return res;
}

console.log(flat({ a: { aa: [{ aa1: 1 }] } }))

```