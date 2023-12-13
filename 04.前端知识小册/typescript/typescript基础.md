### typescript基础

- 类型
    - 数字 number
    - 字符串 string
    - symbol
    - 布尔值 boolean
    - null
    - undefined

    - Date
    - 数组 array
    - 对象
    - 函数

    - Regexp

    
```
let a:string='1'
let b:number=2

let flag:boolean=true

let date:Date=new Date()


// 有些类型就是一个容器，比如定义一个字符串数组或者定义一个数字数组
// 通过泛型来取解决这个问题，泛型中的类型用来取决你这个容器中的类型

let arr:Array<number>=[1,2,3]


// ts 中的几个特殊类型

// any 没有对类型形成严格约束


// never

// unkonown




```

### any、unknown、never、void的区别

1.any：在编程中，特别是在TypeScript中，any类型可以接受任何类型的值。使用any类型可以避免在编译时出现类型错误，但需要注意的是，使用any会降低代码的类型安全性。
2.unknown：在JavaScript中，unknown类型表示一个未知的值。它通常用于函数或方法的参数，表示该参数可以是任何类型。使用unknown类型可以提高代码的健壮性和可读性。
3.never：在TypeScript中，never类型表示一个永不可能出现的值。例如，一个永远抛出异常的函数将返回never类型。另外，如果一个函数永远不会返回（例如，因为它在执行过程中引发了一个异常），那么该函数的返回类型也是never。
4.void：在TypeScript中，void类型表示“无类型”，即该类型的值永远是空或不存在。void类型主要用于函数的返回类型，表示该函数不返回任何值。

-总的来说，any、unknown、never和void都是用于处理不确定或不存在类型的关键字，但它们的使用场景和效果有所不同。any和unknown主要用于处理未知或不确定的类型，但any会禁用类型检查，而unknown不会；never表示永不存在的值，主要用于函数的返回类型；void表示无类型，主要用于函数的返回类型，表示函数不返回任何值。



### ts 自定义类型 type（自定义类型）、interface（接口）、typeof（类型推断）、keyof（枚举对象属性）

```
// 自定义类型
type Person={
    name:string
    age:number
    hobby?:unknown
}
```