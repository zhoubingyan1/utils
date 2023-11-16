### 获取URL中的参数
这里主要还是正则表达式的设计

/?&/igm，前面是？或者&，任意字符直到遇到=，使用非贪婪模式，等号后面是非&符号的任意字符，然后去匹配就好了
理论上可以用matchAll，然后用迭代器去处理

```js
function name(url) {
    const _url = url || window.location.href;
    const _urlParams = _url.match(/[?&](.+?=[^&]+)/igm);
    return _urlParams ? _urlParams.reduce((a,b) => {
        const value = b.slice(1).split('=');
        a[value[0]] = value[1];
        return a;
    }, {}) : {} 
    
}

```