### 深入理解 Vue 单向数据流


之前看到 vue 文档出现这个单向数据流，就觉得有点纳闷，我擦，vue不是双向绑定的么，出现这个是什么鬼，看了文档里也说了两种情况修改 props 的处理方案。还是有点疑惑。所以有了这篇文章。

>从v-model开始讲起

1、v-model 用在 input 元素上

v-model在使用的时候很像双向绑定的（实际上也是。。。），但是 Vue 是单项数据流，v-model 只是语法糖而已：
```
<input v-model="something" />
<input v-bind:value="something" v-on:input="something = $event.target.value" />
```

第一行的代码其实只是第二行的语法糖。然后第二行代码还能简写成这样：
```
<input :value="something" @input="something = $event.target.value" />

```

要理解这行代码，首先你要知道 input 元素本身有个 oninput 事件，这是 HTML5 新增加的，类似 onchange ，每当输入框内容发生变化，就会触发 oninput ，通过 $event 把最新的 value 传递给 something。

> 我们仔细观察语法糖和原始语法那两行代码，可以得出一个结论： 在给 input 元素添加 v-model 属性时，默认会把 value 作为元素的属性，然后把 'input' 事件作为实时传递 value 的触发事件

2、v-model 用在组件上
v-model 不仅仅能在 input 上用，在组件上也能使用，拿官网上的demo看。
```
<currency-input v-model="price"></currency-input>
Vue.component('currency-input', {
  template: '\
    <span>\
      $\
      <input\
        ref="input"\
        v-bind:value="value"\
        v-on:input="updateValue($event.target.value)"\
      >\
    </span>\
  ',
  props: ['value'], // 为什么这里要用 value 属性，value在哪里定义的？
  methods: {
    // 不是直接更新值，而是使用此方法来对输入值进行格式化和位数限制
    updateValue: function (value) {
      var formattedValue = value
        // 删除两侧的空格符
        .trim()
        // 保留 2 位小数
        .slice(
          0,
          value.indexOf('.') === -1
            ? value.length
            : value.indexOf('.') + 3
        )
      // 如果值尚不合规，则手动覆盖为合规的值
      if (formattedValue !== value) {
        this.$refs.input.value = formattedValue
      }
      // 通过 input 事件带出数值
      // <!--为什么这里把 'input' 作为触发事件的事件名？`input` 在哪定义的？-->
      this.$emit('input', Number(formattedValue))
    }
  }
})

```

如果你知道这两个问题的答案，那么恭喜你真正掌握了 v-model，如果你没明白，那么可以看下这段代码：

```
<currency-input v-model="price"></currency-input>
所以在组件中使用时，它相当于下面的简写：
//上行代码是下行的语法糖
<currency-input :value="price" @input="price = arguments[0]"></currency-input>

```

所以，给组件添加 v-model 属性时，默认会把 value 作为组件的属性，然后把 'input' 值作为给组件绑定事件时的事件名。这在写组件时特别有用。

3、v-model 的缺点和解决办法

在创建类似复选框或者单选框的常见组件时，v-model就不好用了。
```
<input type="checkbox" v-model="something" />
```

v-model 给我们提供好了 value 属性和 oninput 事件，但是，我们需要的不是 value 属性，而是 checked 属性，并且当你点击这个单选框的时候不会触发 oninput 事件，它只会触发 onchange 事件。
>因为 v-model 只是用到了 input 元素上，所以这种情况好解决：

```
<input type="checkbox" :checked="value" @change="change(value, $event)"

```
当 v-model 用到组件上时：
```
<checkbox v-model="value"></checkbox>

Vue.component('checkbox', {
  tempalte: '<input type="checkbox" @change="change" :checked="currentValue"/>'
  props: ['value'],
  data: function () {
        return {
            //这里为什么要定义一个局部变量，并用 prop 的值初始化它。
            currentValue: this.value
        };
    },
  methods: {
    change: function ($event) {
      this.currentValue = $event.target.checked;
      this.$emit('input', this.currentValue);  
    }
})

```

在 Vue 2.2 版本，你可以在定义组件时通过 model 选项的方式来定制 prop/event 。

4、vue 组件数据流

从上面 v-model 的分析我们可以这么理解，双向数据绑定就是在单向绑定的基础上给可输入元素（input、textare等）添加了 change(input) 事件，来动态修改 model 和 view ，即通过触发（$emit）父组件的事件来修改mv来达到 mvvm 的效果。而 vue 组件间传递数据是单向的，即数据总是由父组件传递到子组件，子组件在其内部可以有自己维护的数据，但它无权修改父组件传递给它的数据，当开发者尝试这样做的时候，vue 将会报错。这样做是为了组件间更好的解耦，在开发中可能有多个子组件依赖于父组件的某个数据，假如子组件可以修改父组件数据的话，一个子组件变化会引发所有依赖这个数据的子组件发生变化，所以
    vue 不推荐子组件修改父组件的数据，直接修改 props 会抛出警告。流程图如下：

所以，当你想要在子组件去修改 props 时，把这个子组件当成父组件那样用，所以就有了

1、定义一个局部变量，并用 prop 的值初始化它。
2、定义一个计算属性，处理 prop 的值并返回。