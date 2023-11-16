创建一个新对象，使用现有的对象来提供新创建的对象的_proto_
```js
function create(obj){
    function Fun(){

    }

    Func.prototype = obj;
    Func.prototype.constructor = Func;
    return new Fun();

}
```

